# Implementing MySQL for Garment Manufacturing Productivity Dataset
Make sure you have MySQL installed.

## Launch MySQL
Configure MySQL by following the steps [here](https://github.com/yuenherny/um-wqd7007-bdm-cheatsheets/blob/main/mysql/MYSQL-LAB.md#launch-mysql)then you can do:
```
$ mysql -u root -p
```
If the above command does not work, then try:
```
$ mysql -uroot -proot
```

## Create a table
In MySQL, create a database:
```
mysql> create database wqd7007;
mysql> use wqd7007;
```
Create table called `garment`:
```
mysql> create table garment (record_date date,
    -> quarter varchar(30),
    -> department varchar(30),
    -> day varchar(30),
    -> team int(6),
    -> targeted_productivity numeric(10,2),
    -> smv numeric(10,2),
    -> wip int(6),
    -> over_time int(6),
    -> incentive int(6),
    -> idle_time int(6),
    -> idle_men int(6),
    -> no_of_style_change int(6),
    -> no_of_workers numeric(10,2),
    -> actual_productivity numeric(10,6));
```
then exit by typing `exit` and enter.

## Load data into a table
Before you do so, you need to change the permissions. Follow the steps [here](https://github.com/yuenherny/um-wqd7007-bdm-cheatsheets/blob/main/mysql/MYSQL-LAB.md#load-data-into-a-table). Then, download the CSV file in the `dataset` folder and keep it in `Downloads` folder, then:
```
$ mysql -uroot -proot --local_infile=1 mysql -e "load data local infile '~/Downloads/garments_worker_productivity.csv' into table WQD7007.garment fields terminated by ',' ignore 1 lines"
```
Check by logging in again and do `SELECT`:
```
mysql> use wqd7007;
mysql> select * from garment;
```

## Discover insights about the dataset
Find the bottom 3 teams in terms of average actual productivity:
```
mysql> select team, AVG(actual_productivity) as avg_prod from garment group by team order by avg_prod limit 3;
```

Find the bottom 3 departments (tasks) in terms of average actual productivity:
```
mysql> select department, AVG(actual_productivity) as avg_prod from garment group by department order by avg_prod limit 3;
```

Find the top 5 teams that has highest actual productivity, sorted ascendingly on average number of workers across all tasks:
```
mysql> select team, AVG(actual_productivity) as avg_prod, avg(no_of_workers) as avg_workers from garment group by team order by avg_prod desc, avg_workers limit 5;
```

Find the top 5 teams that has highest actual productivity, sorted ascendingly on sum of overtime:
```
mysql> select team, AVG(actual_productivity) as avg_prod, SUM(no_of_workers) as sum_overtime from garment group by team order by avg_prod desc, sum_overtime limit 5;
```

Find the top 5 teams that has highest difference between average targeted productivity and average actual productivity:
```
mysql> select team, AVG(actual_productivity)-AVG(targeted_productivity) as prod_diff from garment group by team order by prod_diff desc limit 5;
```

The full PDF report is accessible in `report` folder.
