# Week 4 — Postgres and RDS
We know need to work towards data modelling in 3NF for SQL. This week focuses on implementing Postgres Relational Database into our backend application to store user and activity Crud post data. AWS RDS will be used as our Production Database. 
## Required Homework
### Provision an RDS instance
First, we provision an RDS instance via ClickOps. This will serve as our Prod Database. It uses a Postgres Engine and runs in the ca-central-1a availability zone. 

![](assets/Week4-RDS-Instance.png)
### Remotely connect to RDS instance
To connect to our prod database from our Gitpod Environment. We need to create a bash script named [db-connect](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/blob/main/backend-flask/bin/db-connect). The script has a parameter that contains an if statement. If we enter the command ./bin/db-connect *prod*, this will connect to the prod database, if *prod* is left out of the command, then the script will connect to the local postgres database. 

![](assets/Week4-Prod-DB-Connect.png)
### Programmatically update a security group rule
We also need to update the security group for our RDS DB to allow a connection from our Gitpod Environment. However, this becomes an issue as the IP Address of the environment does not persist after every reboot. [This](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/commit/8c4f080720e3dda3d4d5d8f77f4f8f28c0dbf61c) was implemented to the gitpod.yml file to update the variable stored for the Gitpod Environment's IP, as well as running a script named [rds-update-sg-rule](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/blob/8c4f080720e3dda3d4d5d8f77f4f8f28c0dbf61c/backend-flask/bin/rds-update-sg-rule), which will run a aws-cli command to update our RDS Security Group to reflect the new IP Address.
### Write several bash scripts for database operations
[Here](ki23gup/aws-bootcamp-cruddur-2023/commit/4596cb7551bd11caacc912a6baea217917d96c1b) are some bash scripts we implemented to speed up interaction with the DB:
- db-connect: Connects to either local or production RDS database based on parameter provided
- db-create: Creates a database named 'cruddur' locally
- db-drop: Drops the 'cruddur' database locally
- db-schema: Creates a schema for the DB either locally or production using a 'schema.sql' file
- db-seed: Seeds in temporary data into the database schema either locally or on production DB using the 'seed.sql' file
### Seed Postgres Database table with Data
The bash scripts created previosly can be used to send data to the postgres table. The [db-seed](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/blob/4596cb7551bd11caacc912a6baea217917d96c1b/backend-flask/bin/db-seed) script makes use of the [seed.sql](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/blob/4596cb7551bd11caacc912a6baea217917d96c1b/backend-flask/db/seed.sql) file to perform this. 

Once we run the command from our backend directory ```./bin/db-seed``` ,

We can view the tables created:
![](assets/Week4-DB-Schema.png)
### Implement a postgres client for python using a connection pool
We also need PostgreSQL adapter for Python which allows the application to create/destroy many cursors and make concurrent INSERT or UPDATE SQLs. We added [this](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/commit/71031e7e390a81b23f0cb0c6f3d97f55bc502f80#diff-e7fc4f0f2b4e4510d81bbc953fe4e4198587359967fc005d49cb23f39e7f3130) to our backend application and created a new file [db.py](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/blob/71031e7e390a81b23f0cb0c6f3d97f55bc502f80/backend-flask/lib/db.py) file which implements the adapter. 
### Implement a Lambda that runs in a VPC and commits code to RDS
We also created a Lambda function to commit submitted user information via Signup to the RDS Production database. This adds the User's profile information such as name, email, handle and cognito ID into the 'users' table in the database once the user signs up for an account. 
Once we signup, you can see the logs from lamda function as it ran:

![](assets/Week4-Post-Confirmation.png)
### Creating Activity
Finally, we can now try to post a 'Crud' in the application to see if it works and stores the activity in the prod database.

The crud posting did not work as expected. The issue seemed to be related to 'andrewbrown' handle being hardcoded into the 'data-activities' function of the app.py file. Since I was logged in as my own username 'akshayvineet' this was causing errors. The [fix](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/commit/83c74ac356edb1ea0706f8e891a646503d6b20f1) was implemented to change the hardcoded values to the currently logged in user. 

Crud posting now works

![](assets/Week4-CrudPost.png)

Checking DB to make sure we have an entry:

![](assets/Week4-Crud-DB-Check.png)

All in all, this week involved a lot of troubleshooting. Nonetheless, it was great to traverse my way through the application and rectifying issues to achieve desired results. 
