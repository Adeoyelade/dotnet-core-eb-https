# Deploying ASP.Net Core application to Elastic Beanstalk’s single instance environment with HTTPS

Asp.Net Core application with minimum assets for deploying to AWS Elastic Beanstalk.
Step-by-step guide for making HTTPS work in AWS Elastic Beanstalk single instance environment.

## BUILD INSTRUCTION

1. Install [.Net Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)

2. Clone the repository
`git clone https://github.com/usix79/dotnet-core-eb-https.git`

3. Restore dotnet tools (fake-cli is used)
`dotnet tool restore`

4. Run the application locally
`dotnet fake build -t run`

5. Open in browser (usually [http://localhost:5000](http://localhost:5000))

If all goes well, you will see such response  
> Hello from ASP.NET Core!

## SETUP AWS ENVIRONMENT INSTRUCTION

### Detailed information

[Deploying an ASP.NET core application with Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/dotnet-core-tutorial.html)

[Terminating HTTPS on Amazon EC2 instances running .NET](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/SSLNET.SingleInstance.html)

### Steps

1. Launch an Elastic Beanstalk environment  
    * Environment type: **Single Instance**
    * Platform: **.Net (Windows/IIS)**

    *At that point your environment is available by [Elastic Beanstalk environment's Domain name](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/customdomains.html)*

2. [Route Traffic to the Elastic Beanstalk Environment](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-beanstalk-environment.html)

    *At that point your environment is available by your domain name, but only through http*

3. Create SSL certificate
    * Use Let's Encript Cert Bot for [creating ssl certificate manually](https://certbot.eff.org/docs/using.html#manual)

        `sudo certbot certonly --manual --preferred-challenges dns`

    * Use OpenSSL for [creating pfx](https://www.ssl.com/how-to/create-a-pfx-p12-certificate-file-using-openssl/)

        `openssl pkcs12 -export -out cert.pfx -inkey privkey1.pem -in cert1.pem`

4. Upload certificate file (pfx) and file with password to S3

5. Setup access to S3
    * Create [Access Point](https://docs.aws.amazon.com/AmazonS3/latest/dev/creating-access-points.html)
    * Add an `AmazonS3ReadOnlyAccess` policy to the `aws-elasticbeanstalk-ec2-role` in IAM Management Console

6. Create bundle and deploy
    * In `https-instance-dotnet.config` asign propper values to `$bucket`, `$certkey` and `$pwdkey`
    * Create new bundle `dotnet fake build -t bundle`
    * Upload `dotnet-core-eb-https-bundle.zip` to the EB environment

If all goes well, you will see such response on https request to your domain
> Hello from ASP.NET Core!

### If something goes wrong

* Check logs from Elastic Beanstalk console
* Connet to your environment's E2 instance using [RDP](https://docs.amazonaws.cn/en_us/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) and explore `C:\cfn\log\cfn-init-cmd.log`
* Try run `C:\certs\install-cert.ps1` manually on the E2 instance  
