# getting "Too Many Connections" error when connecting to my Amazon Aurora MySQL instance?

## login to mysql account with root privilege's 
```bash
SHOW FULL PROCESSLIST;
```

## Show threads currently running on Aurora MySQL DB instance [OPTIONAL]
```bash
 SHOW FULL PROCESSLIST\G;
```
You can also run the following query to get the same result set:

SELECT * FROM INFORMATION_SCHEMA.PROCESSLIST

## Terminate existing connections on your DB instance
 rds_kill and rds_kill_query commands
 ```bash
CALL mysql.rds_kill(thread-ID);
CALL mysql.rds_kill_query(thread-ID);
```
# scale the instance up to a db instance class with more memory
modify db instance class or more memory according Maximum Number of Connections
![image](https://github.com/prajeet1000/devops-duniya/assets/132644038/2bff2042-8d13-46d4-9c53-0b19fd566c89)
 source -- https://support.huaweicloud.com/intl/en-us/pwp-rds/rds_swp_mysql_05.html
 
## Increase the maximum connections to your DB instance
![image](https://github.com/prajeet1000/devops-duniya/assets/132644038/cd942d7e-6f94-4491-8647-ce35508abe10)
![image](https://github.com/prajeet1000/devops-duniya/assets/132644038/38723669-56a4-4310-9863-2951584baa9c)



documentation 
https://repost.aws/knowledge-center/aurora-mysql-max-connection-errors

youtube 
https://www.youtube.com/watch?v=tJOimhdR2rM&ab_channel=AmazonWebServices

table lists the default values of max_connections for different memory specifications
https://support.huaweicloud.com/intl/en-us/rds_faq/rds_faq_0055.html#:~:text=RDS%20does%20not%20have%20constraints,for%20the%20PostgreSQL%20DB%20engine


