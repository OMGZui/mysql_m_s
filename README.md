# mysql主从

```bash
# 准备容器
docker pull mysql:5.5

# 目录
├── LICENSE
├── README.md
├── docker-compose.yml
├── master
│   ├── Dockerfile
│   ├── docker-entrypoint-initdb.d
│   │   └── init.sql
│   └── my.cnf
├── slave1
│   ├── Dockerfile
│   ├── docker-entrypoint-initdb.d
│   │   └── init.sql
│   └── my.cnf
└── slave2
    ├── Dockerfile
    ├── docker-entrypoint-initdb.d
    │   └── init.sql
    └── my.cnf
# 启动
docker-compose up -d master slave1 slave2

# 主容器
# GRANT REPLICATION SLAVE ON *.* to 'backup'@'%' identified by 'backup';
# flush master;
docker-compose exec master /bin/bash
mysql -u root -p < docker-entrypoint-initdb.d/init.sql

# 从容器
# stop slave;
# flush slave;
# CHANGE MASTER TO MASTER_HOST='master', MASTER_PORT=3306, MASTER_USER='backup', MASTER_PASSWORD='backup';
# START SLAVE;

# 从容器1
docker-compose exec slave1 /bin/bash
mysql -u root -p < docker-entrypoint-initdb.d/init.sql

# 从容器2
docker-compose exec slave2 /bin/bash
mysql -u root -p < docker-entrypoint-initdb.d/init.sql

```