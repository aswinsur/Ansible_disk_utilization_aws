***Summary***:

This solution addresses the need to monitor disk utilization across multiple AWS accounts using Ansible. It leverages AWS Organizations for centralized account management and Amazon S3 for centralized data storage. The process involves running an Ansible playbook that assumes roles in each account, collects disk utilization data via Ansible, and uploads the results to a central S3 bucket. Amazon Athena can then be used to query and analyze this data, with the option to connect a visualization tool for reporting. This approach is scalable, allowing for easy integration of new accounts as the company grows through acquisitions.

***Proposed Architetcure*** 

<img title="a title" alt="Alt text" src="https://github.com/aswinsur/Ansible_disk_utilization_aws/blob/main/architecture_lucidity.drawio.png">

The provided architectural diagram outlines a solution for centralized monitoring and reporting of disk utilization across multiple AWS accounts using various AWS services.

1. Centralized Access and Management:
   - The "Management Account" can be set up as the primary account within AWS Organizations. This account will have central control and visibility over the other child accounts (Account 1 and Account 2).
   - AWS Organizations provides features like Service Control Policies (SCPs) and centralized logging and monitoring, which can be leveraged for consistent governance and compliance across all accounts.
   - Alternatively, cross-account access can be established using AWS Identity and Access Management (IAM) roles and permissions. This approach involves creating IAM roles in the child accounts and granting the Management Account permissions to assume those roles.

2. Data Collection and Aggregation:
    The solution proposes the following flow for data aggregation:
    a. Ansible playbooks running on an  EC2 instance in the Organization's management account fetch disk utilization metrics and store them in Amazon S3 buckets.
    b. Please find the attached file diskutil.yml for the ansible code. 
    c. AWS Glue crawls the S3 bucket and creates a unified data catalog.
    c. Amazon Athena can then query the data catalog to analyze and aggregate the disk utilization metrics across all accounts.
    d. The query results can be visualized using Amazon QuickSight for easy consumption.
4. Data Visualization and Reporting:
   - The query results from Amazon Athena can be visualized using Amazon QuickSight, a cloud-native business intelligence service.
   - QuickSight provides a wide range of visualization options, including charts, graphs, and dashboards, making it easy to present the disk utilization metrics in a digestible format.
   - QuickSight can be integrated with Athena, allowing users to create visualizations directly from the Athena query results.

5. Scalability and Future Growth:
   - As the company acquires more companies and AWS accounts, the new accounts can be added to the AWS Organizations structure or granted cross-account access to the Management Account.
   - The Ansible playbooks can be easily extended to the new EC2 instances in the additional accounts, ensuring consistent data collection and uploading to the respective S3 buckets.
   - AWS Glue, Athena, and QuickSight are fully managed services that can automatically scale to handle increased data volumes and workloads, making the solution highly scalable and future-proof.

6. Security and Access Control:
   - AWS Identity and Access Management (IAM) policies and roles can be used to control access to the various components of the solution, such as S3 buckets, Glue crawlers, Athena queries, and QuickSight dashboards.
   - IAM roles with the necessary permissions can be assigned to Ansible hosts or specific users/groups, ensuring secure and controlled access to the disk utilization data and reporting.

7. Automation and Monitoring:
   - AWS CloudWatch can be integrated to monitor the health and performance of the solution components, such as Glue crawlers, Athena queries, and QuickSight dashboards.
   - AWS Lambda functions or AWS Step Functions can be used to automate the execution of Ansible playbooks, Glue crawlers, and Athena queries, ensuring regular and consistent data collection and aggregation.

By leveraging various AWS services and Ansible for configuration management, this solution provides a centralized, scalable, and secure approach to monitoring disk utilization across multiple AWS accounts. The architecture can be further extended with additional features like alerting, cost optimization, and integration with other monitoring tools as per the company's requirements.

This usecase can be achieved multiple other approaches as well , for example we can leverage ansible itself for tdoing the post processing and Data aggregation. Or we can use AWS Opensearch in order to build a Kibana Dashboard. 

