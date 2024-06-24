**Technical Guide to Run platform on your local system**

Running a 360-degree Video Platform project (360 Vision Pro) from start to finish involves several steps, from setting up your local environment to downloading the project from GitHub and running it. 

**Here’s a step-by-step guide:**

1. **Install Xampp**
- Install Xampp from <https://www.apachefriends.org/index.html>

  ![](1.png)


- Run the Xampp installer and open the Xampp control panel
- Make sure that you enable the Apache and MySQL services

  ![](2.png)


1. **Install PHP:** 
- Make sure PHP is installed on your system.
- For Windows: Download and install PHP from php.net.

  ![](3.png)


- Verify installation:

  ![](4.png)

- For macOS: Use Homebrew: brew install php.
- For Linux: Use your package manager, e.g., sudo apt install php for Ubuntu.

1. **Install Composer**
- Go to <https://getcomposer.org/download>

  ![](5.png)

- Verify installation:

  ![](6.png)

- On Windows, download and run the installer
- On Mac, copy the php commands and run in the terminal. Then copy the mv command and run in terminal. You can also install composer with Homebrew

1. **Install Laravel:** 
- Install the Laravel installer globally via Composer.

  bash

  - *composer global require laravel/installer*
- Ensure composer and laravel commands are available: Add Composer's global bin directory to your system’s PATH.
  - For Windows: Add 

    C:\Users\<Your-User>\AppData\Roaming\Composer\vendor\bin to your PATH.

  - For macOS and Linux: 

    Add export PATH="$PATH:$HOME/.composer/vendor/bin" to your ~/.bash\_profile or ~/.zshrc.

**Steps to Run a Laravel Project from GitHub**

1. **Clone the Project from GitHub:**
- Navigate to the directory where you want to store your project.
- Clone the repository using Git.

  ![](7.png)

- bash

  ***git clone https://github.com/bijanstha7/360-Vision-Pro.git***

  ***cd repository***

1. **Install Dependencies:**
- Navigate to the project directory and install dependencies via Composer.
- bash

  ***cd path/to/your/laravel/project***

  ***composer install***

1. **Set Up Environment Configuration:**
- Copy the .env.example file to .env.
- bash

  ***cp .env.example .env***

- Generate an application key.
- bash

  ***php artisan key:generate***

- Configure your .env file with the necessary environment variables (e.g., database credentials).

1. **Set Up the Database:**
- Ensure your database server is running and create a new database for your project.
- Update your .env file with your database details.
- plaintext

  ***DB\_CONNECTION=mysql***

  ***DB\_HOST=127.0.0.1***

  ***DB\_PORT=3306***

  ***DB\_DATABASE=your\_database\_name***

  ***DB\_USERNAME=your\_database\_user***

  ***DB\_PASSWORD=your\_database\_password***

- Run database migrations.
- bash

  ***php artisan migrate***

1. **Run the Development Server:**
- Start the Laravel development server.
- bash

  ***php artisan serve***

- Open your browser and go to http://localhost:8000 to see your Laravel application running.




**Technical Guide to Deploy platform on Google Cloud Platform**

Deploying 360-degree Video Platform project (360 Vision Pro) from start to finish with a MariaDB database on Google Cloud Platform (GCP) involves several steps. Here’s a comprehensive guide to walk you through the entire process:

**Step 1: Set Up Google Cloud Platform**

- **Create a GCP Account:**
  - If you don’t have a GCP account, create one at <https://cloud.google.com/>. 
  ![](8.png)

- **Create a New Project:**
  - Go to the GCP Console, select the project dropdown and click on “New Project”.
  - Enter a project name and create the project.

    ![](9.png)

**Step 2: Set Up a Virtual Machine (VM)**

1. **Navigate to Compute Engine:**
   - In the GCP Console, navigate to Compute Engine > VM instances. 

     ![](10.png)


   - Create a New VM Instance:
   - Click on “Create Instance”.

![](11.png)

1. **Configure the instance:**
- Name: vision 360
- Region and Zone: Select your preferred region and zone.
- Machine type: Choose a machine type according to your requirements (e.g., e2-medium).
- Boot disk: Click on “Change” to select an OS. Choose Debian or Ubuntu.
- Allow HTTP and HTTPS traffic under "Firewall".
- Click on “Create”.

![](12.png)


**Step 3: Install Necessary Software on the VM**

1. **SSH into the VM:**
- Click on the SSH button next to your VM instance to open a terminal.

![](13.png)

- Update the Package List:

  ***sudo apt update***

![](14.png)

1. **Install Apache, PHP, and Required PHP Extensions:**
- ***sudo apt install apache2***
- ***sudo apt install php php-cli libapache2-mod-php php-mysql php-xml php-mbstring php-zip unzip***

1. **Install Composer:**
- ***php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"***
- ***php composer-setup.php --install-dir=/usr/local/bin --filename=composer***
- ***rm composer-setup.php***

**Step 4: Set Up MariaDB**

1. Install MariaDB Server:
- ***sudo apt install mariadb-server***

1. Secure MariaDB Installation:
- ***sudo mysql\_secure\_installation***

1. Log into MariaDB and Create Database:
- ***sudo mysql -u root -p***




1. sql
- ***CREATE DATABASE 360\_video\_project;***
- ***CREATE USER 'laravel\_user'@'localhost' IDENTIFIED BY 'your\_password';***
- ***GRANT ALL PRIVILEGES ON laravel\_db.\* TO 'laravel\_user'@'localhost';***
- ***FLUSH PRIVILEGES;***
- ***EXIT;***

**Step 5: Deploy Laravel Application**

1. Navigate to Apache Root Directory:
- ***cd /var/www/html***

1. Clone Your Laravel Application from GitHub:
- **sudo git *clone https://github.com/bijanstha7/360-Vision-Pro.git***
- **sudo mv your-repository laravel-app**
- **cd laravel-app**

1. Install Laravel Dependencies:
- ***sudo composer install***

1. Set Up Environment File:
- ***sudo cp .env.example .env***
- ***sudo nano .env***

1. Update the .env file with your database credentials:
- makefile
- Copy code
- ***DB\_CONNECTION=mysql***
- ***DB\_HOST=127.0.0.1***
- ***DB\_PORT=3306***
- ***DB\_DATABASE=laravel\_db***
- ***DB\_USERNAME=laravel\_user***
- ***DB\_PASSWORD=your\_password***


1. Generate Application Key:
- ***sudo php artisan key:generate***

1. Run Database Migrations:
- ***sudo php artisan migrate***

**Step 6: Configure Apache for Laravel**

1. Create a New Virtual Host Configuration File:
- ***sudo nano /etc/apache2/sites-available/laravel-app.conf***

1. Add the Following Configuration:

apache

Copy code

***<VirtualHost \*:80>***

`    `***ServerAdmin webmaster@localhost***

`    `***DocumentRoot /var/www/html/laravel-app/public***

`    `***<Directory /var/www/html/laravel-app>***

`        `***Options Indexes FollowSymLinks***

`        `***AllowOverride All***

`        `***Require all granted***

`    `***</Directory>***

`    `***<Directory /var/www/html/laravel-app/public>***

`        `***Options Indexes FollowSymLinks***

`        `***AllowOverride All***

`        `***Require all granted***

`    `***</Directory>***

`    `***ErrorLog ${APACHE\_LOG\_DIR}/error.log***

`    `***CustomLog ${APACHE\_LOG\_DIR}/access.log combined***

***</VirtualHost>***



1. Enable the Site and Rewrite Module:
- ***sudo a2ensite laravel-app.conf***
- ***sudo a2enmod rewrite***
- ***sudo systemctl restart apache2***

**Step 7: Test Your Laravel Application**

1. Access Your Application:
- Open a web browser and navigate to the **external IP** of your VM instance (found in the VM instance details).

1. Verify Everything is Working:
- Ensure the Platform homepage is displayed.
