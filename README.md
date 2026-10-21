# content = r"""# Cloud Computing Practicals — Short Programs

Shortened programs and commands for all 8 experiments from the practical record, with the same sample outputs.

> **Note:** Sample outputs are retained from the original record. When running cloud commands, replace example IP addresses, bucket names, project IDs, and IAM role ARNs with the values from your own cloud account.

## Index

1. [Google Drive Spreadsheet and Notes](#1-google-drive-spreadsheet-and-notes)
2. [Launch a Linux Virtual Machine](#2-launch-a-linux-virtual-machine)
3. [Host a Static Website](#3-host-a-static-website)
4. [Explore Google Cloud Services](#4-explore-google-cloud-services)
5. [Google App Engine](#5-google-app-engine)
6. [Microsoft Azure](#6-microsoft-azure)
7. [Connect Amazon Redshift with S3](#7-connect-amazon-redshift-with-s3)
8. [Create and Query a NoSQL Table](#8-create-and-query-a-nosql-table)

---

## 1. Google Drive Spreadsheet and Notes

**Aim:** To create, edit, and share a spreadsheet and a note/document using Google Drive.

### Program / Steps

In Google Sheets, enter the following data and use `=SUM(C2:E2)` in the Total column, then fill down.

| Roll No | Name | Mark1 | Mark2 | Mark3 | Total |
|---:|---|---:|---:|---:|---:|
| 101 | Arun | 85 | 90 | 88 | `=SUM(C2:E2)` |
| 102 | Priya | 78 | 82 | 91 | `=SUM(C3:E3)` |
| 103 | Kumar | 92 | 88 | 85 | `=SUM(C4:E4)` |

Create a Google Docs note, rename it `Cloud_Notes`, and share it with Editor access.

### Sample Output

```text
Roll No  Name   Mark1  Mark2  Mark3  Total
101      Arun   85     90     88     263
102      Priya  78     82     91     251
103      Kumar  92     88     85     265

Document "Cloud_Notes.docx" created and shared successfully with Editor access.
```

**Result:** Thus a spreadsheet and a note document were created, edited, and shared using Google Drive.

---

## 2. Launch a Linux Virtual Machine

**Aim:** To launch a Linux virtual machine on Amazon EC2 and connect remotely.

### Program / Commands

```bash
chmod 400 CloudKey.pem
ssh -i "CloudKey.pem" ubuntu@<Public-IP-Address>
uname -a
df -h
```

### Sample Output

```text
Welcome to Ubuntu 20.04.5 LTS (GNU/Linux 5.15.0-1019-aws x86_64)
Linux ip-172-31-40-10 5.15.0-1019-aws #23-Ubuntu SMP x86_64 GNU/Linux

Filesystem   Size  Used  Avail  Use%  Mounted on
/dev/xvda1   7.6G  1.6G  6.0G   21%   /
```

**Result:** Thus a Linux virtual machine was launched on AWS EC2 and accessed remotely using SSH.

---

## 3. Host a Static Website

**Aim:** To host a static website using an Amazon S3 bucket.

### Program — `index.html`

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Cloud Website</title>
</head>
<body>
  <h1>Welcome to My Static Website</h1>
  <p>Hosted on Amazon S3.</p>
</body>
</html>
```

Enable **Static website hosting** in S3 and set `index.html` as the index document. Configure the bucket policy to allow public `s3:GetObject` access, as required by the practical.

### Sample Output

```text
Welcome to My Static Website
Hosted on Amazon S3.
```

**Result:** Thus a static website was hosted using Amazon S3.

---

## 4. Explore Google Cloud Services

**Aim:** To explore services offered by Google Cloud Platform.

### Program / Commands

```bash
gcloud projects list
gcloud compute instances list
gsutil ls
bq query --use_legacy_sql=false \
  'SELECT name, count FROM `bigquery-public-data.usa_names.usa_1910_2013` LIMIT 5'
```

### Sample Output

```text
NAME      ZONE           MACHINE_TYPE  STATUS
demo-vm   us-central1-a  e2-micro      RUNNING
```

**Result:** Thus Google Cloud Platform services were explored successfully.

---

## 5. Google App Engine

**Aim:** To configure Google App Engine and deploy a sample application.

### Program — `main.py`

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World! This is my first App Engine application.'
```

### Configuration — `app.yaml`

```yaml
runtime: python39
```

### Deployment Commands

```bash
gcloud init
gcloud app deploy
gcloud app browse
```

### Sample Output

```text
Services to deploy:
descriptor: [app.yaml]
source: [/home/user/myapp]
target project: [cloud-lab-project]
target service: [default]
Deployed service [default] to [https://cloud-lab-project.uc.r.appspot.com]

Browser output: Hello, World! This is my first App Engine application.
```

**Result:** Thus a sample application was deployed successfully using Google App Engine.

---

## 6. Microsoft Azure

**Aim:** To create and manage resources using Azure CLI.

### Program / Commands

```bash
az login
az group create --name CloudLabRG --location eastus
az vm create --resource-group CloudLabRG --name CloudLabVM \
  --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
az resource list --resource-group CloudLabRG --output table
```

### Sample Output

```text
Name          ResourceGroup  Location  Type
CloudLabVM    CloudLabRG     eastus    Microsoft.Compute/virtualMachines
CloudLabVMNic CloudLabRG     eastus    Microsoft.Network/networkInterfaces
CloudLabVMDisk CloudLabRG    eastus    Microsoft.Compute/disks
```

**Result:** Thus Azure CLI was used to create and manage cloud resources.

---

## 7. Connect Amazon Redshift with S3

**Aim:** To load data from an S3 bucket into an Amazon Redshift table.

### Program / SQL Commands

```sql
CREATE TABLE employee (
  emp_id INT,
  emp_name VARCHAR(50),
  dept VARCHAR(50),
  salary DECIMAL(10,2)
);

COPY employee
FROM 's3://cloudlab-bucket/employee.csv'
IAM_ROLE 'arn:aws:iam::123456789012:role/RedshiftS3Role'
CSV IGNOREHEADER 1;

SELECT * FROM employee;
```

### Sample Input — `employee.csv`

```csv
emp_id,emp_name,dept,salary
1,Arun,IT,45000
2,Priya,HR,38000
3,Kumar,Finance,52000
```

### Sample Output

```text
emp_id | emp_name | dept    | salary
1      | Arun     | IT      | 45000.00
2      | Priya    | HR      | 38000.00
3      | Kumar    | Finance | 52000.00
```

**Result:** Thus Amazon Redshift was connected with S3 and the data was loaded and queried.

---

## 8. Create and Query a NoSQL Table

**Aim:** To create and query a NoSQL table using Amazon DynamoDB.

### Program / Commands

```bash
aws dynamodb create-table \
  --table-name Student \
  --attribute-definitions AttributeName=StudentID,AttributeType=S \
  --key-schema AttributeName=StudentID,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST

aws dynamodb put-item --table-name Student \
  --item '{"StudentID":{"S":"101"},"Name":{"S":"Arun"},"Marks":{"N":"85"}}'
```

### Sample Input — Get Item

```bash
aws dynamodb get-item --table-name Student \
  --key '{"StudentID":{"S":"101"}}'
```

### Sample Output

```json
{
  "Item": {
    "StudentID": {"S": "101"},
    "Name": {"S": "Arun"},
    "Marks": {"N": "85"}
  }
}
```

**Result:** Thus a NoSQL table was created and queried using Amazon DynamoDB.

---

## End
"""
path = "/mnt/data/README.md"
with open(path, "w", encoding="utf-8") as f:
    f.write(content)
print(f"Created: {path}")
