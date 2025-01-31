# nginx-setup-deployment
# Configuring NGINX on a Fresh Ubuntu Server: My Experience

Setting up NGINX on a fresh Ubuntu server is a fundamental task for any DevOps engineer. Recently, I had to configure NGINX on a brand-new Ubuntu instance and serve a custom HTML page as the default page. While the process is straightforward, I encountered some challenges and learned a few valuable lessons along the way. In this blog post, I’ll share my experience, step-by-step setup, and troubleshooting tips.

## 🚀 Setting Up the Environment
To start, I spun up a new Ubuntu server instance on Google Cloud Platform (GCP) Compute Engine. After provisioning the server, I connected to it via Google Cloud SSH:
If you're using any other cloud platform, you can SSH into it using the IP address:

```bash
ssh ubuntu@your-server-ip
```

## 🚀 Installing NGINX
NGINX is not installed by default on Ubuntu, so the first step I did was to update the package list and install it:

```bash
sudo apt update
sudo apt install nginx -y
```

Once installed, I checked the status to ensure it was running:

```bash
sudo systemctl status nginx
```

It wasn’t running, so I started and enabled it to run on boot:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

## 🚀 Creating a Custom HTML Page ✨
NGINX serves files from `/var/www/html`. So to customize the default page, I created an `index.html` file inside this directory:

```bash
sudo vim /var/www/html/index.html
```

**NB:** We have two major text editors which we use on the terminal; Vim and Nano (I just prefer using Vim).

I added a simple HTML page:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome To My HTML Page!</title>
    <style></style>
</head>
<body>
    <div style="font-size: 8rem;">Welcome To DevOps Stage 0 - Fave💕✨</div>
</body>
</html>
```

After saving the file, I needed to restart NGINX for the changes to take effect:

```bash
sudo systemctl restart nginx
```

## 🚀 Configuring NGINX to Serve the Custom Page
In some cases, the default NGINX configuration might not serve the custom HTML page correctly. So I verified that the default server block in `/etc/nginx/sites-available/default` was set up properly:

```bash
sudo nano /etc/nginx/sites-available/default
```

I ensured the configuration looked like this in the default file:

```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html;
    server_name _;
  
    location / {
        try_files $uri $uri/ =404;
    }
}
```

After saving the changes, I tested the configuration:

```bash
sudo nginx -t
```

Everything was fine, so I reloaded NGINX:

```bash
sudo systemctl reload nginx
```

## 🚀 Testing the Setup
To verify that everything worked correctly, I accessed the server’s public IP address in a web browser:

```
http://34.59.6.191
```

I was greeted with my custom HTML page!! 😌💅
![Screenshot 2025-01-31 180830](https://github.com/user-attachments/assets/65b80bd7-8afb-4749-b697-93e5882fa7b2)


## 🚀 Challenges I Faced
While the process might seem smooth, I encountered some challenges and I had to troubleshoot:

- **NGINX not starting:** Running `sudo nginx -t` helped identify if I missed any steps or configurations.
- **Page not updating:** Clearing the browser cache or restarting NGINX usually fixed this.
- **Firewall blocking requests:** I had to edit the firewall rule on my instance to only serve port 80. Ensuring the firewall allowed HTTP traffic solved the issue.

## 🚀 References:
- [DevOps Engineers](https://hng.tech/hire/devops-engineers)
- [Cloud Engineers](https://hng.tech/hire/cloud-engineers)

## 🚀 Conclusion
Configuring NGINX on a fresh Ubuntu server to serve a custom HTML page was a great learning experience. While the setup process was mostly smooth, troubleshooting minor issues reinforced my understanding of how NGINX handles requests and configurations. If you're setting up a web server for the first time, following these steps should help you get started quickly.

Thanks 😌🔥
