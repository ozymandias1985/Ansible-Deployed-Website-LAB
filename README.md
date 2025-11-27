Personal Website Deployment with Ansible Automated Static Website Setup 

🎯 Project Overview
This is a beginner-friendly Ansible project that automatically deploys a personal static website on an Ubuntu server. It sets up Nginx, configures firewall rules, deploys your website files, and ensures everything is secure and running.
Perfect for:

Learning Ansible basics with a real-world project
Automating web server setup
Hosting portfolio/resume websites
Understanding infrastructure as code
Adding to your GitHub portfolio

What This Automates:

✅ System updates and security patches
✅ Nginx web server installation and configuration
✅ Firewall (UFW) setup with proper rules
✅ Website file deployment
✅ Basic security hardening
✅ Service management and health checks


📋 Prerequisites
Required:

Ubuntu 20.04+ server (VM, cloud instance, or bare metal)
SSH access to the server with sudo privileges
Ansible installed on your control machine: sudo apt install ansible -y
SSH key-based authentication set up

Optional:

Your own domain name (works with IP address too)
Basic understanding of web servers (helpful but not required)


🚀 Quick Start
# 1. Clone this repository
git clone https://github.com/yourusername/ansible-website-deploy.git
cd ansible-website-deploy

# 2. Configure your server details
nano inventory/hosts.ini
# Add your server IP and SSH user

# 3. Customize your website
# Edit files in website_files/ directory or replace with your own

# 4. Run the playbook
ansible-playbook -i inventory/hosts.ini site.yml

# 5. Visit your website
# http://your-server-ip

ansible-website-deploy/
├── README.md                    # Project documentation
├── site.yml                     # Main playbook (entry point)
├── inventory/
│   └── hosts.ini               # Server inventory file
├── group_vars/
│   └── webservers.yml          # Variables for web servers
├── roles/
│   ├── common/                 # Basic server setup & security
│   │   └── tasks/
│   │       └── main.yml
│   ├── nginx/                  # Nginx installation & configuration
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   ├── templates/
│   │   │   └── site.conf.j2   # Nginx site configuration
│   │   └── handlers/
│   │       └── main.yml
│   └── website/                # Website deployment
│       ├── tasks/
│       │   └── main.yml
│       └── files/
└── website_files/              # Your actual website content
    ├── index.html
    ├── about.html
    ├── css/
    │   └── style.css
    └── images/
        └── logo.png

📝 Configuration

[webservers]
web1 ansible_host=192.168.1.100 ansible_user=ubuntu

[webservers:vars]
ansible_python_interpreter=/usr/bin/python3

2. Variables Configuration
Edit group_vars/webservers.yml:
# Website configuration
website_domain: example.com
website_port: 80
website_root: /var/www/html

# Server settings
server_admin_email: admin@example.com

3. Add Your Website Content
Replace the files in website_files/ with your own:

index.html - Homepage
about.html - About page
css/style.css - Stylesheets
images/ - Images and media


🎮 Usage

Deploy Website
ansible-playbook -i inventory/hosts.ini site.yml

Deploy to Specific Hosts
ansible-playbook -i inventory/hosts.ini site.yml --limit web1

Check What Will Change (Dry Run)
ansible-playbook -i inventory/hosts.ini site.yml --check

Verbose Output for Debugging
ansible-playbook -i inventory/hosts.ini site.yml -vvv

Update Only Website Content
ansible-playbook -i inventory/hosts.ini site.yml --tags "website"


🏷️ Available Tags
Run specific parts of the playbook:

common - Basic system setup
nginx - Nginx installation and config
website - Website deployment only
security - Security hardening only

Example:

ansible-playbook -i inventory/hosts.ini site.yml --tags "website,nginx"

🔧 What Each Role Does
Common Role

Updates system packages
Installs essential tools (vim, curl, git, etc.)
Configures UFW firewall
Sets up automatic security updates
Creates necessary user groups

Nginx Role

Installs Nginx web server
Configures virtual host from template
Enables and starts Nginx service
Sets up log rotation
Opens necessary firewall ports

Website Role

Copies website files to server
Sets correct permissions
Validates HTML (optional)
Creates backup of previous deployment
Restarts Nginx to apply changes


🔒 Security Features
This playbook includes basic security hardening:

UFW firewall enabled (only SSH and HTTP)
Automatic security updates configured
Nginx running as non-root user
Proper file permissions (644 for files, 755 for directories)
SSH port configurable (default: 22)

Note: For production, consider adding:

SSL/TLS certificates (Let's Encrypt)
Fail2ban for brute force protection
More restrictive firewall rules
Regular backup automation


🧪 Testing
Test on Local VM
# Using Vagrant (recommended for testing)
vagrant init ubuntu/focal64
vagrant up
# Update inventory with VM IP (usually 192.168.56.10)
ansible-playbook -i inventory/hosts.ini site.yml

Verify Deployment

# Check Nginx status
ansible webservers -i inventory/hosts.ini -m shell -a "systemctl status nginx"

# Check website is accessible
curl http://your-server-ip

# Verify firewall rules
ansible webservers -i inventory/hosts.ini -m shell -a "ufw status"



🐛 Troubleshooting
Connection Issues
# Test connectivity
ansible webservers -i inventory/hosts.ini -m ping

# Check SSH access manually
ssh user@your-server-ip

Nginx Not Starting
# Check Nginx configuration
ansible webservers -i inventory/hosts.ini -m shell -a "nginx -t"

# View Nginx logs
ansible webservers -i inventory/hosts.ini -m shell -a "tail -n 50 /var/log/nginx/error.log"

Permission Denied
Make sure your user has sudo privileges:
# On the target server
sudo usermod -aG sudo your-username

🚀 Customization Ideas

Add SSL/TLS:

Integrate Let's Encrypt with certbot
Update Nginx template for HTTPS


Multiple Websites:

Create additional roles for different sites
Use different virtual host configurations


Database Support:

Add MySQL/PostgreSQL role
Deploy database-backed applications


Monitoring:

Add Prometheus/Grafana roles
Set up log aggregation


CI/CD Integration:

Trigger playbook from GitHub Actions
Automated deployments on git push




📚 Learning Resources
Ansible Documentation

Official Docs
Best Practices
Module Index

Related Topics

YAML syntax
Jinja2 templating
Linux system administration
Nginx configuration


🤝 Contributing
Contributions are welcome! Feel free to:

Report bugs
Suggest new features
Submit pull requests
Improve documentation


📄 License
This project is licensed under the MIT License - see the LICENSE file for details.

🙏 Acknowledgments

Built as a learning project for Ansible beginners
Inspired by real-world infrastructure automation needs
Thanks to the Ansible community for excellent documentation


📬 Contact