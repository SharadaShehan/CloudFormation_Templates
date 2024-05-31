### Template for Lambda Triggered S3 Object Processing

This template creates two s3 buckets and a lambda function that processes images uploaded to one of S3 buckets and stores them in other S3 bucket. The lambda function is triggered by an S3 event notification.

#### Steps to deploy the template

1. Create a S3 bucket to store configuration files. Replace `config-bucket-name` with the name of the bucket you want to create.
    ```
    aws s3 mb s3://config-bucket-name
    ```

2. Upload the lambda layer zip file to the S3 bucket created in step 1.
    ```
    aws s3 cp lambda-layer.zip s3://config-bucket-name/
    ```

3. Create Stack using the resources_template.yaml file. Replace `config-bucket-name` with the name of the bucket created in step 1.
    ```
    aws cloudformation create-stack --stack-name pma-resources-stack --template-body file://resources_template.yaml --capabilities CAPABILITY_NAMED_IAM --profile default --parameters ParameterKey=ConfigBucket,ParameterValue=config-bucket-name
    ```

4. Update the stack using the add_lambda_trigger.yaml file. Replace `config-bucket-name` with the name of the bucket created in step 1.
    ```
    aws cloudformation update-stack --stack-name pma-resources-stack --template-body file://add_lambda_trigger.yaml  --capabilities CAPABILITY_NAMED_IAM --profile default --parameters ParameterKey=ConfigBucket,ParameterValue=config-bucket-name
    ```

* You can change profile to the profile you are using to deploy the stack and stack-name to the name you want to give to the stack. (Must be done in both steps 3,4)
