# Migrating Data to the RDS Database

## Dump the Data from the MySQL Database
Log in to the Plant Shop Server.

Dump the contents of the MySQL Database.

```
sudo mysqldump -u root --databases plantshop --single-transaction --order-by-primary > plantshop-dump.sql
```

Get the contents of the dump file and copy them.

```
cat plantshop-dump.sql
```

## Prepare the CloudShell Environment
Log in to the AWS Management Console.

Search for and select the `CloudShell` service.

You will need to wait a few moments for the CloudShell environment to be created.

Create the dump file in the CloudShell environment.

```
vi plantshop-dump.sql
```

Paste in the content of the dump file. Check the beginning and end of the file to make sure it pastes correctly.

Get the public IP address of your CloudShell instance. We will use this to temporarily open security group access to the RDS instance to import the data.

```
curl -s checkip.dyndns.org
```

## Temporarily Open Network Access from the CloudShell Instance to RDS Database
Open the AWS management console in another tab.

Search and select the `RDS` service.

Select `Databases`, then choose your database instance. If you used default naming, it will be called `database-1-instance-1`.

Under `VPC security groups`, you can find a link to the security group which contains the RDS instance. Use this link to open the security group in a new tab.

In the security group details, select the `Inbound rules` tab, then click `Edit inbound rules`.

Click `Add rule`. Create a rule with the follwing settings:
- Type: `All traffic`
- Source: `Custom`
- Source IP field: `<CloudShell public IP address>/32`

Click `Save rules`.

## Load the Dumped Data into the RDS Database
Return to the tab with your RDS instance details.

Copy the URL displayed under `Endpoint`.

Go back to the CloudShell tab.

Re-create the data in the RDS database. You will need to supply the admin password for the RDS instance.

```
cat plantshop-dump.sql | mysql --host=<Endpoint URL> -u admin -p
```

Create a service account with limited permissions to be used by the API later. Feel free to use whatever you wish for the password, just remember that you will need this password later.

```
mysql --host=<Endpoint URL> -u admin -p -e \
'create user "plantshop" identified with mysql_native_password by "<password>"; \
grant select on plantshop.items to plantshop;'
```

Test the service account and verify that the migrated data is present in the database. You will need to enter the password for the `plantshop` account.

```
mysql --host=<Endpoint URL> -u plantshop -p -e 'select * from plantshop.items'
```

Return to the inbound rules for the security group and select `Edit inbound rules` again.

`Delete` the rule you added earlier for the CloudShell IP address, the click `Save rules`.
