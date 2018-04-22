## Laravel Homestead Configuration - Quick Setup Guide

* Install VirtualBox from [here](https://www.virtualbox.org/wiki/Downloads)

* Install Vagrant from [here](https://www.vagrantup.com/downloads.html)

* Open a terminal window on your local machine

* Install Homestead Vagrant Box by executing the below command from your command line.

```
vagrant box add laravel/homestead
```

* Install Homestead using below command

```
git clone https://github.com/laravel/homestead.git ~/Homestead
```


* This will clone the homestead directory in your home folder

* Navigate to the Homestead directory using

```
cd ~/Homestead
```

* As mentioned in the Laravel Page, checkout the correct version of Homestead branch by executing the below command.

```
git checkout v7.3.0
```

* From the command line terminal window, run the below command from the Homestead directory.

```
bash init.sh  (For Mac or Linux)
```  

```
init.bat  (For Windows)
```

* This creates the ```Homestead.yaml``` file. Its a configuration file for the Homestead box

* At this point, you can check if your homestead box is up and running.

* From the command line run the command  from the Homestead folder, to bring up the vagrant box.

```
vagrant up
```

* And then SSH into the vagrant homestead box using the below command.

```
vagrant ssh
```

This will log you into the vagrant machine containing the homestead box.

* The default folder structure inside the homestead box is of the type, ```/home/vagrant```. You can create a folder here inside the vagrant directory to store the code and sync folders with your local machine.

Leave the vagrant box, by typing ```exit``` from the vagrant box terminal

* Now go back to your local machine and open the ```Homestead.yaml``` file and make sure the contents are as mentioned below:

```
---
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/mavbasic/
      to: /home/vagrant/mavbasic/

sites:
    - map: mavbasic.app
      to: /home/vagrant/mavbasic/public

databases:
    - homestead
    - mavbasic
# blackfire:
#     - id: foo
#       token: bar
#       client-id: foo
#       client-token: bar

# ports:
#     - send: 50000
#       to: 5000
#     - send: 7777
#       to: 777
#       protocol: udp
```

 * If you check your version of ```Homestead.yaml``` file, some of the configuration options would have been already done.

 * The first line specifies the IP for the localhost and is for running our application on localhost. We will update this in our hosts file later. Make sure you set it to the value as shown below.

 ```
 ip:"192.168.10.10"
 ```

 * Ensure, that the provider variable is set. This is applicable if you installed VirtualBox. In case you installed VMWare, the provided would change accordingly.

 ```
 provider: virtualbox
 ```

 * Next, in the ```folders``` section, we will mention the local and virtualbox file system and sync them together.
    * The ```map``` section should specify the folder where you cloned your repository on your system.

    * The ```to``` section specifies the folder inside the virtualbox where your code repository will be synced. By default, it contains ```/home/vagrant/```. You can create a folder inside that directory in the virtual box and mention it here.

    * Assuming, I cloned my repository in home folder inside mavbasic and want to sync that folder to the virtualbox filesystem path "/home/vagrant/mavbasic/", it would be as shown below. This can be any folder with a name of your choice.

    ```
    folders:
        - map: ~/mavbasic/
          to: /home/vagrant/mavbasic/
    ```

* Next in the ```sites``` section, we will specify the name of the application, by which we want to run it in our localhost. This can be any name you want.
    * In the ```map``` section mention the name of the application you want. This can be any name you choose to be.

    * In the ```to``` section mention the absolute path to the public folder. This path should be the absolute path inside our Homestead VirtualBox. The public folder is where laravel will pick up the application ```index.php``` file.

    * Assuming I named my app as "mavbasic", the settings would be as shown below.

    ```
    sites:
        - map: mavbasic.app
          to: /home/vagrant/mavbasic/public
    ```

* By default, Homestead creates databases of the name "homestead". We can create new database by just specifying the name of the database below. This will create both mySQL and postgreSQL databases when we bring up the homestead box.

* The rest of the lines can stay commented and are not of relevance to us in this setup.

* Once you have done these changes, exit from the Vagrant box if you are already logged in, using the ```exit``` command.  

* Execute the below command from your local machine's terminal from the Homestead directory. This will reload the Homestead configuration from the ```Homestead.yaml``` file and our changes will take effect.

```
vagrant reload --provision
```

from your terminal on the local machine.

* Log into the vagrant machine again using the below command.

```
vagrant ssh
```

* You should now be able to see your cloned repository inside the vagrant machine.

### Below instructions are only application for setting up the development environment on your machine

* Download and Install PHPStorm from [here](https://www.jetbrains.com/phpstorm/)

* Assuming, you have cloned your repository, from github, open the code in PHPStorm.

* Rename the default ```.env.example``` file to ```.env```

* From the terminal inside your vagrant machine, generate a new key using the below command

```
php artisan key:generate
```

* To configure the local database and connect it to the database created by Homestead box, make sure you add the below configuration in the ```.env``` file

```
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=<DB Name mentioned in the Homestead.yaml file>
DB_USERNAME=homestead
DB_PASSWORD=secret
```

* The username and password are default created by Homestead box for the database.

* Using PostgreSQL is better than mySQL, as we will be deploying in Heroku and use PostgreSQL services. This will ensure our local environment is similar to the hosted environment.

#### Update Hosts file.

* Open the hosts file as an administrator in your local machine.

```
/etc/hosts (in Mac & Linux)

C:\Windows\System32\Drivers\etc\hosts  (Windows)
```

* Add the below line to your hosts file and save it.

```
192.168.10.10   mavbasic.test   
```

* The application name here should be the same as the one mentioned in  in the ```sites``` section of your ```Homestead.yaml``` file.

> Due to some recent changes in the Google Chrome browser, this feature has sometimes not worked properly. It has required to change the application name to ```.test``` instead of ```.app```.

> In case the browser complaints about insecure connection, when you access the application, go ahead and click on "Advanced" and "Proceed" to the page. If "mavbasic.test" does not work, try "mavbasic.dev"

* Go to the terminal in your vagrant box and navigate to the location where you cloned the repository and run the below command. This will install all the packages required to run the application.

```
composer update
```

* Once this is completed, go to your browser, and type the name of the the application, ```mavbasic.test``` in this example, to access the application on your local machine.

* One additional useful feature, is that we can configure our local database within PHPStorm.

* From the top menu bar, go to

```
View > Tool Windows > Database
```

The database will show up on the right hand side.

* Click on the Settings icon, which says "Data Source Properties". This will open the configuration window for the local database.

* At the left top, click on the "+" icon, and select PostgreSQL.

* Specify the configuration parameters for this local database as below:

```
Host: localhost
Database: <Name of your local database, as mentioned in the Homestead.yaml file>
User: homestead
Password: secret
Port: 54320
```

* If it requires for Drivers to be installed, install the required drivers, and make sure you mention the Port as "54320" and not "5432" in this case alone. This is needed to connect PHPStorm to the local database.

* You can click on "Test Connection" to ensure, that the database is connected.

* Note: All ```php artisan``` commands, should be run from the terminal inside the homestead box. All your git commands can be run from the local machine's terminal or from the vagrant box's terminal.

That's it. The development environment is setup, database is connected, and application should be running on localhost on your browser.
