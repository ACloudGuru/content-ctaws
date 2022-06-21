# Creating a Lambda Function

## Create the Function
Log in to the AWS Management Console.

Search for and select the `Lambda` service.

Click `Create function`, and create a lambda function with the following settings:
- Function name: `PlantShopAPI`
- Runtime: `Node.js`

Under `Advanced Settings`:
- Check `Enable function URL`
- Auth type `NONE`
- Check `Configure cross-origin resource sharing (CORS)`
- Check `Enable VPC`
- VPC: Select the default VPC from the list (This should be the same VPC the RDS instance is in).
- Subnets: Check all of the subnets in the list.
- Security groups: Check the default security group from the list (Again, this is the same security group that the RDS instance is in).

Click `Create function`.

## Package the Code
From your CloudShell environment, package and deploy the code.

```
cd api/lambda

npm install

zip -r lambda.zip .
```

This should create a file called `lambda.zip`. However, if you wish to skip building the zip file yourself, you can simply download it from the GitHub release:

https://github.com/ACloudGuru/ctaws-plant-shop/releases/download/v0.0.1/lambda.zip

## Deploy the Code
From the CloudShell instance, use the `aws` cli to deploy the Lambda code.

```
aws lambda update-function-code \
    --function-name PlantShopAPI \
    --zip-file fileb://lambda.zip
```

## Set Up Environment Variables
Go back to the Lambda function in the AWS management console.

Before we can test the function, we need to add the MySQL connection info to the function's environment variables.

Click `Configuration` > `Environment variables` > `Edit`.

Use the `Add environment variable` button to add three new environment variables:
- `DB_HOST` - Open another AWS Management console tab, navigate to the `RDS` Service and select your RDS instance (`database-1-instance-1` if you used the default name). Copy the value dispolayed under `Endpoint`, and use that for the value of this environment variable.
- `DB_USER` - `plantshop`
- `DB_PASS` - Enter the password you used for the `plantshop` service account when setting up the RDS data.

Click `Save`.

## Test the Function
Click the `Test` tab, then the `Test` button lower down to perform a test of the function. It should succeed. You can click `Details` to see more info.

Next, locate the `Function URL` in the overview box at the top of the page. Open this url in a new browser tab. You should get JSON data from the API Lambda function.

This data originated from the Amazon Aurora RDS instance, and was retrieved by the Lambda function. This means our Lambda function is working and ready for use!
