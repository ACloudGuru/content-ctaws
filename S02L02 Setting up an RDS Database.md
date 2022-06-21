# Setting up an RDS Database

## Create the RDS Database
Log in to the AWS Management Console.

Search for and select the `RDS` service.

Select `Databases`, then `Create Database`.

Choose the following options:
- Choose a database creation method: `Standard create`
- Engine type: `Amazon Aurora`
- Edition: `Amazon Aurora MySQL-Compatible Edition`
- Master username: `admin`
- Enter and confirm a master password. Make sure you remember the password!
- DB instance class: `Burstable classes` > `db.t3.small`
- Multi-AZ deployment: `Don't create an Aurora Replica`
- Public access: `Yes`

Click `Create database`.

You should see a database and a database instance. Wait until the `Status` for both the database and the database instance becomes `Available`.
