# aws-practices

## Monitor AWS Services Logs & Metrics


Amazon CloudWatch and AWS X-Ray are both observability tools, 
but they serve different purposes and complement each other beautifully when used together.

### Core Difference

    Feature	                        Amazon CloudWatch	                                AWS X-Ray
    -------------------------------------------------------------------------------------------------------------------------------
    Purpose	                        Infrastructure & service monitoring	                Distributed tracing & request path analysis
    
    Data Type	                    Metrics, logs, events, alarms	                    Traces, segments, service maps
    
    Best For	                    CPU, memory, latency, error rates	                Debugging request flow across microservices
    
    Granularity	                    System-level	                                    Application-level (end-to-end request tracing)
    
    Integration	                    All AWS services	                                Lambda, API Gateway, ECS, EKS, RDS, etc.
    
    Visualization	                Dashboards, alarms, log insights	                Service maps, trace timelines, latency graphs
    
    Pricing Model	                Per metric/log/retention/alarm	                    Per trace recorded/retrieved/scanned
    
### Use Case Comparison

🔹 CloudWatch

        Monitor EC2 CPU, Lambda duration, DynamoDB throttles
        
        Set alarms (e.g., RDS CPU > 80%)
        
        Visualize metrics and logs in dashboards
        
        Aggregate logs from multiple services

🔹 X-Ray

        Trace a single request across Lambda → API Gateway → RDS
        
        Identify latency bottlenecks and retry loops
        
        Visualize service dependencies and error hotspots
        
        Sample traces intelligently to reduce cost

### How They Work Together
        
        can correlate CloudWatch metrics and logs with X-Ray traces to get a full picture:
        
        CloudWatch shows what is happening (e.g., high latency)
        
        X-Ray shows why it’s happening (e.g., slow DB query in a specific trace)

AWS even integrates X-Ray with CloudWatch Application Signals and RUM to give you end-to-end observability from user to backend


**Summary:**

        Use CloudWatch for infrastructure and system health, and 
        
        X-Ray for debugging and optimizing request flows—especially in microservices and serverless apps.


To monitor your EKS, AWS Lambda, S3, RDS, and DynamoDB using Amazon CloudWatch, 
the monthly cost depends on log ingestion, metrics, dashboards, and alarms.

Here's a tailored estimate for a production-grade setup like yours:
    
### Estimated Monthly CloudWatch Cost Breakdown

        AWS Service	        Monitoring Type	                       Est. Log Volume	        Cost Estimate (USD)
        
        Amazon EKS	        Container Insights + Metrics	       20 GB	                  ~$46.00
        AWS Lambda	        Logs + Metrics (Tiered)	               10 GB	                  ~$5.00
        Amazon S3	        Replication + Access Logs	           5 GB	                      ~$2.50
        Amazon RDS	        Enhanced Monitoring + Logs	           10 GB	                  ~$23.00
        DynamoDB	        Metrics + Throttling Logs	           5 GB	                      ~$2.50
        Dashboards	        3 Custom Dashboards	                   —	                      Free (within limits)
        Alarms	            10 Standard Alarms	                   —	                      ~$1.00
        
    Total Estimated Monthly Cost: ~$80–$85



Assumes moderate production usage with 30-day retention and standard resolution metrics. 
Pricing varies slightly by region.

### Optimization Tips
      
      Use tiered pricing for Lambda logs (starts at $0.50/GB, drops with volume)
      
      Set log retention policies to avoid indefinite storage
      
      Filter noisy metrics (e.g., reduce EKS pod-level verbosity)
      
      Use S3 or Firehose as alternate log destinations for Lambda to reduce CloudWatch costs
      
      Tag resources for cost attribution in Cost Explorer

### Monitoring Strategy
      
      You can build a unified CloudWatch dashboard or use CloudWatch Logs Insights to:
      
      Track ingestion volume per service
      
      Visualize latency, error rates, and throttling

Set alarms for anomalies (e.g., RDS CPU > 80%, Lambda duration spikes)
