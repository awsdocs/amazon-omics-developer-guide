# Using workflows<a name="creating-workflows"></a>

 The first step to using Omics Workflows is to create a workflow The following examples demonstrate how to create and manage a workflows using the APIs or AWS CLI\.



To create a workflow, you will need both input and output Amazon S3 buckets and an Amazon ECR container\. You will also need an IAM policy that grants access to those resources\.

The following is a comprehensive example of an IAM role that grants permission to access the those resources\. Also included in this policy is access to the CloudWatch logs that are helpful for troubleshooting or for tracking use AWS actions and resources\.

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

The role must authorize the service to assume it, which can be done by adding the following trust policy\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":"sts:AssumeRole",
         "Principal":{
            "Service":"omics.amazonaws.com"
         }
      }
   ]
}
```

Workflow definitions must be written in either WDL or Nextflow\. The following is an example that demonstrates a workflow that reads the contents of an INPUT file and writes them into a RESULT file\. 

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

Once the file is created and zipped, it can be used along with a parameters JSON file similar to the following to create a workflow\.

```
{
"input_txt_file": "Input text file to copy"
}
```

Once you've defined your workflow and the parameters, you can create a workflow using the API as shown\. If you are including multiple workflow definition files, use the `--main` parameter to specify which file is the main definition file for your workflow\.

```
aws omics create-workflow --name Test --main multi_workflow/workflow2.wdl --definition-zip fileb://definition.zip --parameter-template file://params_sample_description.json    
```

In place of a file path, you can also specify an input Amazon S3 bucket using the `--definition-uri` parameter, as shown\.

```
aws omics create-workflow --name Test --main multi_workflow/workflow2.wdl --definition-zip DOC-EXAMPLE-BUCKET --parameter-template file://params_sample_description.json         
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

You can verify the status of the workflow with get\-workflow as shown\.

```
aws omics get-workflow --id 12345       
```

The response will give you details of your workflow, including the status, as shown\.

```
{
    "arn": "arn:aws:omics:us-west-2:....",
    "id": "12345",
    "status": "ACTIVE",
    "type": "PRIVATE",
    "name": " "
    "creationTime": "2022-07-06T00:27:05.542459"
    }
}
```

Before a run can be started, the status must be listed as ACTIVE\.