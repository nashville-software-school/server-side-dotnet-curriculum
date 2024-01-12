# Deployment Tutorial for Digital Ocean

Digital Ocean provides cloud computing resources that you can use to serve your application code as well as the Postgresql database that goes with it. There are many ways that you can deploy an application, even within Digital Ocean, so this tutorial will not be an exhaustive overview of what's possible. This tutorial will create resources that are _not free_, and you are responsible for understanding and managing the costs of running your deployed application. 

## Sign up for an account
1. Sign up for an account [here](https://www.digitalocean.com/).
1. Create a project using the `+ New Project` button. 
1. Use the `Create` button at the top of the site, and choose `Droplets`.

## Creating a droplet
On Digital Ocean, a droplet is a like a virtual machine on which you can install and run software. 

1. On the `Create Droplets` page, choose the NY region (unless the SF region is closer to you geographically)
1. Choose the Ubuntu image for your OS
1. Select the Basic type, Regular CPU, and the 1GB memory tier (as of 2023 this costs $6/month)
1. Create a new SSH key (you can use the instructions given in the sidebar when you click `New SSH Key`). The name in those snippets is `id_rsa`. Use another name like `digital_ocean` in case your Github SSH key is already called `id_rsa`.  
1. for the `Hostname`, name the droplet after your app (kebab-case)
1. For the project, choose the project you created in the previous section. 
1. Create the droplet!

## Connecting to your droplet 
You can use `ssh` to connect to your droplet from your terminal. When you do that, you will be able to enter shell commands that will run on the droplet rather than on your computer. 

1. Open a terminal
1. Run `eval "$(ssh-agent -s)"`
1. Run `ssh-add ~/.ssh/digital_ocean` (if you named your key something else, use that name!)
1. You should now be able to use SSH to connect to your droplet! At the top of the dashboard for your droplet, you should be able to find the IP address for the droplet where it says `ipv4`. 
1. In you terminal run `ssh root@<the ip address>`
1.  If your ssh key is set up correctly, you should see something that starts with `Welcome to Ubuntu 23.04 (GNU/Linux 6.2.0-32-generic x86_64)`
1. At the bottom of what is printed to the console, you will see a prompt that ends with `#`. This prompt works the same as the prompt in your computer's terminal, but it end in `#` instead of `$` because you are logged in as the _root_ user of the machine. Root users have broad privileges such that it is considered good practice to create another, non-root user to do our work on the droplet. Let's do that now.

## Creating another non-root user
1. Run `adduser <username>` (you can choose the name)
1. You will be prompted to choose a password, make it strong. 
1. You will be prompted to add more information about the user, leave the rest as the default values
1. Give the user `sudo` privileges by running: `usermod -aG sudo <username>`
1. Allow the new user to use your SSH key by running this: `rsync --archive --chown=<username>:<username> ~/.ssh /home/<username>`
1. type `exit` to end your SSH session as `root`. 
1. Try to reconnect with SSH using: `ssh <username>@<ipaddress-for-droplet>`
1. Once reconnected, the first time you run any command with `sudo` you will periodically be prompted for the password you created for the new user. Enter it whenever prompted.
1. `sudo ufw allow OpenSSH`
1. `sudo ufw enable`

## Register a domain name
You will need a domain for your users to find your app on the web. There are many services with which to register a domain, at varying costs. In [this Digital Ocean article](https://docs.digitalocean.com/products/networking/dns/getting-started/dns-registrars/), you can find some common domain registrars, along with instructions on pointing to DO nameservers from those registrars. Once you've picked a registrar and registered a name, follow the instructions in the article to update the nameservers to use Digital Ocean's. After you have configured the nameservers, use the `Networking` tab on the Digital Ocean dashboard, choose the domain that you added, and add two `A` records for that domain. Use `@` for the first hostname (which will correspond to `your-domain.com`), and `www` for the second hostname (which will correspond to `www.your-domain.com`). You should be able to choose your droplet in the `Will Direct To` input. Leave the default TTL for both.

Once this process is complete, Digital Ocean's nameservers will be able to point requests to `your-domain.com` and `www.your-domain.com` to the IP address of your droplet.

## Install and configure `nginx`
`nginx` is a very popular HTTP server that we will use as a proxy server for both our front end code as well as the API.  
1. To install `nginx`, ssh into your DO droplet with the username you created before
1. Run `sudo apt update`
1. Run `sudo apt install nginx`
1. Adjust the firewall settings so that we can receive HTTP requests: 
    ``` bash
    sudo ufw allow 'Nginx HTTPS'
    sudo ufw allow 'Nginx HTTP'
    ```
### Configuration
1. `sudo nano /etc/nginx/sites-available/your-domain`
1. Paste the following code in the nano editor, changing `your-domain` to the new domain that you registered: 
    ``` 
    server {

            root /var/www/your-app;
            index index.html index.htm index.nginx-debian.html;

            server_name your-domain www.your-domain;

            location / {
                    try_files $uri /index.html =404;
            }

            location /api/ {
            proxy_pass https://localhost:5001;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection keep-alive;
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            }

	}
    ```
1. When you have made the edits, type `ctrl + X` to exit, and `Y` to save the changes. 
1. `sudo ln -s /etc/nginx/sites-available/your-domain /etc/nginx/sites-enabled/` to add the site to the enable sites. 
1. `sudo systemctl restart nginx`
1. `sudo apt install certbot python3-certbot-nginx`
1.  `sudo certbot --nginx -d your-domain -d www.your-domain` (change to your domain name)
1. `sudo systemctl restart nginx`

## Installing Postgresql

1. Run `sudo apt-get update`
1. Run `sudo apt-get install postgresql-15`
1. Run `sudo -u postgres psql template1`
1. Run `ALTER USER postgres with encrypted password 'your_password';`
1. Run `\q` to exit from psql
1. Run `sudo systemctl restart postgresql.service`

## Install the dotnet sdk
1. Run `sudo apt install dotnet-sdk-8.0`

## Clone your repository code
1. Run `mkdir ~/app && cd $_`
1. Run `git clone <https address for your repo>`
1. Run `cd <your project repo name>`
1. Run `dotnet restore`
1. Run `dotnet build`
1. Run `dotnet publish`

## Build the database
1. Install the dotnet Entity Framework cli tool: `dotnet tool install --global dotnet-ef`
1. Add it to the path (change username to the non-root user): 
    ``` bash
    cat << \EOF >> ~/.bash_profile
    # Add .NET Core SDK tools
    export PATH="$PATH:/home/username/.dotnet/tools"
    EOF
    ```
1. Update the bash session with the new path: `source ~/.bash_profile`
1. Edit `appsettings.json` in your app's source code using the `nano` program by running `nano appsettings.json` in the project directory.  
1. Add two properties, replacing `YourApp` with your app's name, and the `<>`s with the appropriate passwords: 
    ``` json
    "YourAppDbConnectionString": "<connection string for your database using the postgres password you set above>", 
    "AdminPassword": "<Initial password for your admin user in the app>"
1. When you have made the edits, type `ctrl + X` to exit, and `Y` to save the changes. 
1. If your initial migration is in the source code from Github, skip to the next step, otherwise, inside your project directory, run: `dotnet ef migrations add InitialCreate`
1. Run `dotnet ef database update`
1. Once you have migrated the database, you can remove the `AdminPassword` property from `appsettings.json`.  

## Run the .NET app as a service
We could run the app on the droplet like we run it on our local machines, but it's better to run it as a linux system service, so that if the droplet is restarted, we can rely on the .NET app restarting (as well other benefits to running the app as a service).

1. `cd /etc/systemd/system`
1. `sudo nano <your-app>.service`
1. Paste the following code in the `nano` editor, changing `your-app` to your app's name, and the username to the non-root linux user you created. NOTE: You may have used different naming conventions for creating your app's name with dotnet _and_/_or_ with Github. You should check to make sure that these file paths match what you have on the droplet:
    ``` 
    [Unit]
    Description=your-app

    [Service]
    WorkingDirectory=/home/username/app/your-app
    ExecStart=/usr/bin/dotnet /home/username/app/your-app/bin/Debug/net8.0/publish/YourApp.dll
    Restart=always
    RestartSec=10
    SyslogIdentifier=your-app
    User=username 
    Environment=ASPNETCORE_ENVIRONMENT=Production
    Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

    [Install]
    WantedBy=multi-user.target
    ```
1. When you have made the edits, type `ctrl + X` to exit, and `Y` to save the changes. 
1. `sudo systemctl enable <your-app>.service`
1. `sudo systemctl start <your-app>.service`
1. `sudo systemctl status <your-app>.service`
1. Look for an `active` status on the `Active` line of the output of the last command. If that's what you see, everything is working. If not, you should double-check the file paths in the `<your-app>.service` file. 

## Build the front-end project
We want to have a production-ready build of the react code as well. This is the version of your react app that the server will serve to client browsers. First we create the built project in the `build` directory, then copy it to `/var/www/your-app`, where the server will expect to find it. 

1. `cd ~/app/your-app/client`
1. `sudo apt install npm` 
1. `npm install`
1. `npm run build`
1. `sudo mkdir -p /var/www/your-app`
1. `sudo cp -r ./build/. /var/www/your-app`
1. `sudo chown -R $USER:$USER /var/www/your_app`
1. `sudo chmod -R 755 /var/www/your-app`

## Testing your site
1. Navigate to `your-domain.com`
1. If you see the login view, the nginx server is correctly serving the react code
1. Log in with the `AdminPassword` and admin user email that you configured for the project
1. If you successfull login, the server is also successfully proxying requests to the .NET API
1. Congratulations!



