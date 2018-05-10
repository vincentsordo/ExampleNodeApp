# ExampleNodeApp

This project was created to document what is required to deploy a simple node application using Docker and AWS Elastic Beanstalk. Additionally I included information about creating SSL certificates with ACM and using AWS Route 53. When I was working on projects I wasn't able to easily find all this information and I hope this repository helps others who have struggled with deploying code to AWS. 

## Steps
1. Clone this repository: https://github.com/vincentsordo/ExampleNodeApp
2. Install nodejs and npm (if not already installed)
   - If you don’t have Homebrew you should download it. Open up a terminal and run `ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
   - brew install node - this will install nodejs and npm (node package manager)
   - Make sure that it worked by checking the version of node and npm
     - node --version
     - npm --version
3. Run the project locally with `npm start` or `node app.js`
4. Install Docker
   - AWS only supports up to [17.12.0-ce](https://download.docker.com/mac/stable/23011/Docker.dmg) of Docker, install this version for now
5. Build the app `docker build -t examplenodeapp .`
6. View the built app `docker image ls`
7. Run app `docker run -d -p 80:3000 examplenodeapp`
8. Install Elastic Beanstalk CLI
   - note: I am assuming that you have already have an AWS account. If not you should create one before moving on.
   - Install elastic beanstalk cli `brew install aws-elasticbeanstalk`
9. Link eb cli to aws account `eb config`
   - Set up your aws key and secret
   - I suggest that you create a new IAM user instead of using your master account for security reasons
10. Initialize your project `eb init`
    - Follow the instructions
    - I don’t enable code commit or ssh
11. Create elastic beanstalk application and environment `eb create`
    - Warning: make sure all files are committed and pushed otherwise the create command may fail
    - Choose: Classic load balancer
    - This whole process will take some time, however you will see updates in the terminal
    - You can log into your AWS account at this time
    - Go to Services->Elastic beanstalk
    - Click on your application that you created ExampleNodeApp
    - Click on the environment that you created ExampleNodeApp-dev1
    - Click on the url and you should see hello world
12. [Create SSL certificate](https://docs.aws.amazon.com/acm/latest/userguide/acm-services.html)
13. Add HTTPS Listener to Load Balancer
    - Edit configuration of the environment that you created using the eb cli
    - Click on configuration tab on the left
    - Click on Modify on the Load Balancer configuration
    - Add new listener for HTTPS
      - Listener Port=443
      - Listener protocol=HTTPS
      - Instance port=80
      - Instance protocol=HTTP
      - Select SSL cert that you created in the Creating SSL certificate section
14. [Create Alias in Amazon 53 Service](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-elb-load-balancer.html)
15. Update Your Domain provider to use Amazon name servers
