---
title: How to Deploy Dokku on a VPS, A Beginner's Guide
description: >-
  A step-by-step solution for deploying Dokku on your own VPS, with Dokku managing and deploying multiple sites as easily as breathing.
author: zsoolti8917
date: 2024-08-23 12:42:00 +0200
categories: [VPS, Dokku, Project hosting]
tags: [Dokku]
pin: false
media_subpath: 'assets/posts/dokku'
---


Welcome to your next adventure in the world of web development! If you're reading this, chances are you've got an idea for an app, a side project, or maybe you just want to level up your dev skills. Whatever brought you here, this guide is going to walk you through how to deploy Dokku on your very own VPS---no prior experience required.

## What's Dokku, and Why Should You Care?

So, what's this Dokku thing anyway? Imagine having your own private Heroku---Dokku turns your server into a mini-PaaS (Platform as a Service), making it super easy to deploy and manage your apps. If you've ever found yourself tangled in the complexities of server management, Dokku is like a breath of fresh air.

### Here's why Dokku rocks:

-   **Simplicity at its Best:** You don't need to be a server wizard. Dokku handles a lot of the heavy lifting for you, letting you focus on what you love: building cool stuff.
-   **Budget-Friendly:** Forget about those hefty Heroku bills. Running Dokku on a VPS is a wallet-friendly alternative that gives you full control.
-   **Flexibility:** Whether you're working with Node.js, Python, Ruby, or something else, Dokku's got your back. You push your code, and Dokku does the rest.
-   **Scalability:** As your app grows, so can your Dokku setup. Need to handle more users? No problem---Dokku scales with you.

### What's in Store for You

In this guide, I'm going to take you through every single step of getting Dokku up and running on a VPS. No stone will be left unturned. Here's the game plan:

-   **VPS Setup:** We'll start from square one, choosing a VPS provider (I went with Forpsi), setting up SSH, and making sure your server is locked down and secure.
-   **Dokku Installation:** Next, we'll dive into the nitty-gritty of getting Dokku installed on your server. Don't worry, I'll break it all down so it's easy to follow.
-   **Deploying Your First App:** The moment of truth---deploying your first app using Dokku. It's going to feel like magic, but it's all real, and you'll be the one making it happen.
-   **After the Setup:** Finally, we'll talk about what comes next---setting up SSL for security, managing your domain, and making sure your setup is ready for the real world.

By the time you're done, you'll not only have a working Dokku setup but also the confidence to deploy and manage your own apps. Let's get started and turn your VPS into a deployment powerhouse!

Section 1: Setting Up Your VPS
------------------------------

When it came to picking a VPS provider for hosting my personal and commercial websites, I decided on **Forpsi**. You might be wondering why I chose Forpsi over other big names like DigitalOcean or AWS. Here's the deal: Forpsi is a semi-local provider for me, based in Slovakia, which means I get better latency and more tailored support. Plus, it's easy on the wallet without sacrificing performance.

### Here's what my VPS setup looks like:

-   **VPS Type:** CLOUD VPS OPENSTACK
-   **Processor:** 4 vCPUs (AMD)
-   **RAM:** 8 GB
-   **Storage:** 80 GB NVMe SSD (Software-Defined Storage)
-   **Bandwidth:** 50 TB per month
-   **Network:** 1 Gb/s connectivity
-   **IP:** 1 public IPv4 and IPv6
-   **Operating System:** Linux/Windows options
-   **Price:** €13/month

I chose this more powerful VPS because I'm hosting multiple projects---both personal and commercial. With 8 GB of RAM and 4 vCPUs, I can run several websites, apps, and services without breaking a sweat. The 80 GB SSD ensures that everything runs fast and smoothly, and the 50 TB/month bandwidth gives me plenty of room to handle traffic spikes.

But here's the thing: **If you're only planning to host a few websites, you don't need to go all out like I did.** A more modest VPS can easily handle your needs, and you can always scale up later if your projects start to grow.

### Let's talk about Dokku's requirements:

Dokku is pretty lightweight, so you don't need a monster server to run it. Here are the **minimum specs** you'll need:

-   **vCPU:** 1 core
-   **RAM:** 1 GB (2 GB is recommended if you're running multiple apps)
-   **Storage:** 20 GB SSD
-   **Operating System:** Ubuntu 20.04 or later

Even with these minimal specs, Dokku can handle a couple of small websites or apps comfortably. If you're just getting started or only need to deploy a few projects, a VPS with 1-2 GB of RAM and a single CPU core is more than enough. You can find VPS plans like this for around €2-5/month on Forpsi or similar providers.

However, if you're like me and planning to host several projects or expect your sites to grow in traffic, going for a bit more power makes sense. This way, you're not constantly worried about running out of resources or having to upgrade your server every time you add a new site.

### Initial VPS Configuration

Once you've settled on your VPS setup, the first step is to get it configured and ready for action. Here's how you get started:

1.  **Receive Your Credentials:** After signing up with Forpsi, you'll get an email with your VPS's IP address, along with the root username and password.
2.  **Connect via SSH:**
    -   On Windows, use PuTTY (we'll dive into setting that up in the next section).
    -   On Linux or macOS, just open the terminal.
    -   Enter the IP address and connect using SSH. The default port is 22.

4.  **First-Time Login:**

    -   Log in as root using the credentials provided.
    -   Immediately change the default root password to something secure.

6.  **Why Choose a Non-Root User:** Using the root account for everyday tasks isn't recommended for security reasons. Later on, we'll create a new user with sudo privileges, so you can manage your server without risking the root account.

That's it for the initial setup! Now you're logged into your VPS and ready to move forward with securing it and setting up Dokku.


Section 2: Preparing Your Local Environment
-------------------------------------------

Before you can start deploying your apps on your shiny new VPS, there are a couple of things you need to set up on your local machine. This section will guide you through generating an SSH key and configuring PuTTY on Windows 10. Don't worry---these steps are easier than they sound, and I'll walk you through each one.

### Generating an SSH Key on Windows 10

**What's an SSH Key and Why Do You Need One?**

An SSH key is like a special password that only you have, which allows you to securely connect to your VPS without needing to type in your password every time. It's made up of two parts: a public key (which you'll add to your VPS) and a private key (which stays on your computer). Together, they ensure that only your computer can access the server, making it way more secure than traditional password-based logins.

#### How to Generate an SSH Key Using ssh-keygen on Windows 10

Here's how to create an SSH key pair using the ssh-keygen tool, which comes with Git for Windows (or you can use Windows Subsystem for Linux, if you have it installed):

1.  **Install Git for Windows (if you haven't already):**

    -   Download and install it from [git-scm.com](https://git-scm.com/).
    -   During installation, make sure to select "Use Git from the command prompt" so that ssh-keygen will be available.

3.  **Open Git Bash:**

    -   After installation, right-click anywhere on your desktop or in a folder, and select "Git Bash Here."

5.  **Generate the SSH Key:**

    -   In the Git Bash window, type the following command and hit Enter:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
> Replace "your_email@example.com" with your actual email address. {: .prompt-tip }

-   When prompted to "Enter a file in which to save the key," just press Enter to accept the default location (~/.ssh/id_rsa).
-   You'll be asked to enter a passphrase---this adds an extra layer of security. You can either choose one or press Enter to skip it.

2.  **Your SSH Key is Ready:**

    -   After you hit Enter, the tool will generate your SSH key pair. The private key (id_rsa) stays on your computer, and the public key (id_rsa.pub) is what you'll copy over to your VPS.

#### Copying the Public Key for Later Use

Now that you've got your SSH key, let's prepare it for use on your VPS:

1.  **View the Public Key:**

Still in Git Bash, type:

```bash
cat ~/.ssh/id_rsa.pub
```
This will display your public key in the terminal.

2.  **Copy the Public Key:**

Highlight the entire key (starting with ssh-rsa) and right-click to copy it.

Keep this handy, as you'll need it when we log into your VPS and set up SSH access for your non-root user. {: .prompt-tip }

### 2\. Installing and Configuring PuTTY

**What is PuTTY and Why Do You Need It?**

PuTTY is a free SSH client for Windows that lets you connect to your VPS securely. Think of it as the bridge between your computer and your server. While tools like Git Bash can also handle SSH connections, PuTTY offers a more user-friendly interface and additional features, like saving session configurations, which is super handy when you're regularly accessing your VPS.

#### How to Download, Install, and Configure PuTTY

Let's get PuTTY up and running on your system:

1.  **Download PuTTY:**

    -   Head over to the [PuTTY download page](https://www.putty.org/) and grab the Windows installer (you'll likely want the 64-bit version).

3.  **Install PuTTY:**

    -   Run the installer you just downloaded and follow the prompts. It's a straightforward installation process---just keep clicking "Next" and then "Finish" when you're done.

5.  **Launch PuTTY:**

    -   Once installed, open PuTTY from your Start menu.

7.  **Configure Your Session:**

    -   In the "Host Name (or IP address)" field, enter the IP address of your VPS.
    -   Make sure the "Port" is set to 22 (this is the default SSH port).
    -   Save this session configuration by typing a name under "Saved Sessions" and clicking "Save."

### How to Load Your SSH Key into PuTTY

To use your SSH key with PuTTY, we need to load it into PuTTY's key manager:

1.  **Convert the SSH Key to PuTTY's Format (PPK):**

    -   PuTTY doesn't use the standard SSH key format, so we need to convert your key.
    -   Open PuTTYgen (it's installed along with PuTTY) and click "Load."
    -   Navigate to C:\Users\YourUsername\.ssh\, select id_rsa, and click "Open."
    -   Click "Save private key" (you can ignore the passphrase warning if you didn't set one).

3.  **Add the Key to PuTTY's Agent:**

    -   Open Pageant (PuTTY's SSH authentication agent---it's like a key manager).
    -   Right-click on the Pageant icon in your system tray (bottom-right corner of your screen) and select "Add Key."
    -   Browse to the .ppk file you just saved and add it.

5.  **Connect to Your VPS:**

    -   Go back to PuTTY, load your saved session, and click "Open."

If everything's set up correctly, PuTTY will use your SSH key to log into your VPS without asking for a password.{: .prompt-tip }

And that's it! Your local environment is now set up with SSH keys and PuTTY, so you're ready to connect to your VPS and start installing Dokku.


Section 3: Connecting to Your VPS
---------------------------------

### Ready to Connect to Your VPS? Let's Dive In!

Now that you've set up PuTTY and configured your SSH key, it's time to connect to your VPS and start getting things ready. No need to worry—this process is straightforward, and I'll guide you through every step to make your first connection and secure your server.

#### Step 1: Launch PuTTY

First, open PuTTY from your Start menu. You'll see the **PuTTY Configuration** window where you need to enter a few details:

- **Host Name (or IP address):** Type in the IP address of your VPS (the one you received from Forpsi).
- **Port:** Keep this set to `22`, the default for SSH connections.
- **Connection Type:** Ensure that **SSH** is selected—this tells PuTTY to establish a secure connection.

#### Step 2: Save Your Session (Optional but Handy!)

If you'd like to save these settings for future use, type a name like "MyVPS" under **Saved Sessions** and click **Save**. This way, you can easily connect later without re-entering all the details.

#### Step 3: Open the Connection

Next, click on the **Open** button at the bottom of the window. A terminal window will pop up, asking for your username.

- Enter `root` (or the username you plan to use) and hit `Enter`.
- If you've set up your SSH key correctly, PuTTY should log you in automatically without asking for a password.
- If prompted for a password, make sure **Pageant** (PuTTY's key manager) is running and has your SSH key loaded.

#### Step 4: Enhance Your Server's Security

Once logged in, you're almost ready to start setting things up, but let's take a moment to enhance your server's security. By default, the root password provided by Forpsi might be a bit too simple. It's a good practice to change this password to something more secure.

1. At the command prompt, type `passwd` and press `Enter`.
2. You'll be prompted to enter a new password.
    - **Tip:** Choose something strong—mix uppercase and lowercase letters, numbers, and special characters.
3. Confirm the new password by entering it again.

Changing the root password is a simple but crucial step in securing your VPS. Even though you're using SSH keys for secure access, having a strong root password adds an extra layer of protection.

#### Step 5: You're Ready to Go!

With your new password set, you're now ready to proceed with setting up your server. The next step? Creating a non-root user to manage your server safely.

And there you have it—your VPS is connected and secured. You're all set to dive into the next steps of your Dokku installation!



Section 4: Creating a New User on the VPS
-----------------------------------------

Alright, now that your VPS is up and running securely, it's time to create a new user. While it might seem tempting to use the root account for everything, it's actually safer and more practical to set up a non-root user for your day-to-day tasks.

### Why Avoid Using the Root User?

Using the root account for all your operations isn't just risky---it's a bad practice. The root user has unrestricted access to all parts of the system, which means if something goes wrong, it could be disastrous. By creating a non-root user with limited privileges, you reduce the risk of accidental system changes or security breaches. This approach helps you keep your server safer and more manageable.

### Creating a New User

To create a new user, log into your VPS as root. Once you're logged in, you'll use a couple of simple commands to set things up. First, type the following command to create a new user:

```bash
adduser newusername
```
Replace newusername with whatever name you want for the new user. The system will ask you to set a password for this user and provide some additional information like the user's full name. You can either fill these out or just hit Enter to skip them.

After creating the user, you need to give them administrative privileges. This is done by adding the user to the sudo group. Run this command:

```bash
usermod -aG sudo newusername
```

This command adds the new user to the sudo group, which allows them to execute administrative commands with sudo.

### Setting Up SSH Access for the New User

Now that you have a new user, you'll want to set up SSH access for them just like you did with the root user. This way, you can log in directly as this new user without having to switch from root.

First, switch to the new user account:

```bash
su - newusername
```
Once you're logged in as the new user, create the .ssh directory and set the right permissions:

```bash
mkdir ~/.ssh
chmod 700 ~/.ssh
```
Now you need to copy your SSH key to this user's authorized_keys file. Start by switching back to the root user or using a different terminal where you still have access. Use the following command to append your public SSH key to the new user's authorized_keys file:

```bash
cat ~/.ssh/id_rsa.pub | ssh newusername@your-vps-ip 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
```
This command copies your public key into the authorized_keys file of the new user. Make sure to replace newusername and your-vps-ip with the appropriate values.

After doing this, switch back to the new user's account and check that the permissions for the authorized_keys file are correct:

```bash
chmod 600 ~/.ssh/authorized_keys
```
With this setup, your new user now has SSH access, and you've got a secure, non-root account ready for managing your VPS. This approach not only enhances security but also makes your server administration more organized and safer.

Now, you're all set to proceed with installing and configuring Dokku!


Section 5: Installing Dokku on the VPS
--------------------------------------

Let's get Dokku installed on your VPS! Dokku simplifies the deployment of applications by using Git commands, and even though we're working with a headless Ubuntu server, the process is quite straightforward. Here's how you can install and configure Dokku.

### Introduction to Dokku

Dokku is a lightweight PaaS (Platform as a Service) that lets you deploy and manage applications on your server. It's like having your own Heroku, giving you control over your app deployments while handling scaling, environment management, and more. It's a great tool for developers who want the convenience of a PaaS with the flexibility of managing their own server.

### System Update and Package Installation

Before installing Dokku, you'll need to make sure your system is up-to-date. Connect to your VPS as the non-root user you created earlier, and run these commands:

```bash
sudo apt update

sudo apt upgrade -y
```
These commands update your package lists and upgrade all installed packages to their latest versions. Next, you'll need to install some essential packages:

```bash
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```
These packages ensure that your system can fetch packages securely and manage software sources properly.

### Installing Dokku

Now, let's install Dokku. Use the following command to download and execute the Dokku installation script:

```bash
wget -qO- https://dokku.com/install.sh | sudo DOKKU_TAG=v0.30.3 bash
```
This command runs the installation script provided by Dokku. The DOKKU_TAG specifies the version you're installing; check the [Dokku website](https://dokku.com/) for the latest version if you want the newest features. The script will handle the installation and initial configuration.

### Setting Up Dokku

Dokku doesn't have a graphical setup interface, so you'll configure it via the command line. Here's what you need to do:

**Configure Domain and SSH Keys:**

-   **Set Up Your Domain:**

Make sure your domain points to your VPS's IP address by updating your DNS settings. Once your domain is correctly set up, you can configure Dokku to use it.

-   **Add Your SSH Key:**

To enable Git deployments, you need to add your public SSH key to Dokku. Here's how you can find and copy your SSH key from a Windows 10 machine:

**Find Your Public SSH Key:**

Open File Explorer and navigate to your .ssh folder. This is usually located at:

```makefile
C:\Users\<YourUsername>\.ssh
```
Inside the .ssh folder, look for a file named id_rsa.pub. This is your public key.

**Open Your Public Key:**

Right-click on id_rsa.pub and select **Open with**. Choose **Notepad** or another text editor to view the file.

**Copy the Public Key:**

In Notepad, you'll see a long string of characters---this is your public SSH key. Select the entire string, right-click and choose **Copy**.

**Add the Public Key to Your VPS:**

Connect to your VPS via SSH and add your copied public key. First, ensure you're logged in as your non-root user. Then create the .ssh directory and add your public key:

```bash
mkdir -p ~/.ssh

echo 'your-public-ssh-key' >> ~/.ssh/authorized_keys

chmod 600 ~/.ssh/authorized_keys
```
Replace 'your-public-ssh-key' with the key you copied earlier. This allows Dokku to authenticate your Git pushes.

**Verify Installation:**

After setting up SSH access, verify your Dokku installation with:

```bash
dokku report
```
This command provides a summary of your Dokku installation and can help troubleshoot any issues.

With Dokku installed and configured, you're ready to start deploying applications. Dokku will handle the deployment process, letting you focus on your apps without worrying about the underlying infrastructure.


Section 6: Deploying Your First Application with Dokku
------------------------------------------------------

Now that you've got Dokku set up, it's time to deploy your first application. Don't worry; we'll guide you through the process step-by-step. Let's get your app up and running smoothly!

### Preparing a Simple Application

Let's use a basic Node.js app as an example. If you prefer Python or another language, the steps will be similar, but tailored to your chosen framework.

**Create a Simple Node.js App on Your Local PC:**

First, ensure Node.js and npm are installed on your local machine. If they're not, download them from [nodejs.org](https://nodejs.org/).

Open your command line and create a new project directory:

```bash
mkdir my-app

cd my-app
```
Initialize a new Node.js project:

```bash
npm init -y
```
Install Express, a popular framework for Node.js:

```bash
npm install express
```
Create an index.js file with the following code:

```javascript
const express = require('express');

const app = express();

const port = process.env.PORT || 3000;

app.get('/', (req, res) => {

  res.send('Hello World!');

});

app.listen(port, () => {

  console.log(`App listening at http://localhost:${port}`);

});
```
Add a Procfile to tell Dokku how to run your app. The Procfile should contain:

```makefile
web: node index.js
```
Your app is now ready for deployment!

### Setting Up Dokku on Your VPS

With your app prepared on your local machine, let's get it running on your VPS.

**Create a Dokku App:**

Connect to your VPS via SSH and create a new app on Dokku. Replace your-app-name with a name for your application:

```bash
dokku apps:create your-app-name
```
**Add Your SSH Key and Set Up HTTPS with Let's Encrypt:**

Make sure your public SSH key is set up for Dokku by following the previous instructions. This step ensures secure deployments.

To secure your application with HTTPS, you can use Let's Encrypt. First, ensure your domain is correctly pointing to your VPS, then run:

```bash
dokku domains:add your-app-name your-domain.com

dokku letsencrypt:enable your-app-name
```
This will automatically obtain and configure an SSL certificate for your domain, enabling HTTPS for your application.

### Pushing Your Application to Dokku

Now, let's push your app from your local machine to Dokku:

**Initialize Git Repository (if not done already):**

In your project directory, initialize a Git repository:

```bash
git init
```
Add and commit your files:

```bash
git add .

git commit -m "Initial commit"
```
**Add Dokku Remote:**

Add a Git remote for Dokku, pointing to your VPS. Replace your-app-name with your app's name and your-vps-ip with your VPS's IP address:

```bash
git remote add dokku dokku@your-vps-ip:your-app-name
```
**Deploy Your App:**

Push your code to Dokku:

```bash
git push dokku master
```
Dokku will build and deploy your app, displaying logs of the process so you can monitor its progress.

### Accessing Your Deployed Application

Once the deployment is complete, you can access your app. Open your web browser and navigate to your domain:

```bash
http://your-domain.com
```
If you didn't set up a domain, you can use your VPS's IP address:

```bash
http://your-vps-ip
```
Congratulations! Your application is now live and accessible. You've successfully deployed your first app using Dokku. Feel free to explore more of Dokku's features and capabilities to manage and scale your applications. If you run into any issues or have questions, don't hesitate to leave a comment or ask for help. Happy deploying!



Conclusion
----------

Congratulations! You've just walked through the process of deploying Dokku on your VPS, from setting up your server to installing and configuring Dokku. Here's a quick recap of the journey:

1.  **Choosing and Setting Up Your VPS:** You picked a VPS from Forpsi, set it up with a suitable plan, and connected to it using PuTTY. You created a non-root user to enhance security and ensure safer management of your server.
2.  **Preparing Your Local Environment:** On your Windows 10 machine, you generated an SSH key and configured PuTTY to connect securely to your VPS. This setup was crucial for establishing a secure connection between your local environment and your server.
3.  **Connecting to Your VPS:** Using PuTTY, you connected to your VPS, logged in for the first time, and changed the default password to something more secure. This step was essential for maintaining the security of your server.
4.  **Creating a New User on the VPS:** You created a non-root user to avoid the risks associated with using the root account for daily tasks. You also set up SSH access for this user to streamline future connections.
5.  **Installing and Configuring Dokku:** Finally, you installed Dokku by running the installation script and configured it through the command line. You set up SSH access for Dokku, allowing you to deploy applications with ease.

With Dokku now installed and configured, you have a powerful tool at your disposal for deploying and managing applications on your VPS. Dokku offers a range of advanced features for scaling, environment management, and more. Feel free to explore these features to get the most out of your setup.

If you have any questions or run into issues, don't hesitate to leave a comment below. I'm here to help and would love to hear about your experiences or any challenges you faced during the setup. Happy deploying!