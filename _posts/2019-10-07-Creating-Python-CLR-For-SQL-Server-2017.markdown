---
layout: post
title:  Using Python scripts natively in SQL Server 2017
date:   2019-10-07
description: Overview of running Python scripts on a SQL Server 2017 instance!
img: py.png # Add image post (optional)
tags: [Python,SQL,SSMS,Sql Server]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<br>
<br>
In a recent blog post I covered how to get started C# common language runtimes. I mentioned that the flavors available are c# and Visual basic, this technology began shipping in SQL 2005. 
<br>
<br>
However a more recent development in the SQL server world began shipping with SQL Server 2017 and this is availability for Python and R scripts to run natively in your SQL instance.
<br>
<br>
This is extremly interesting because now you can develop stored procedures that utalize all the machine learning models that you have developed natively. And your OLTP data can be ran through the models natively on your server.
<br>
<br>
To get things started I will just show a few high level steps of how to get your SQL instance primed for this.
<br>
<br>
Firstly your going to need a SQL server 2017 instance, I learned that SQL express editions do not carry the functionaility for Python and R scripting. So you can download a 180 eval edition <a href="https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2017-rtm" target="_blank" >here</a>.
<br>
<br>
The main thing to be concerned with here under feature selection is to click the check for Python under database engine services.
<br>
<br>

![Before](/assets/img/PythonS001.PNG)

<br>
<br>
Additionally when your instance has finished installing enable external scripts.
<br>
<br>
{% highlight SQL %}

sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  

{% endhighlight %}
<br>
<br>
Once that is complete, a restart for the instance will need to be performed.
<br>
<br>
![Before](/assets/img/PythonS002.PNG)
<br>
<br>

Now lets take a look at what a Python script looks like inside of SQL server.
<br>
<br>
Here is an example to check the location of the install for specifically the SQL Server 2017 Python install.
<br>
<br>
{% highlight SQL %}
EXEC sp_execute_external_script

  @language =N'Python',

  @script=N'import sys; print("\n".join(sys.path))'
{% endhighlight %}
<br>
<br>
A quick note here that if you run anaconda or Python on the client machine already, the dependencies that you have will not be carried over to this new SQL Server 2017 Python. It is completly different and you will need to manage the libraries seperately!
<br>
<br>
This code will give the install location so a simple way to install a new module so that you may use it in SQL server is :
<br>
<br>
{% highlight CMD %}
cd C:\Program Files\Microsoft SQL Server\MSSQL14.FOGELDEV2017\PYTHON_SERVICES\Scripts
pip.exe install text-tools
{% endhighlight %}
<br>
<br>