## Adv1
Установить плагин **Publish over SSH**

### Джоб 1
New Item - Freestyle project  
Item name: **MyJobNumber-1**  
Description: **Adv1 first job**  

Разрешить выполнять несколько билдов одновременно:
- [X] Execute concurent builds if necessary  

Удалять старые билды из папки `cd /var/lib/jenkins/jobs/ИмяДжоба/builds `: 
 - [X] Discard old builds
Обычно ставят max of builds to keep: 5

**Build:**  
Execute shell:  
```bash
echo "HelloWorld!"
echo "This is Build number: $BUILD_NUMBER"
pwd
sleep 5
whoami
echo "Name of this Build is: $BUILD_DISPLAY_NAME"
```



Каждый запуск создает директорию в:  
```bash
cd /var/lib/jenkins/jobs 
ll
```

### Job 3
New Item - Freestyle project  
Discard old builds: Max of builds: 5  
- [ ] Execute concurrent

**Build:**  
**Execute Shell:**  
```bash
echo "==========Build Started=========="
cat <<EOF >> index.html
<html>
<body bgcolor=black>
<center>
<h2><font color=yellow>Hello Job</font></h2>

<font color=blue>Job</font>

</center>
</body
</html>
EOF

echo "==========Build Finished=========="
```  

**Execute Shell:**  
```bash
echo "==========Test Started=========="
result=`grep "Hello" index.html | wc -l`
echo $result
if [ "$result" = "1" ]
then
    echo "Test Passed"
    exit 0
else
    echo "Test Failed"
    exit 1
fi
echo "==========Test Finished=========="
```


**Execute Shell:**  
```bash
echo "==========Deployment Started=========="

```

**Post-build Action**  
**Send build artifacts over SSH:**  
Source files  `/*`  
Remote directory ` `  
Exec command  `sudo service httpd restart`  
