+++
date = "2010-11-07T06:59:27Z"
title = "svn 提交时 文件夹 missing 问题的解决"
slug = "svn_commit_missing"

+++
    svn co svn://localhost/ ./
    svn mkdir tag
    rm -rf tag
    svn ci
svn: 提交失败(细节见下)：
`svn: Directory '/home/usr/svncheckout/tag' is missing`
引用[http://stackoverflow.com/questions/1032031/how-to-get-rid-of-missing-directories-in-svn-commit](#)
    mkdir tag
    svn delete --force ./tag
D tag
解决。

