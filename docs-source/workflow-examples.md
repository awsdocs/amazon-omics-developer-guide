# Workflow tutorial<a name="workflow-examples"></a>

## Creating a workflow using the AWS Command Line Interface<a name="workflow-examples-cli"></a>

You will need both input and output Amazon S3 buckets, as well as an IAM role with access to those buckets\. The following is an example IAM policy that grants permission to access the contents of an Amazon S3 bucket, the ECR containers, and Cloud Watch logs\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::[[s3path]]/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::[[s3path]]"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::[[output_s3path]]/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:DescribeLogStreams",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:{{region}}:{{accountId}}:log-group:/aws/omics/WorkflowLog:log-stream:*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup"
            ],
            "Resource": [
                "arn:aws:logs:{{region}}:{{accountId}}:log-group:/aws/omics/WorkflowLog:*"
            ]
        },
        {
          "Effect": "Allow",
          "Action": [
              "ecr:BatchGetImage",
              "ecr:GetDownloadUrlForLayer",
              "ecr:BatchCheckLayerAvailability"
          ],
          "Resource": [
              "arn:aws:ecr:{{region}}:{{accountId}}:repository/*"
          ]
      }
    ]
}
```

The role will need to authorize the service to assume it before it can be used in a workflow run\. This can be done by adding "trust relationships" similar to the following statement\.

```
{
    "Effect": "Allow",
    "Principal": {
    "Service": "omics.amazonaws.com"
           },
     "Action": "sts:AssumeRole"
}
```

Workflow definitions must be written in the supported languages, either WDL or Nextflow\. The following is a basic example that demonstrates a workflow that reads the contents of an INPUT file and writes them into a RESULT file\. 

```
  version 1.0

# Simple demo workflow, copy input file into output file

workflow TestFlow {
    input {
        File input_txt_file
    }

    #copies input file data to output. 
    call TxtFileCopyTask{
        input:
            input_txt_file = input_txt_file,
    }

    output {
        File output_txt_file = TxtFileCopyTask.output_txt_file
    }

}

#Task Definitions

task TxtFileCopyTask {
    input {
        File input_txt_file
    }

    command {
        cat ~{input_txt_file} > outfile.txt
    }

    output {
        File output_txt_file = "outfile.txt"
    }
}
```

The workflow definition files need to be zipped before calling the Omics CreateWorkflow API operation\. 

```
zip definition.zip main.wdl 
```

Once you've defined your workflow and the parameters, you can create a Workflow using the API as shown\.

```
aws omics create-workflow --name Sample --description BasicExample --definition-zip fileb://definition.zip --parameters file://params_sample_description.json 
```

You should receive the following response to confirm that the workflow has been created\.

```
{
    "arn": "arn:aws:omics:us-west-2:....",
    "id": "12345",
    "status": "CREATING",
    "tags": {
        "resourceArn": "arn:aws:omics:us-west-2:...."
    }
}
```