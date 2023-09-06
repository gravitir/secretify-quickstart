<img src="https://app.secretify.io/assets/logo.svg" width="100">

A secure way to transfer or share secrets.

# Get Started

10 min.

In this unit you will learn how to install Secretify using Docker.

At the end of this section, you will have a working Secretify instance behind a reverse proxy. The reverse proxy used in this guide is [traefik](https://traefik.io/), but any other preferable option woudl work fine.

## Prerequisite

Before you begin, make sure you have the following tools installed:

* [Docker](https://docs.docker.com/engine/install/) - Containerization platform for running Secretify.
* [Docker Compose](https://docs.docker.com/compose/install/) - Used to manage multi-container Docker applications (only necessary for older Docker versions; Compose V2 is included in newer versions).
* Git - You'll need Git to clone the Secretify Quickstart GitHub repository to your working directory.

```bash
git clone https://github.com/gravitir/secretify-quickstart.git
```

## Installation

This step-by-step installation guide is intended for testing and development purposes and should not be used in a production environment.

If you're using Docker in a local environment, you can proceed with the intended guide. However, if you plan to expose your Secretify instance publicly, follow the secure variant as Secretify requires HTTPS for proper functionality in a public setting.

### Step 1: Login to the Private Docker Registry

Since this is a private docker registry, you need credentials. Log in to our docker registry, and you will get a prompt to enter your password.

```bash
docker login registry.gravitir.io -u YOUR_USERNAME
```

**Verification**

After you enter the password, you should see the following output:  
*Login Succeeded*

### Step 2: Pull the Docker image

Open a terminal session and browse to `secretify-quickstart`. Pull the Docker images defined in `docker-compose.yml`:

```bash
docker-compose pull
```

**Verification**

When the required images are successfully pulled, the terminal will return the following:  
*Pulling traefik       ... done*  
*Pulling secretify-api ... done*  
*Pulling secretify-ui  ... done*

### Step 3: Configure Host IP and DNS Record

Depending on your setup, you need to do this differently.

Open the `.env` file and set the `HOST_IP` to your host IP address and `FQDN` (Fully Qualified Domain Name): 

```bash
HOST_IP="172.16.3.58"
FQDN="secretify.localhost"
```

`HOST_IP`: Set this to your host's IP address, which you can find by running a command like `ipconfig` (Windows) or `ifconfig` (Linux/macOS) in your terminal. Make sure it matches your active network adapter's IPv4 address.

Next, add a DNS record for your `FQDN` (e.g. `secretify.localhost`) in your DNS configuration. This step might vary depending on your network setup and DNS server.

> **_NOTE:_**   Ensure that you do not use a loopback address (e.g., 127.0.0.1) for the `HOST_IP` in the `.env` file. This is because the API instance needs to reach the UI, and using a loopback address would prevent proper communication.

### Step 3b: Additonal Configuration for Secure Variant

Set the `SCHEME` to `https://` in the `.env` file. Additionally, make sure to define a valid email address in `POSTMASTER_EMAIL` which used by traefik for issuing Let's Encrypt certificates.

### Step 5: Start the Secretify Instance

To start the Secretify instance, use one of the following commands depending on your chosen variant:

For the local variant:

```
docker-compose up -d
```

For the secure variant:

```
docker-compose up -f docker-compose.secure.yaml -d
```

When Secretify starts, the terminal will return the following:  
*Creating network "secretify-quickstart_default" with the default driver*  
*Creating volume "secretify-quickstart_db" with default driver*  
*Creating volume "secretify-quickstart_files" with default driver*  
*Creating traefik       ... done*  
*Creating secretify-api ... done*  
*Creating secretify-ui  ... done*  

**Verification**

Run the following command to see a list of running containers:

```
docker ps -a
```

### Step 6: Open Secretify in Your Browser

Open a browser and navigate to http://secretify.localhost

## Tweak configuration

Now you can customize various configuration parameters by editing the `.env` file. For example, you can change the primary branding color by editing `BRANDING_PRIMARY_COLOR="#EA2027"`. After making changes, restart Secretify using again:

For the local variant:

```
docker-compose up -d
```

For the secure variant:

```
docker-compose up -f docker-compose.secure.yaml -d
```

You should see the changes reflected in the Secretify interface.

### Branding configuration

*coming soon..*

### Authentication Microsoftonline configuration

*coming soon..*

### Mail STMP configuration

*coming soon..*

### Storage configuration

*coming soon..*

### Outlook Plugin configuration

*coming soon..*

## Acknowledgments

Our thanks are extended but not limited to the following people or organizations:

* [Freepik](https://www.flaticon.com/free-icons/star-wars) - Star wars icons created by Freepik - Flaticon
