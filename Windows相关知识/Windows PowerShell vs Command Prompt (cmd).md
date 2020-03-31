### What is the Windows Command Prompt?

Windwos Command Prompt(also known as the command line, cmd.exe or simply cmd) is a command shell based on the MS-DOS operating system from the 1980s that enables a user to interact directly with the operating system. Specifically, this venerable command shell provides an environment to run applications and various utilities; output is displayed in the same window. It is possible to use the cmd shell to create and edit scripts and save them to batch files to solve automation tasks in one-system frames; however, it was never intended for remote system administration.

![](I:\GreatGeek\相关知识\Windows相关知识\Windows PowerShell vs Command Prompt (cmd).assets\cmd_vs_powershell.png)


### What is Windows PowerShell?
Windows PowerShell is a command shell and scripting language designed for system administration tasks. It was built on top of the .NET framework, which is a platform for software programming developed by Microsoft in 2002.

PowerShell commands, or cmdlets, help you manage your Windows infrastructure. In addition, they enable a user to access the registry, the file system and Windows Management Instrumentation(WMI) space on systems remotely. Moreover, the PowerShell command shell enables you to create complex scripts with multiple conditions.

![](I:\GreatGeek\相关知识\Windows相关知识\Windows PowerShell vs Command Prompt (cmd).assets\cmd_vs_powershell2.png)


### How PowerShell differs from Command Prompt
As mentioned earlier, cmd is a very old tool that was never intended for remote system administration. Extending its functionality requires additional utilities, such as Microsoft Sysinternals PsExec.

PowerShell, on the other hand, provides many cmdlets to simplify system administration task. It supports the automation of a wide range of tasks, such as Active Directory administration, user and permissions management, and extracting data about security configurations. Moreover, PowerShell now supports Linux.

The following table summarizes the key differences between Command Prompt and PowerShell from a programming and operations perspective:

![](I:\GreatGeek\相关知识\Windows相关知识\Windows PowerShell vs Command Prompt (cmd).assets\cmd_vs_PS_table.jpg)

### PowerShell or cmd: Which should I choose
Clearly, there are many reasons why Windows PowerShell replaced the Command Prompt as the default in the Windows 10 operating system, and was preinstalled starting with Window XP. But if you're used to using cmd, you don't need to feel any urgency in switching to PowerShell. In fact, most commands from cmd work fine in the PowerShell environment -- Microsoft wanted to simplify the lives of system administrators, so it created command prompt aliases in PowerShell that enable it to interpret old DOS commands as new PowerShell commands.

To find out how old cmd commands map to the newer PowerShell cmdlets, use the Get-Alias command:

![](I:\GreatGeek\相关知识\Windows相关知识\Windows PowerShell vs Command Prompt (cmd).assets\cmd_vs_powershell3.png)

[PowerShell 概述](https://docs.microsoft.com/zh-cn/powershell/scripting/overview?view=powershell-7)