## 检查项目是否在启动 <!-- {docsify-ignore-all} -->

 [  -n  参数  ]  可以用来判断该参数是否已被赋值

```$shell

#Docker
APPID=`ps -ef | grep dockerd | grep usr | awk '{print $2}'`
if [ -n "$APPID" ]
then
		printf "%-60s %-15s\n" "Docker" "Started"
        # echo -e "Docker                            $Started"
else
		printf "%-60s %-15s\n" "Docker" "Stoped"
	    # echo -e "Docker                            $Stoped"
fi
```




## 