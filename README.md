# Tomcat HTTPS Configuration

This project provides scripts and configuration files to set up Apache Tomcat with HTTPS support using SSL/TLS certificates.

## Files in this Project

- **create-server-cert.sh**: Script to create server certificate and private key
- **create-tomcat-keystore.sh**: Script to convert certificates to Tomcat-compatible keystores
- **tomcat-server-xml-example.xml**: Example Tomcat server.xml configuration with HTTPS enabled

## Prerequisites

- OpenSSL
- Java Runtime Environment (JRE) or Java Development Kit (JDK)
- Apache Tomcat (10.1.x recommended)

## Installation & Setup

### 1. Generate Server Certificate

```bash
./create-server-cert.sh
```

This script:
- Creates a private key
- Generates a Certificate Signing Request (CSR)
- Creates a self-signed certificate
- Stores the files in the `certs/` directory

### 2. Create Tomcat Keystore

```bash
./create-tomcat-keystore.sh
```

This script:
- Converts the certificate and key to PKCS12 format
- Creates a Java KeyStore (JKS) from the PKCS12 file
- Stores both files in the `tomcat/` directory
- Sets the default keystore password to "changeit"

### 3. Install Tomcat

Download and install Apache Tomcat. Example installation:

```bash
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.40/bin/apache-tomcat-10.1.40.tar.gz
tar -xzf apache-tomcat-10.1.40.tar.gz
sudo mv apache-tomcat-10.1.40 /opt/tomcat
sudo chmod +x /opt/tomcat/bin/*.sh
```

### 4. Configure Tomcat for HTTPS

```bash
# Copy the keystore to Tomcat's conf directory
sudo cp tomcat/localhost.p12 /opt/tomcat/conf/

# Replace Tomcat's server.xml with our HTTPS-enabled configuration
sudo cp tomcat-server-xml-example.xml /opt/tomcat/conf/server.xml
```

### 5. Start Tomcat

```bash
sudo /opt/tomcat/bin/startup.sh
```

## Verification

Test your HTTPS configuration:

```bash
curl -k https://localhost:8443
```

Or open https://localhost:8443 in a web browser.

## Security Considerations

- Change the default keystore password ("changeit") in both the keystore and server.xml
- For production environments, obtain a certificate from a trusted Certificate Authority
- Regularly update your certificates (typically every 1-2 years)
- Configure proper cipher suites and TLS protocols for enhanced security

## Maintenance

- **Stop Tomcat**: `/opt/tomcat/bin/shutdown.sh`
- **View logs**: Check files in `/opt/tomcat/logs/`
- **Certificate renewal**: Run the certificate scripts again and copy the new keystores

## Additional Resources

- [Apache Tomcat SSL/TLS Configuration](https://tomcat.apache.org/tomcat-10.1-doc/ssl-howto.html)
- [OpenSSL Certificate Management](https://www.openssl.org/docs/man1.1.1/man1/openssl.html)
