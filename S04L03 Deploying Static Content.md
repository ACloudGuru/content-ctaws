# Deploying Static Content

## Clone the Frontend Code in CloudShell
Open a CloudShell instance by searching for and navigating to the CloudShell service in the AWS management console.

Once the instance is ready, use it to clone the GitHub repo for the app. You can skip this if you already did it in a previous lesson.

```
git clone https://github.com/ACloudGuru/ctaws-plant-shop.git
```

## Provide the Lambda URL to the App's Code
Next, we will need to edit the app's main file to supply the new Lambda API endpoint URL.

To get the URL, open AWS Management Console in a new tab. Search for and select the `Lambda` service.

Select `Functions`, then your `PlantShopAPI` function.

Look under `Function URL`. This the URL we need. You can copy it or take note of it for later.

Edit the app's main file.

```
cd ctaws-plant-shop/frontend

vi src/App.js
```

Find the line with `const apiUrl` and change its value to the URL of the lambda function.

```
const apiUrl = "<Your Lambda Function URL>"
```

## Build the Code
Next, build the app.

```
npm install

npm run build
```

This will create a directory within `ctaws-plant-shop/frontend` called `build`.

## Upload the Files to the S3 Bucket
Now we are ready to upload the files. You can do so using the `aws` command line tool which is already installed in the CloudShell environment. You will need to supply your bucket name with this command.

```
aws s3 cp --recursive build s3://<Bucket Name>
```

In the AWS management console for your bucket, go to the `Properties` tab. Under `Static website hosting` you can find `Bucket website endpoint`. This is the URL for your bucket, which you can use to test the app. Open this URL in a new tab.

If everything is working as expected, you should be able to see the plant shop site populated by the item data loaded from the database via the API.

This means that the frontend and the entire application is working!
