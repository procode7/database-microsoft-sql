
# Microsoft SQL Server 2008 Opspack

Microsoft SQL Server is a relational database management system. As a database server, it is a software product with the primary function of storing and retrieving data as requested by other software applications which may run either on the same computer or on another computer across a network. Microsoft markets numerous editions of Microsoft SQL Server aimed at varying audiences/workloads (ranging from small single-machine applications to large Internet-facing applications) and our Opspack ensures that you can monitor your Microsoft SQL server to ensure it is operating correctly every step of the way.

## What You Can Monitor

Opsview Monitor contains all the important service checks to make sure your Microsoft SQL Server is up and running. Service Checks for Microsoft SQL Server include:

- Database size, memory, and agents checks
- Database Log Information
- SQL Stats including Server cache statistics and wait statistics
- SQL transactions per second, time to connect and more.

## Service Checks

| Service Check | Description |
|:------------- |:----------- |
|Active transactions | Check number of active transactions on DB   |  
|Check memory | Check memory without agent using WMI     |
|Database size | Check size of DB in KB's     |
|DB Agent Service | Checks Microsoft SQL Agent service is started   |  
|DB Browser | Checks Microsoft SQL Browser service is started     |
|Generic SQL server statistics | Check SQL server stats without agent using WMI     |
|List all DBs | List SQL cache types without agent using WMI     |
|List all lock types | List all lock types without agent using WMI    |
|List SQL cache types | List SQL cache types without agent using WMI   |  
|Lock statistics | Lock statistics without agent using WMI     |
|Log cache hit | Log cache hit percentage.     |
|Log flush time | How long to flush logs (ms)     |
|Log growths | Check log growths     |
|Server buffer manager | Check system buffer without agent using WMI    |
|Server Latch/SuperLatch statistics | Check SQL latch/superlatch stats without agent using WMI     |
|Server Listener | Checks Microsoft SQL Server on TCP port 1433    |
|Server Service | Checks Microsoft SQL Server Service is started  |  
|Server wait statistics | Server wait statistics without agent using WMI  |  
|SQL Server cache statistics | Check SQL server cache stats without agent using WMI    |
|Stats | SQL stats without agent using WMI     |
|Time to connect | Checks how long it takes to connect to DB (ms) |    
| Transactions per second | Number of transactions per second on this database |

## Setup and Configuration

The plugins check_mssql_database.py and check_wmi_plus.pl are used with this Opspack. check_wmi_plus.pl is delivered from the WMI Opspack. check_mssql_database.py is also included, but has some dependencies:

- Requires the Python MSSQL module. For Ubuntu and Debian, you need to install python-pymssql. For RHEL/CentOS, you will need to install pymssql. For SLES, you will need to source the dependency manually as it is not clear which package will provide this.

- Download Links:
  - [RHEL 5/CentOS 5 x64](http://rpm.pbone.net/index.php3/stat/4/idpl/15275777/dir/redhat_el_5/com/pymssql-1.0.2-2.el5.x86_64.rpm.html)
  - [RHEL 6/CentOS 6 x64](http://rpm.pbone.net/index.php3/stat/4/idpl/15698010/dir/redhat_el_6/com/pymssql-1.0.2-4.el6.x86_64.rpm.html)
  - [RHEL 5/CentOS 5 x86](https://pkgs.org/download/pymssql)
  - [RHEL 6/CentOS 6 x86](https://pkgs.org/download/pymssql)

Download the above .rpm files, and then use a command such as “yum –nogpgcheck localinstall pymssql-1.0.2-4.el6.i686.rpm”

For SLES: Download and compile using the source .tar.gz file [here](https://sourceforge.net/projects/pymssql/files/pymssql/1.0.2/)

Note: There have been issues during initial testing with monitoring from Ubuntu 12.04 LTS, due to differences in Python versions.
This Opspack has been tested on MS SQL Server 2008 R2 running on Windows Server 2008 R2.

### Network Dependencies
TCP port 1433 needs to be open from the Opsview Monitor server to connect to the monitored host. For more details, see the Microsoft document: http://support.microsoft.com/kb/287932

### Configuration
In Opsview Monitor, add your Microsoft SQL Database as a Host and apply the Host Template “Database - Microsoft SQL” to that host.

#### Creating a monitoring user

- Create a new “SQL Authentication user” on the server you are monitoring by logging into the “Microsoft SQL Server Management Studio”, and navigating to “localhost > Security > Logins” and right clicking on Logins and selecting “New Login…”.

- Populate the sections as below:

  - GENERAL: create a new login name (username), and change to “SQL Server Authentication”. Enter a password (must be complex, ex. numbers and upper case characters) and then remove “Enforce password expiration and “user must change password at new login”.

  - SERVER ROLES: Select “public” and “sysadmin”.
  - USER MAPPING: Leave as default.
  - SECURABLES: Click “Search..”, select “All objects of the types..”, and then “Servers” and click OK. Then scroll down the lists to find the permission titled “View server state”, and check “Grant”.
  - STATUS: Ensure “Grant” and “Enabled” are checked respectively.

### Variables

Add the MSSQLCREDENTIALS Variable to the host (the value here can be anything). Set arg1 to be the username of the database (e.g., sa) and arg2 to be the password.

We recommend that you do not use the sa user and instead create a specific user for monitoring. For further details, see Microsoft's documentation.

Try to avoid using passwords that contain the exclamation mark (!), as Nagios uses this as a special argument separator.

You will also need to set WINCREDENTIALS. Set arg1 to be the username of the server, eg. Administrator orDOMAIN\User. Set arg2 to be the password.

You will need to set MSSQLDATABASE for each database you want to monitor.

### Troubleshooting

If we see an error such as 'CRITICAL:Unable to access SQL Server', then you should check that the credentials you have specified are those for MSSQL (e.g. sa), and not the Windows credentials (e.g. Administrator)
