#### Files to be moved and where to 
- cloud-config.yaml 
	- To be used within the creation of your droplets within digital ocean. 
	- Accessed by doing the following
		- 1) Clicking "Advanced Options" under "Choose Authentication Method"
		- 2) Copy and pasting the entirety of the cloud-config.yaml file into the box that states "Enter user data here..."
	- Make sure to change the following
		- name: web
		- primary_Group: web
		- ssh-authorized-keys: your key here with your email 
		- packages: nginx, ufw
		- runcmd:
```bash
- systemctl restart ssh
- systemctl restart systemd-journal
- sudo ufw allow http
- sudo ufw allow ssh
```
	
- hello.conf
	- This file will be put into your nginx `sites-available` folder then symbolically linked to your `sites-enabled` folder
	- The path should be `/etc/nginx/sites-enabled/hello.conf`
	- Change the following in the file:
		- `root /var/www/<site-file>`
		- `location /hey` should have a proxy_pass of http://127.0.0.1:8080/hey
		- `location /echo` should have a proxy_pass of http://127.0.0.1:8080/echo
	- The ln symbolic link should be done with the following command 
```bash
sudo ln -s /etc/nginx/sites-available/hello.conf /etc/nginx/sites-enabled
```

- hello-server
	- This file will be put into your backend folder in `/var/www`
	- The path should be `/var/www/backend/hello-server`

- hello-server.service 
	- This file will be put into your `/etc/systemd/system` folder 
	- The path should be `/etc/systemd/system/hello-server.service`
	- You will need to enable changes to the folder so that your web accounts can access and add files to the folder. 
	- You need to change the file with the following lines. Add these lines under the following sections. If the section doesn't exist, copy and paste it with the section.
```bash
[Service]
ExecStart=/var/www/backend/hello-server

[Install]
WantedBy=mult-user.target
```

- index.html 
	- This html file will be put into your site folder. 
	- The site folder will be a directory made under `/var/www/<site-file>`
	- The file path should be something like `/var/www/<site-file>/index.html`

- curl.md
	- Just make sure to change the ip addresses to your load balancers. 