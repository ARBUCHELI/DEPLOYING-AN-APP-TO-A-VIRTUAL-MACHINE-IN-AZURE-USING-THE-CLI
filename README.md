# DEPLOYING-AN-APP-TO-A-VIRTUAL-MACHINE-IN-AZURE-USING-THE-CLI

(We are talking here about an app created with Python and Flask)

* Connect to the VM

```
ssh [ADMIN-NAME]@[PUBLIC-IP] 
```

* Install Python Virtual Environment and NGINX

```
sudo apt-get -y update && sudo apt-get -y install nginx python3-venv
```

* Configure Nginx to redirect all incoming connections on port 80 to our app that is running on localhost port 3000:

```
cd /etc/nginx/sites-available
```

```
sudo unlink /etc/nginx/sites-enabled/default
```

```
sudo vim reverse-proxy.conf
```

```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
 }
```

```
sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf
```

```
sudo service nginx restart
```

* Create a virtual environment
```
python3 -m venv venv 
```

* Activate the virtual environment
```
source venv/bin/activate 
```

* Upgrade pip in the virtual environment
```
pip install --upgrade pip 
```

* Install requirements
```
pip install -r requirements.txt 
```

* Run the app
```
python application.py 
```

* Cleanup
```
az group delete -n resource-group-west
```

* Config a file from the CLI
```
Press Esc, then type :wq! and press Enter to save and exit the file.
```

* Disconnect from the app service
```
exit
```

