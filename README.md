# grafana-migration-6.5-to-8.5

Steps
=====

1. Create new RDS progress DB 
2. Install Grafana with latest version on new host and point it to DB created in step 1
3. Take a backup of existing Grafana(6.5) RDS DB using pg_dump postgress utility.
    
        pg_dump --host=<hotname> --port=5432 --username=<username> --password --database=<db_name> --tables > src_backup
        pg_dump --host=<hotname> --port=5432 --username=<username> --password --database=<db_name> --tables=public.temp_user > src_temp_user_backup
    
4. Take the backup of 8.5 grafana

        pg_dump --host=<hotname> --port=5432 --username=<username> --password --database=<db_name> --tables > tgt_backup
        pg_dump --host=<hotname> --port=5432 --username=<username> --password --database=<db_name> --tables=public.temp_user > tgt_temp_user_backup

5. Update the datatype for last two columns of public.temp_user from int to char

        alter table public.temp_user alter column <column_name> type <new_data_type>    

        alter table public.temp_user alter column created type varchar 250;    
        alter table public.temp_user alter column updated type varchar 250;    


6. Import the data from 6.5 RDS DB to new DB

            psql --host=<hotname> --port=5432 --username=<username> --password --database=<db_name> --tables < src_backup
            psql --host=<hotname> --port=5432 --username=<username> --password --database=<db_name> --tables=public.temp_user < src_temp_user_backup

7. Now from the GUI if you see, there is list of pending invite users for the grafana. You need to send invite for them.

