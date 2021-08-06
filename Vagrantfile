# -*- mode: ruby -*-
# vi: set ft=ruby :

# author Alexandre Cunha <contato@alexandreti.com.br>
# version 0.1.0
# Vangrantfile to deploy MS-SQL Express
#

$machine_image  = "ubuntu/focal64"

$machine_name   = "ubuntu01"
$machine_memory = "2048"
$machine_cpus   = "4"
$machine_ip     = "10.10.10.143"
$vb_group       = "/MS-SQL-Express-Lab"
$mssqlpass      = "padrao@1234"

$script         = <<-SCRIPT
    # Definir senha denha do SA (Super Administrator) do MS-SQL
    sudo apt-get update
    sudo apt-get upgrade -y
    # Install MS-SQL Express
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
    sudo apt-get update
    sudo apt-get install -y mssql-server
    sudo ACCEPT_EULA='Y' MSSQL_PID='Express' MSSQL_SA_PASSWORD='padrao@1234' MSSQL_TCP_PORT=1433 /opt/mssql/bin/mssql-conf setup
    systemctl status mssql-server --no-pager
    #
    #
    # Install mssql-tool (sqlcmd)
    sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list)"
    sudo apt-get update
    ##sudo ACCEPT_EULA=Y apt-get install -y msodbcsql17
    # optional: for bcp and sqlcmd
    sudo ACCEPT_EULA=Y apt-get install -y mssql-tools
    echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
    export PATH="$PATH:/opt/mssql-tools/bin"
    #
    #
    # Test MS-SQL Express conection by sqlcmd
    sqlcmd -S localhost -U SA -P "$MSSQLPASS" -Q "select @@VERSION;"
    sqlcmd -S localhost -U SA -P "$MSSQLPASS" -Q "SELECT Name from sys.Databases;"
    #
    #
    sqlcmd -S localhost -U SA -P "$MSSQLPASS" -Q \
       "USE master;
        CREATE DATABASE TestDB;"
    #
    #
    sqlcmd -S localhost -U SA -P "$MSSQLPASS" -Q \
       "USE TestDB;
        CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT);
        INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
        SELECT * FROM Inventory;
        SELECT * FROM Inventory WHERE quantity > 152;"
    #
    #
    sqlcmd -S localhost -U SA -P "$MSSQLPASS" -Q \
       "USE master;
        DROP DATABASE TestDB;"
    #
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.define "machine" do |machine|
    machine.vm.box = $machine_image
    machine.vm.hostname = $machine_name
    machine.vm.network "private_network", ip: $machine_ip
    machine.vm.network "forwarded_port", guest: 1433, host: 1433, protocol: "tcp"
    machine.vm.provider "virtualbox" do |vb|
      vb.memory = $machine_memory
      vb.cpus = $machine_cpus
      vb.customize ["modifyvm", :id, "--groups", $vb_group]
    end
    machine.vm.provision "shell",
      env: {"MSSQLPASS" => $mssqlpass},
      inline: $script

  end
end