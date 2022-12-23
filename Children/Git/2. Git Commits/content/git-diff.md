## Git Diff \<commit1\> \<commit2\>
`git diff master..feature` То же что и пробел. Дифф между коммитами, на которых стоят бранчхеды.  
`git diff master...feature` Дифф feature с последним общим коммитом веток  
`git diff e2e4 e4e2` Дифф e4e2 с e2e4  
`git diff e2e4` Сравнить WT с коммитом e2e4  
`git diff HEAD` Как git status, только для содержимого файлов  
`git diff -- index.html` Сравнить версию из working directory с последним коммитом

## Git Diff \-\-cached
Сравнить staging с указанным коммитом
`git diff --cached` Сравнить staging с HEAD
Git diff не показывает untracked файлы

## Git diff \[\-\- index.md\] (вообще без номеров коммитов в параметрах)
Сравнивает WT vs Staging

## Git diff \<один коммит\>
WT vs Коммит
`git diff @`
## Git Diff \-\-name-only master feature

## Git diff commit1:path1 commit2:path2
