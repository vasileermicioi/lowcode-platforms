## nginx

`sudo apt update`

`sudo apt install nginx`

```
sudo tee /etc/nginx/sites-available/mysubdomain.mydomain.com > /dev/null <<'EOF'
server {
    listen 80;
    server_name mysubdomain.mydomain.com;

    location / {
        proxy_pass         http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection 'upgrade';
        proxy_set_header   Host $host;
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
EOF
```

`sudo ln -s /etc/nginx/sites-available/mysubdomain.mydomain.com /etc/nginx/sites-enabled/`

`sudo rm /etc/nginx/sites-enabled/default`

`sudo nginx -t`

`sudo systemctl reload nginx`