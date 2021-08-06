# vagrant-mssql-express

Deploy a test environment with Ubuntu 20.04 and MS SQL Express.


## Author

- [@jacunha](https://www.github.com/jacunha)


## Requirements
- [Virtualbox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)


## How to use

Clone this repository and up the infrastructure using vagrant.
In folder `vagrant-mssql-express`:

```bash
  vagrant up
```

The MS SQL Express listen on `127.0.0.1:1433` or `10.10.10.143:143`.

The tool `sqlcmd` virtual machine console using `vagrant ssh`.


Example using `sqlcmd`:
```bash
vagrant@ubuntu01:~$ sqlcmd -S localhost -U SA -Q "select @@VERSION;"
Password:


-----------------------------------------------------------------------
Microsoft SQL Server 2019 (RTM-CU12) (KB5004524) - 15.0.4153.1 (X64)
        Jul 19 2021 15:37:34
        Copyright (C) 2019 Microsoft Corporation
        Express Edition (64-bit) on Linux (Ubuntu 20.04.2 LTS) <X64>

(1 rows affected)
vagrant@ubuntu01:~$
```

To stop guest machine:
```bash
vagrant halt
```

To destroy guest machine:
```bash
vagrant destroy -f
```
