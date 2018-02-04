---
title: nginx每天生成一个log文件
date: 2018-02-03 13:01:03
tags:
- nginx
categories:
- nginx
---
### 需求
    nginx常年累月地运行，会产生大量的日志，这些日志如果一直存放在一个文件中，会导致日志文件越来越大，最终
    查阅这些文件时会非常麻烦，因此如果每天的日志存放在单独一个文件中，维护起来就会非常方便了。但是nginx没有
    提供这个功能，因此，需要我们写脚本来实现这个功能。
    
### 脚本
```
LOGS_PATH=/usr/local/nginx/logs
DEST_LOGS_PATH=/data/logs/nginx
PID_FILE=/usr/local/nginx/logs/nginx.pid

YESTERDAY=$(date -d -1day +%Y-%m-%d)
TWO_MONTH_AGO=$(date -d -60day +%Y-%m-%d)
OLD_ACCESS_LOG=${DEST_LOGS_PATH}/access_${TWO_MONTH_AGO}.log
OLD_ERROR_LOG=${DEST_LOGS_PATH}/error_${TWO_MONTH_AGO}.log

mv ${LOGS_PATH}/access.log ${DEST_LOGS_PATH}/access_${YESTERDAY}.log
mv ${LOGS_PATH}/error.log ${DEST_LOGS_PATH}/error_${YESTERDAY}.log 

echo ${TWO_MONTH_AGO}

# 删除两个月以前的access error log
if [ -f ${OLD_ACCESS_LOG} ]; then
	rm -rf ${OLD_ACCESS_LOG}
fi
if [ -f ${OLD_ERROR_LOG} ]; then
	rm -rf ${OLD_ERROR_LOG}
fi

sudo kill -USR1 $(cat $PID_FILE)
```

### 说明
    上述功能实现了每天把日志文件转存到另外一个目录下，然后LOGS_PATH会产生两个新的日志文件，nginx新产生
    文件都会保存在新日志文件中，这种方式变相实现了日志俺日期分割。另外，此脚本也会删除2个月前的日志。
