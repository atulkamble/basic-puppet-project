Below is a **basic, beginner-friendly Puppet project** that you can run on an **Ubuntu EC2 instance**. This is perfect for demos, labs, or first-time Puppet practice (agent–masterless mode).
I’ll keep it **simple, practical, and interview-ready**.

---

## 🎯 Project Goal

Use **Puppet** to:

* Install **Apache (apache2)**
* Ensure the service is **running & enabled**
* Create a **custom index.html**
* Manage everything using a **Puppet manifest**

---

## 🏗️ Architecture (Simple & Real-World)

![Image](https://www.puppeteers.net/wp-content/uploads/2023/11/Puppet-agent-server-architecture.png?utm_source=chatgpt.com)

![Image](https://aosabook.org/static/puppet/dataflow.png?utm_source=chatgpt.com)

![Image](https://www.ais.com/wp-content/uploads/2021/09/Puppet-Architecture-scaled.jpg?utm_source=chatgpt.com)

👉 We’ll use **Puppet Standalone (No Master)**

* Ideal for **EC2, labs, demos**
* Faster to set up
* Very commonly asked in interviews

---

## 🧰 Prerequisites

* AWS EC2 (Ubuntu 20.04 / 22.04)
* Security Group:

  * SSH (22)
  * HTTP (80)
* User: `ubuntu`

---

## 🔹 Step 1: Launch Ubuntu EC2

SSH into your instance:

```bash
ssh -i key.pem ubuntu@<EC2-Public-IP>
```

---

## 🔹 Step 2: Install Puppet on Ubuntu

```bash
sudo apt update
sudo apt install -y wget
wget https://apt.puppet.com/puppet7-release-focal.deb
sudo dpkg -i puppet7-release-focal.deb
sudo apt update
sudo apt install -y puppet-agent
```

Add Puppet to PATH:

```bash
echo 'export PATH=/opt/puppetlabs/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

Verify:

```bash
puppet --version
```

---

## 📁 Step 3: Create Project Structure

```bash
mkdir -p puppet-apache/{manifests,files}
cd puppet-apache
```

```bash
tree
```

```
puppet-apache
├── manifests
│   └── site.pp
└── files
    └── index.html
```

---

## 📝 Step 4: Create Apache Puppet Manifest

### `manifests/site.pp`

```puppet
package { 'apache2':
  ensure => installed,
}

service { 'apache2':
  ensure => running,
  enable => true,
  require => Package['apache2'],
}

file { '/var/www/html/index.html':
  ensure  => file,
  content => "Hello from Puppet on Ubuntu EC2!\n",
  owner   => 'www-data',
  group   => 'www-data',
  mode    => '0644',
  require => Package['apache2'],
}
```

---

## 📄 Step 5: (Optional) Custom Web Page

### `files/index.html`

```html
<h1>Apache installed using Puppet</h1>
<p>Server managed by Puppet on Ubuntu EC2</p>
```

To use this file instead of inline content, replace the `file` resource with:

```puppet
file { '/var/www/html/index.html':
  ensure => file,
  source => 'file:///home/ubuntu/puppet-apache/files/index.html',
}
```

---

## ▶️ Step 6: Apply Puppet Manifest

```bash
sudo puppet apply manifests/site.pp
```

Expected output:

```
Notice: Applied catalog in X.XX seconds
```

---

## 🌐 Step 7: Verify in Browser

Open in browser:

```
http://<EC2-Public-IP>
```

✅ You should see your **Puppet-managed Apache page**

---

## 🧪 Validation Commands (Interview Useful)

```bash
systemctl status apache2
puppet resource service apache2
puppet resource package apache2
```

---

## 📌 What You Learned (Interview Talking Points)

✔ Puppet **declarative language**
✔ Idempotency (run manifest multiple times safely)
✔ Package, Service, File resources
✔ Puppet **standalone mode on EC2**
✔ Infrastructure as Code basics

---

## 🚀 Next Level Enhancements (Optional)

If you want, I can help you add:

* Puppet **modules**
* Nginx instead of Apache
* User & group management
* Cron jobs via Puppet
* Puppet Master + Agent setup
* GitHub-ready Puppet project
* Terraform + EC2 + Puppet combo project

👉 Just tell me how advanced you want it.
