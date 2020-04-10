#### file Protocol

Opens a file on a local or network drive.

**Syntax**

```xml
file:///sDrives[|sFile]
```

**Tokens**

* sDrives

  Specifies the local or network drive.

* sFile

  Optional. Specifies the file to open. If sFile is omitted and the account accessing the drive has permission to browse the directory, a list of accessible files and directories is displayed.

**Remarks**

> The **file** protocol and *sDrives* parameter can be omitted and substituted with just the command line representation of the drive letter and file location. For example, to browse the My  Documents directory, the **file** protocol can be specified as file:///C|/My Documents/ or as C:\My Documents\. In addition, a  single '\' is equivalent to specifying the root directory on the primary local drive. On most computers, this is C:\.
>
> Available as of Microsoft Internet Explorer 3.0 or later.
>
> **Note** Internet Explorer 6 Service Pack 1 (SP1) no  longer allows browsing a local machine from the Internet zone. For  instance, if an Internet site contains a link to a local file, Internet  Explorer 6 SP1 displays a blank page when a user clicks on the link.  Previous versions of Windows Internet Explorer followed the link to the  local file.

Example

The following sample demonstrates four ways to use the File protocol.

```
//Specifying a drive and a file name. 

file:///C|/My Documents/ALetter.html

//Specifying only a drive and a path to browse the directory. 

file:///C|/My Documents/

//Specifying a drive and a directory using the command line representation of the directory location. 

C:\My Documents\

//Specifying only the directory on the local primary drive. 

\My Documents\
```





#### 参考来源

[file Protocol](https://docs.microsoft.com/en-us/previous-versions/aa767731(v=vs.85))