# MySQL & MySQL Workbench – Installation Guide

This guide explains how to install **MySQL Server** and **MySQL Workbench** on **Windows, macOS, and Linux**.

---

## ✅ **1. Install MySQL on Windows**

### **Download MySQL Installer**

1. Go to the official MySQL website:
   [https://dev.mysql.com/downloads/installer/](https://dev.mysql.com/downloads/installer/)

2. Download **MySQL Installer (Community Edition)**:

   * Choose: **MySQL Installer (Full or Web)**

### **Install Components**

During installation, select:

* **MySQL Server**
* **MySQL Workbench**
* MySQL Shell *(optional)*
* MySQL Router *(optional)*

### **Setup Steps**

1. Choose **Standalone MySQL Server**
2. Configure:

   * Port: `3306`
   * Authentication: Strong or Legacy password
3. Set **root password**
4. Finish installation

### **Verify Installation**

Open PowerShell or CMD:

```
mysql --version
```

Open MySQL Workbench and connect to:

* Host: `localhost`
* Port: `3306`
* User: `root`

---

## ✅ **2. Install MySQL on macOS (Intel & M1/M2/M3)**

### **Option A: Download DMG Installer**

1. Visit:
   [https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)

2. Choose:

   * macOS 13+, 12, etc.
   * Download the **DMG Archive**

### **Install Package**

1. Open `.dmg` file
2. Run the installer
3. Set up:

   * Root password
   * Startup preferences (auto-start MySQL server)

### **Verify Installation**

Open Terminal:

```
mysql --version
```

Start MySQL service:

```
sudo /usr/local/mysql/support-files/mysql.server start
```

### **Install MySQL Workbench**

Download from:
[https://dev.mysql.com/downloads/workbench/](https://dev.mysql.com/downloads/workbench/)

---

### **Option B: Install via Homebrew**

```
brew update
brew install mysql
brew services start mysql
```

Check version:

```
mysql --version
```

---

## ✅ **3. Install MySQL on Linux (Ubuntu / Debian)**

### **Install through APT**

Open Terminal:

```
sudo apt update
sudo apt install mysql-server
```

Check status:

```
sudo systemctl status mysql
```

Start MySQL:

```
sudo systemctl start mysql
```

Secure MySQL installation:

```
sudo mysql_secure_installation
```

Login:

```
sudo mysql
```

### **Install MySQL Workbench**

```
sudo apt install mysql-workbench
```

---

## 🧪 **Verify Whether MySQL is Installed**

Run:

```
mysql --version
```

Expected output:

```
mysql  Ver 8.0.xx
```

---

## 🖥 **Start MySQL Workbench**

Search for **MySQL Workbench** in your OS and open it.

Connect using:

* Host: `localhost`
* Port: `3306`
* User: `root`
* Password: (the one you set)

---

## ⚠️ Troubleshooting

### MySQL Command Not Found?

Add MySQL to PATH manually or reinstall.

### Can't Connect on Port 3306?

Another program is using the port → change the MySQL port during installation.

---

## ✔️ Installation Complete

You now have:

* **MySQL Server** installed
* **MySQL Workbench** configured
* Ready to create schemas, tables, queries, and manage your database

