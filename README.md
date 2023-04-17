# Lab 5: Web Security
By Luis Daniel Casais Mezquida  
Cybersecurity Engineering 22/23  
Bachelor's Degree in Computer Science and Engineering, grp. 89  
Universidad Carlos III de Madrid

## Project statement
The goal of this lab is to practice some of the web security concepts studied in class.  

To do the lab, you will need the same setting used in [Lab 4](https://github.com/ldcas-uc3m/CE-Lab4) (or see [Installation and execution](#installation-and-execution)).  
In the following, we assume the Metaexploitable2 machine runs in `192.168.56.3`.

### Activities
1. Open a browser window and connect to `http://192.168.56.3`. Use the credentials `admin:password` to log in.  
Click on `DVWA Security` and select `Low`. Click on `Command Execution` and then on `View Source`. If you don’t know php, search for the functions and statements in the code and be sure you understand them.
    1. Describe what vulnerability is present in this code and illustrate with an impactful example how to exploit it.
    2. **Bonus:** repeat the step above but selecting `Medium` and `High` in the `DVW Security` flag.
2. Click on `SQL Injection` with `DVWA Security` set to `Low`. Analyze the source code and determine what vulnerability is present.
    1. Perform a basic SQL injection to retrieve all tables in the MySQL database. (Hint: find out how this is named in MySQL.)
    2. Using the output of the previous step, locate the table listing all users and find out all the column fields for each user.
    3. Retrieve the list of users and their passwords (hashes)
    4. **Bonus:** use John the Ripper to break one password (put `username:hash` in `pwd.txt`, and run `john --format=raw-MD5 pwd.txt`)
3. Click on `CSRF` with `DVWA Security` set to `Low` and check how the password is changed (look at the HTTP method and how the username and new password is sent). Take note of this.
    1. Open up a terminal window and use `curl` to change the password for an arbitrary user using what you have learned in the CSRF box. Can you do it? Why?
    2. Click on `XSS Reflected`, inspect the source code, and figure out how to exploit it to retrieve the session cookie (`document.cookie`).
    3. Repeat the first step using the stolen cookie to change the password. Does it work?


## Installation and execution

First configure the VMs.

1. Get a [kali VM image](https://www.kali.org/get-kali/#kali-virtual-machines) and install the VM in [Virtual Box](https://www.virtualbox.org/). Just use `Machine` > `Add` and select the `.vbox` file.  
The credentials for this machine are `kali:kali`.  
Note that you can use any other Linux distro, just make sure you install [`nmap`](https://nmap.org/) and [`netcat`/`nc`](https://netcat.sourceforge.net/).
2. Download the [Metaesploitable2](https://sourceforge.net/projects/metasploitable/) distribution and create a VM with it in Virtual Box.
    1. Create a new VM with `Machine` > `New`. Use `Type`: `Linux` and `Version`: `Other Linux (64-bit)`.
    2. Default hardware config (512MB RAM and 1 CPU) should do.
    3. On `Virtual Hard Disk` select `Use an Existing Virtual Hard Disk File` and add and choose the `Metaexploitable.vmdk` file.

    The credentials for this machine are `msfadmin:msfadmin`.
3. Create a virtual network (`Tools` > `Network` > `Create`) on the `192.168.56.1/24` IP address range, and enable the DHCP Server.  
    1. Configure the adapter manually as such:
        - `IPv4 Adress`: `192.168.56.1`
        - `IPv4 Network Mask`: `255.255.255.0`
    2. Configure the DHCP server as such:
        - `Server Adress`: `192.168.56.2`
        - `Server Mask`: `255.255.255.0`
        - `Lower Address Bound`: `192.168.56.3`
        - `Upper Address Bound`: `192.168.56.254`

4. Attach both machines to this network (`Machine` > `Settings` > `Network` → `Attached-to`: `Host-only Adapter`, and select the virtual network name from the drop down menu).
5. Run both machines and check their IPs with `ifconfig`. Remember you can change the keyboard layout in `Settings` > `Keyboard` > `Layout`.
6. Check the machines reach each other by performing a `ping` from one machine to the other.
