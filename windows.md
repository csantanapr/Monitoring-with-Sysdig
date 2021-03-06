---

copyright:
  years:  2018, 2020
lastupdated: "2020-05-12"

keywords: Sysdig, IBM Cloud, monitoring, windows

subcollection: Monitoring-with-Sysdig

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}


# Monitoring a Windows environment
{: #windows}

The standard Sysdig agent cannot be installed on a Windows platform. In order to monitor a Windows system with {{site.data.keyword.mon_full_notm}}, you can leverage the Prometheus WMI Exporter to perform the collection of the metrics on the system.
{:shortdesc}

Once the metrics are collected you have two options for publishing the metrics to Sysdig, remotely scraping the metrics with a Linux Sysdig agent,or pushing from a local instance of Prometheus using remote write. Step 3 will cover these two options, but step 1 and 2 are the same regardless of how the metrics are sent.

Complete the following steps to configure the following Windows images to send metrics to a Sysdig instance:
* Windows Server 2019 Standard Edition (64 bit)
* Windows Server 2016 Standard Edition (64 bit)
* Windows Server 2012 Standard Edition (64 bit)
* Windows Server 2012 R2 Standard Edition (64 bit)

## Step 1. Configure the Prometheus WMI exporter
{: #windows_step1}

Configure the Prometheus WMI exporter to collect Windows system metrics.

The Prometheus WMI exporter runs as a Windows service. You configure the metrics that you want to monitor by enabling collectors. 

The following collectors are supported:
* CPU
* Computer system metrics (cs)
* Disk metrics
* Network interface metrics


Complete the following steps to configure the Prometheus WMI exporter in your Windows system:

1. Login to your Windows system, for example, you can connect via remote desktop (RDP).

2. [Download the Prometheus exporter ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/martinlindhe/wmi_exporter/releases){:new_window}.

3. Identify the collectors that include data for the metric data that you want to send to Sysdig.  

4. Run the `wmi_exporter` and configure the collectors that you want to enable.

    ```
    .\wmi_exporter-0.10.2-amd64.exe --collectors.enabled <COLLECTORS> 
    ```
    {: codeblock}

    Where `<COLLECTORS>` indicates the list of connectors that you want to configure.

    For example, to collect computer system metrics (cs), CPU metrics, disk metrics and network interface I/O metrics, see the following example:

    ```
    .\wmi_exporter-0.10.2-amd64.exe --collectors.enabled "cpu,cs,logical_disk,net"
    ```
    {: codeblock}





## Step 2. Configure network settings
{: #windows_step2}

Complete the following steps:
1. Enable the Windows firewall to allow access to `wmi_exporter-0.10.2-amd64.exe`.

2. [Optional] Update the VPC rules

    If you use private endpoints, add an inbound rule to the security group for port `9182` with `source type = Security Group` and choose the security group for the Windows system.


## Step 3. Choose the method to collect metrics
{: #windows_step3}


### Option 1: Run a Sysdig agent on a Linux system and remotely scrape the Windows endpoint
{: #windows_step3_1}

Complete the following steps:

1. [Install the Sysdig agent on a Linux node](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-config_agent#config_agent_linux).

    You can collect a maximum of 3000 time series per Sysdig Linux agent. If you need to collect more than 3000 time series for all your Windows systems, you need more than one Linux agent.
    {: important}
    
2. Update the `/opt/draios/etc/dragent.yml` to [enable remote scraping ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.sysdig.com/en/collecting-prometheus-metrics-from-remote-hosts.html){:new_window}. 

    ```yaml
    prometheus:
        enabled: true
        interval: 30
        log_errors: true
        max_metrics: 3000
        max_metrics_per_process: 3000
        max_tags_per_metric: 20
        remote_services:
            - <HOSTNAME>:
                always: true
                conf:
                    url: "http://<WINDOWS_IP>:9182/metrics"
                    tags:
                        region: us-east
                        instance: <HOSTNAME>
                        job: <JOBNAME>
    ```
    {: codeblock}

    Where

    `<WINDOWS_IP>` is the IP of the Windows system

    `<HOSTNAME>` is the name of the Windows system

    `<JOBNAME>` is a custom attribute that you can set to identify the role of the node that you are scraping, and you can also use to scope the data in Sysdig

    See the following sample configuration that you can set to enable scraping for a Windows system with hostname `my-windows-hostname` in your environment:
 
    ```yaml
    prometheus:
        enabled: true
        interval: 30
        log_errors: true
        max_metrics: 3000
        max_metrics_per_process: 3000
        max_tags_per_metric: 20
        remote_services:
            - my-windows-hostname:
                always: true
                conf:
                    url: "http://10.245.0.5:9182/metrics"
                    tags:
                        region: us-east
                        instance: my-windows-hostname
                        job: my-job-name
    ```
    {: screen}

3. Configure the Sysdig agent to reduce the number of metrics that are collected by the Windows wmi_exporter. 

    You can configure the `metrics_filter` section to remove metrics. For example, you can remove collector metrics. You can also remove specific metrics that you do not wish to collect.

    For example, to remove go metrics and the logical disk request metrics, you can set the section in the following way:

    ```yaml
    metrics_filter:
      - exclude: go_*
      - exclude: wmi_logical_disk_requests_queued
    ```
    {: codeblock}



### Option 2: Use the Prometheus remote-write capabilities to push the metrics from the Windows system by running Prometheus as a client collector on Windows
{: #windows_step3_2}

Complete the following steps:

1. Download the Prometheus monitoring system and time series database. [Download prometheus-2.15.2.windows-amd64.tar.gz](https://prometheus.io/download/) 

2. Unzip the file `prometheus-2.15.2.windows-amd64.tar.gz`. 

3. Edit the `prometheus.yml` file. For example, you can edit it with notepad. 

4. Configure the `scrape_configs` section of `prometheus.yml` configuration file as follows to have prometheus scrape the Windows wmi_exporter.

    ```
    scrape_configs:
      # The job name is added as a label `job=<job_name>` to any timeseries scraped from this configuration.
      - job_name: 'wmi_exporter'

        static_configs:
         - targets: ['localhost:9182']

         labels:
           region: us-east
           instance: <HOSTNAME>
           job: <JOBNAME>
    ```
    {: codeblock}

    Where

    `<HOSTNAME>` is the name of the Windows system

    `<JOBNAME>` is a custom attribute that you can set to identify the role of the node that you are scraping, and you can also use to scope the data in Sysdig

    Next, you will add the `remote_write` configuration to the end of the `prometheus.yml` file to configure the target Sysdig instance that will receive the metrics.

    ```yaml
    remote_write:
      - url: "ENDPOINT/api/prometheus/write"
  
        bearer_token_file: C:\Users\Administrator\prom\sysdig-apikey

        write_relabel_configs:
          # Drop forwarding the metrics generated by the exporter that are not supported
          - source_labels: ["__name__"]
            regex: "^wmi_(.*)"
            action: keep

          - regex: "(__name__)|(job)|(region)|(instance)|(status)|(core)|(name)|(start_mode)|(nic)|(volume)|(state)|(version)|(mode)|(branch)|(timezone)|(goversion)|(collector)|(revision)"
            action: labelkeep
    ```
    {: codeblock}

    Where 
    
    `ENDPOINT` is the Sysdig collector endpoint. To see the list of endpoints, see [Sysdig collector endpoints](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-endpoints#endpoints_ingestion).

    `sysdig-apikey` is the file that contains the **Sysdig Monitor API Token**. Notice that the file name does not have an extension.  For more information about how to get the API token, see [Getting the Sysdig API token](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-api_token#api_token_get).

    For example, the completed version of the prometheus.yml could look like :
  
    ```yaml
    # my global config
    global:
      scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
      evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
      # scrape_timeout is set to the global default (10s).

    # Alertmanager configuration
    alerting:
      alertmanagers:
      - static_configs:
        - targets:
          # - alertmanager:9093

    # Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
    rule_files:
      # - "first_rules.yml"
      # - "second_rules.yml"

    # A scrape configuration containing exactly one endpoint to scrape:
    # Here it's Prometheus itself.
    scrape_configs:
      # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
      - job_name: 'wmi_exporter'

        static_configs:
        - targets: ['localhost:9182']

          labels:
            instance: "my-windows-hostname"
            region: "us-south"

    # Connection to sysdig 
    remote_write:
      - url: "https://ingest.eu-gb.monitoring.cloud.ibm.com/api/prometheus/write"

        bearer_token_file: C:\Users\Administrator\prom\sysdig-api

        write_relabel_configs:
          - source_labels: ["__name__"]
            regex: "^wmi_(.*)"
            action: keep

          - regex: "(__name__)|(job)|(region)|(instance)|(status)|(core)|(name)|(start_mode)|(nic)|(volume)|(state)|(version)|(mode)|(branch)|(timezone)|(goversion)|(collector)|(revision)"
            action: labelkeep
    ```
    {: codeblock}

5. Start the Prometheus executable from the location containing the `prometheus.yml` file. Run `.\prometheus.exe`.


## Step 4. Monitor Windows system metrics
{: #windows_step4}

To monitor Windows systems metrics, you can use the default dashboard `Windows Node Overview` to view the Windows metrics. This default dashboard is located in the section **Hosts and Containers.**


## Step 5. [Optional] Verifying uptime for Windows with Prometheus Blackbox exporter
{: #windows_step5}

You can configure the Prometheus Blackbox exporter to get information about the availability of a Windows system.

The Prometheus Blackbox exporter can be run as an application or a docker container from a Linux system in conjunction with the Sysdig agent.

The Prometheus Blackbox exporter allows blackbox probing of endpoints over HTTP, HTTPS, DNS, TCP and ICMP.  The Sysdig agent can be used in conjunction with the Blackbox exporter to collect availability metrics. The availability metrics can then be alerted upon within Sysdig to alert users on the availability of the endpoints.

The exporters are available as [binary releases ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/prometheus/blackbox_exporter/releases){:new_window}, as a [docker container ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://hub.docker.com/r/prom/blackbox-exporter/){:new_window} or the [code is available in github ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/prometheus/blackbox_exporter){:new_window}.

For example, complete the following instructions when the container approach is used:

1. Download the [blackbox.yml file ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/prometheus/blackbox_exporter/blob/master/blackbox.yml){:new_window} from Github.

2. Start the blackbox exporter.

    ```
    docker run --rm -d -p 9115:9115 --name blackbox_exporter -v `pwd`:/config/ prom/blackbox-exporter:master --config.file=/config/blackbox.yml
    ```
    {: codeblock}

3. Test the blackbox exporter is working by manually running the probe to test your Windows system.  

    For example, you can do a simple `icmp` check to see if the system is responding. See the [documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/prometheus/blackbox_exporter/blob/master/README.md){:new_window} for other options. 

    ```
    curl 'http://localhost:9115/probe?module=icmp&target=<system ip>'
    ```
    {: codeblock}

    where `<system ip>` is the IP of the host you are checking.

    You should get a payload back with `probe_success 1` as the last line to indicate that the system at `<system ip>` is up.

3. Update the `/opt/draios/etc/dragent.yaml` to enable `probe_success` metrics.

    You must add details about the check that you want to run on the call. 

    ```yaml
    prometheus:
        enabled: true
        interval: 30
        log_errors: true
        max_metrics: 3000
        max_metrics_per_process: 3000
        max_tags_per_metric: 20
        remote_services:
            - blackbox_win_1:
                always:
                conf:
                url: "http://localhost:9115/probe?module=icmp&target=10.240.0.5"
                tags:
                    service: windows_uptime
                    windows_hostname: windows-test-01
    ```
    {: codeblock}

    When you enable this option, you can segment data by `windows_hostname` and build alerts upon this metric.




