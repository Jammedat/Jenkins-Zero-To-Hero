Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:



1. Access the Jenkins UI by clicking on the Jenkins button in the top bar. Log in using the credentials: username admin and password Adm!n321.


2. Create a new Jenkins job named install-packages and configure it with the following specifications:


Add a string parameter named PACKAGE.
Configure the job to install a package specified in the $PACKAGE parameter on the storage server within the Stratos Datacenter.

### Solution
### **1. Create the Job**

1. Go to **Jenkins Dashboard**

2. Click **New Item**

3. Enter name: **install-packages**

4. Choose **Freestyle project**

5. Click **OK**

***

###  **2. Add a String Parameter**

1. Inside the job config page, scroll to:\
   **"This project is parameterized"**

2. Check the box.

3. Click **Add Parameter → String Parameter**

4. Fill like:

```vbnet
Name: PACKAGE
Default Value: (can be empty, or example: tree)
Description: Name of package to install
```

***

### **3. Install necessary plugins like**
- SSH
- Parameterized Remote Trigger
- Terminal
- Credentials
- Credentials Binding

### **4. Configure the job to install the package on the storage server**

Use **Execute shell**, and SSH directly from Jenkins node:

Scroll to **Build → Add Build Step → Execute Shell**

Add:

```bash
sshpass -p '<password>' ssh -o StrictHostKeyChecking=no natasha@ststor01 "echo '<password>' | sudo -S yum install -y $PACKAGE"
```


Anotrher way, not hardcoding the sensitive information:  
```bash
sshpass -p "$PASSWORD" ssh -o StrictHostKeyChecking=no $USERNAME@ststor01 "echo "$PASSWORD" | sudo -S yum install -y $PACKAGE"
```

### Scheduled job for copying files 
Configure the job by selecting build periodically and insert the schedule:  
`H/10 * * * *`
This will schedule the job to run periodically every 10 minutes.



```bash
sshpass -p "$pass_app_s2" ssh -o StrictHostKeyChecking=no \
$user_app_s2 "echo "$pass_app_s2" | sudo sshpass -p "$pass_storage" scp /var/log/httpd/access_log  /var/log/httpd/error_log $user_storage:/tmp" 
sshpass -p "$pass_storage" ssh -o StrictHostKeyChecking=no $user_storage \
"echo "$pass_storage" | sudo cp /tmp/access_log /tmp/error_log /usr/src/sysops"
```

**Note:** To know more about schedule see below:  

This field follows the syntax of cron (with minor differences). Specifically, each line consists of 5 fields separated by TAB or whitespace:

```
MINUTE HOUR DOM MONTH DOW
```

|        |                                                     |
| ------ | --------------------------------------------------- |
| MINUTE | Minutes within the hour (0–59)                      |
| HOUR   | The hour of the day (0–23)                          |
| DOM    | The day of the month (1–31)                         |
| MONTH  | The month (1–12)                                    |
| DOW    | The day of the week (0–7) where 0 and 7 are Sunday. |

To specify multiple values for one field, the following operators are available. In the order of precedence,

* `*` specifies all valid values
* `M-N` specifies a range of values
* `M-N/X` or `*/X` steps by intervals of X through the specified range or whole valid range
* `A,B,...,Z` enumerates multiple values
