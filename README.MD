# Prometheus CloudWatch Exporter

This project sets up a Prometheus CloudWatch Exporter to monitor various AWS EC2 metrics.

## Prerequisites

- Docker (not required but can be used to run prometheus and grafana if used)
- AWS CLI configured with appropriate permissions <br/>
   **_NOTE:_** Admin used for proof of concept
- Prometheus (used to pull the metrics and present to dashboarding tool)

## Configuration

Create an IAM user that will be used to scrapte the metrics. Gather the AWS keys and add to the AWS CLI using the ```aws configure``` command. 

**__Example__**:
```
AWS Access Key ID [YOUR_ACCESS_KEY_ID]: <AWS_ACCESS_KEY_HERE>
AWS Secret Access Key [YOUR_SECRET_ACCESS_KEY]: <AWS_SECRET_ACCESS_KEY_HERE>
Default region name [YOUR_DEFAULT_REGION]: <REGION>
Default output format [NONE]: json
```

The configuration template was used to monitor EC2 instance metrics in the `us-east-1` region (```example.yml``` file in the project):

## Running the CloudWatch Exporter
1. Clone the repository:
    ```
    git clone https://github.com/prometheus/cloudwatch_exporter.git
    cd cloudwatch_exporter
    ```
    **__Note__**: this project downloaded a JAR file noted in the main project (below)
2. Build the Docker image:
    ```
    docker build -t cloudwatch_exporter .
    ```

3. Run the Docker container:
    ```
    docker run -d -p 9106:9106 -v $(pwd)/config.yml:/config/config.yml cloudwatch_exporter
    ```

4. Verify the exporter is running: Open your browser and navigate to http://localhost:9106/metrics. You should see the metrics being exported.

## Configuring Prometheus
Add the following job to your Prometheus configuration:

```
scrape_configs:
  - job_name: 'cloudwatch_exporter'
    static_configs:
      - targets: ['localhost:9106']
```

## Conclusion
You have successfully set up the Prometheus CloudWatch Exporter to monitor AWS EC2 metrics. You can now visualize these metrics in Prometheus and Grafana.

## References

- [Prometheus AWS Exporter and Grafana](https://dzone.com/articles/prometheus-aws-exporter-amp-grafana)
- [Importing your Cloudwatch Metrics into Prometheus](https://coralogix.com/blog/import-cloudwatch-metrics-prometheus/)
- [prometheus/cloudwatch_exporter](https://github.com/prometheus/cloudwatch_exporter?tab=readme-ov-file)
- [cloudwatch_exporter/examples/EC2.yml](https://github.com/prometheus/cloudwatch_exporter/blob/master/examples/EC2.yml)