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

5. Check (usually on [http://localhost:5000](http://localhost:5000))

  If all goes well, you will see such response  
  > Hello from ASP.NET Core!
