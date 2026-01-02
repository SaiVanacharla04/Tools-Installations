
---

# Vagrant Environment Setup – Manual Provisioning

## Overview

This repository provides a **pre-configured multi-VM Vagrant environment** for development and testing. It includes:

| VM Name | Role                        | OS           | Private IP    |
| ------- | --------------------------- | ------------ | ------------- |
| `web01` | Web Server (Nginx)          | Ubuntu 25.04 | 192.168.56.11 |
| `app01` | Application Server (Tomcat) | CentOS 7     | 192.168.56.12 |
| `rmq01` | RabbitMQ                    | CentOS 7     | 192.168.56.16 |
| `mc01`  | Memcached                   | CentOS 7     | 192.168.56.14 |
| `db01`  | Database Server             | CentOS 7     | 192.168.56.15 |

All VMs are provisioned using **VirtualBox** and configured with **1 CPU and 1GB RAM** by default. Shared folders and private networks are pre-configured for seamless local development.

---
Got it! On **GitHub Markdown**, ASCII diagrams like this **must be inside triple backticks** to preserve spacing and formatting. Here’s the **correct way**:

```
# VM Topology & Network Diagram


<img width="648" height="486" alt="image" src="https://github.com/user-attachments/assets/1c2aa581-c16d-4fa8-b0ff-41f46a4fdd74" />


•	web01 serves as the frontend Nginx server.
•	app01 hosts the application (Tomcat).
•	rmq01 handles messaging with RabbitMQ.
•	mc01 provides caching via Memcached.
•	db01 runs the backend database.
All VMs are connected through a private network (192.168.56.0/24) for secure inter-VM communication.

---

## Prerequisites

1. **Vagrant** – Install the latest version from HashiCorp:
   [https://developer.hashicorp.com/vagrant/install](https://developer.hashicorp.com/vagrant/install)

2. **VirtualBox** – Recommended version: 7.x. Download from:
   [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

3. **Vagrant Hostmanager Plugin** – Required for automatic `/etc/hosts` updates:

```bash
vagrant plugin install vagrant-hostmanager
```

4. **Vagrant Boxes** – Pre-downloaded boxes included in `D:/Vagrant`:

   * Ubuntu 25.04: [alvistack/ubuntu-25.04](https://portal.cloud.hashicorp.com/vagrant/discover/alvistack/ubuntu-25.04)
   * CentOS 7: [alvistack/centos-7](https://portal.cloud.hashicorp.com/vagrant/discover/centos/stream9)

---

## Project Structure

```
D:/Vagrant/local-setup/vagrant/Manual_provisioning/
├── Vagrantfile              # Defines all 5 VMs
├── alvistack_ubuntu-25.04.box
├── alvistack_centos-7.box
└── README.md                # This documentation
```

---

## Vagrantfile Configuration

The `Vagrantfile` uses **Vagrant 2.x syntax** and includes the following:

```ruby
Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  # Web Server (Ubuntu 25.04)
  config.vm.define "web01" do |web01|
    web01.vm.box = "alvistack_ubuntu-25.04"
    web01.vm.hostname = "web01"
    web01.vm.network "private_network", ip: "192.168.56.11"
    web01.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  end

  # Application Server (Tomcat, CentOS 7)
  config.vm.define "app01" do |app01|
    app01.vm.box = "alvistack_centos-7"
    app01.vm.hostname = "app01"
    app01.vm.network "private_network", ip: "192.168.56.12"
    app01.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  end

  # RabbitMQ (CentOS 7)
  config.vm.define "rmq01" do |rmq01|
    rmq01.vm.box = "alvistack_centos-7"
    rmq01.vm.hostname = "rmq01"
    rmq01.vm.network "private_network", ip: "192.168.56.16"
    rmq01.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  end

  # Memcached (CentOS 7)
  config.vm.define "mc01" do |mc01|
    mc01.vm.box = "alvistack_centos-7"
    mc01.vm.hostname = "mc01"
    mc01.vm.network "private_network", ip: "192.168.56.14"
    mc01.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  end

  # Database Server (CentOS 7)
  config.vm.define "db01" do |db01|
    db01.vm.box = "alvistack_centos-7"
    db01.vm.hostname = "db01"
    db01.vm.network "private_network", ip: "192.168.56.15"
    db01.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
  end
end
```

> All VMs are installed **on the D: drive** by default, as Vagrant is executed from `D:/Vagrant`.

---

## Quick Start

1. Navigate to the project directory:

```bash
cd D:/Vagrant/local-setup/vagrant/Manual_provisioning
```

2. Bring up all VMs:

```bash
vagrant up
```

3. Check the status of all VMs:

```bash
vagrant status
```

4. SSH into any VM:

```bash
vagrant ssh web01
vagrant ssh app01
```

5. Halt all VMs when finished:

```bash
vagrant halt
```

6. Destroy all VMs if needed:

```bash
vagrant destroy -f
```

---

## Useful References

* **Vagrant Boxes:** [https://portal.cloud.hashicorp.com/vagrant/discover?providers=virtualbox&query=ubuntu](https://portal.cloud.hashicorp.com/vagrant/discover?providers=virtualbox&query=ubuntu&sort=updated_at%20desc)
* **Vagrant Installation:** [https://developer.hashicorp.com/vagrant/install](https://developer.hashicorp.com/vagrant/install)
* **CentOS Box:** [https://portal.cloud.hashicorp.com/vagrant/discover/centos/stream9](https://portal.cloud.hashicorp.com/vagrant/discover/centos/stream9)
* **Ubuntu Box:** [https://portal.cloud.hashicorp.com/vagrant/discover/alvistack/ubuntu-25.04](https://portal.cloud.hashicorp.com/vagrant/discover/alvistack/ubuntu-25.04)
