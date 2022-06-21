# What is S3?

## Create the S3 Bucket
Log in to the AWS Management Console.

Search for and select the `S3` service.

Click `Create bucket`.

Create a bucket with the following settings:
- Bucket name: Choose something unique. Bucket names must be unique, even among other AWS users. You may want to include `plant-shop` in the name, just to ensure that you can tell what this bucket is for later.
- **Uncheck** `Block all public access`
- Check the `I acknowledge...` warning that appears. Don't worry, we want this bucket to be publicly available since it is the frontend for our static website!

Click `Create bucket`.

## Set Up Static Website Hosting
Click on your new bucket, then click `Properties`.

Click the `Edit` button next to `Static website hosting`. Change the following settings:
- Static website hosting - `Enable`
- Index document - `index.html`

Click `Save changes`.

## Set Up the Bucket Policy
Click the `Permissions` tab. Then, click the `Edit` button next to `Bucket policy`.

Create a bucket policy that will allow everyone to view everything in the bucket (i.e. all of our static website files). You will need to supply the name of your bucket where the policy below says `<bucket name>`.

**Note:** You may see some error message on this screen. You can ignore these.

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "AllowAllGet",
			"Effect": "Allow",
			"Principal": "*",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::<bucket name>/*"
		}
	]
}
```

Click `Save changes`.
