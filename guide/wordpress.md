<h1>Step 4: Putting it all together</h1>

<p>
Now that our required instances are running and sentinal, <a href="https://aws.amazon.com/getting-started/hands-on/deploy-wordpress-with-amazon-rds/4/">it's time to run wordpress</a>.
<br />
Let's start off with an SSH into our EC2 instance:
</p>

```bash
aws ec2 describe-instances 		#grab the public DNS name

ssh -i cloud-1-key-pair ec2-user@<<PUBLIC_DNS_NAME>>
```

<p>
Then we'll download and install mysql.
</p>

```bash
sudo su

yum install mysql -y
```

<p>
Setup your MySQL hostname to the RDS ip address.
</p>

```bash
aws rds describe-db-instances	#Find your RDS IP

export MYSQL_HOST = <<enpoint>> #replace <<enpoint>> with your RDS IP
```

<p>
Next we'll setup the credentials for our DB.
</p>

```bash
mysql --user=admin --password=farewell42@WTC cloud-1
```

<p>
Now that we're connected to the DB, we'll create a database for our application.
</p>

```bash
CREATE USER 'wordpress' IDENTIFIED BY 'wordpress-pass';
GRANT ALL PRIVILEGES ON wordpress.* TO wordpress;
FLUSH PRIVILEGES;
Exit
```

<p>
Now, let's install and sun apache.
</p>

```bash
yum install httpd -y

service httpd start
```

<p>
Use the describe-instances from earlier command to get your EC2 Public DNS (IPv4) address and enter it in your browser.
<br />
Next we will <a href="https://aws.amazon.com/getting-started/hands-on/deploy-wordpress-with-amazon-rds/5/">download and configure WordPress</a>.

```bash
wget https://wordpress.org/latest.tar.gz

tar -xzf latest.tar.gz

cd wordpress

cp wp-config-sample.php wp-config.php
```

<p>
Open the wp-config.php file with a text editor.
</p>

```bash
nano wp-config.php

* DB_NAME: cloud-1
* DB_USER: admin
* DB_PASSWORD: farewell42@WTC
* DB_HOST: <<DB HOSTNAME>>
```
<p>
We can also <a href="https://api.wordpress.org/secret-key/1.1/salt/">generate unique keys and salts for our authentication process with this link</a>.
<br />
Finally we will deploy WordPress.
</p>

```bash
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2

cd /home/ec2-user

sudo cp -r wordpress/* /var/www/html/

sudo service httpd restart
```

<p>
If you see the WordPress welcome page, that means our installation was successful.
</p>

<hr />
<a href="rds.md">
&lt; Previous
</a>

<a href="load_balancer.md" align="right">
Next &gt;
</a>