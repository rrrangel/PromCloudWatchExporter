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

## Running the CloudWatch Exporter in AWS EC2 instance (Process that I followed)
1. Update the instance and install openJDK:
    ```
    sudo apt update
    sudo apt install -y openjdk-11-jdk
    java -version
    ```
2. Configure AWS CLI:
    ```
    sudo apt  install awscli
    aws configure
    ```

3. Download JAR and example.yml; update example.yml **__Note__**: this project downloaded a JAR file noted in the main project (below):
    ```
    wget https://repo1.maven.org/maven2/io/prometheus/cloudwatch/cloudwatch_exporter/0.16.0/cloudwatch_exporter-0.16.0-jar-with-dependencies.jar
    wget https://raw.githubusercontent.com/prometheus/cloudwatch_exporter/cbe73e796998012504485bc3f01298b6403d4615/example.yml
    vi example.yml
    ```

4. Run the jar file pointing to the example.yml and use the default port (9601):
    ```
    java -jar cloudwatch_exporter-0.6.0-jar-with-dependencies.jar 9601 example.yml
    ```

5. Verify the exporter is running: Open your browser and navigate to http://localhost:9106/metrics. You should see the metrics being exported.

## Configuring Prometheus
I did not use Prometheus but if you want to use prometheus, you can add the following job to your Prometheus configuration (change localhost to the host running CloudWatch Exporter):

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