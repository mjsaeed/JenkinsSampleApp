pipeline {
    agent any
      environment {
        DIRECTORY_PATH= "c://project/app_code/"
        STAGING_SERVER= "ec2-user@17.5.x.x"
        PRODUCTION_SERVER= "ec2-user@17.5.x.x"
    }
    stages {
        stage('Build') { 
            steps {
                echo "run command -- cd $DIRECTORY_PATH"
                echo "run command -- git pull"
                echo "Build with Maven tool, run command -- mvn clean" // build the code 
            }
        }        
        stage('Unit and Integration Tests') { 
            steps {
                echo 'Unit tests with Maven, run command -- mvn test' // run unit tests 
                echo 'Integration tests with Maven, run command -- mvn integration-test' // run integration tests
            }
            post{
                success {
                    mail to: 'rayedalsrdi@gmail.com',
                    subject: 'Test Status Email',
                    body: 'Unit and Integration Tests were successful'
                }
            }
        } 
        stage('Code Analysis') { 
            steps {
                echo 'Code analysis with Maven and SonarQube, run command -- mvn clean verify sonar:sonar' // integrate Maven with SonarQube
            }
        }        
        stage('Security Scan') {
            steps {
                echo 'Security Scan with Maven and SonarQube, check credentials for SonarQube login' // use credentials for SonarQube login
            }
            post{
                success {
                    mail to: 'rayedalsrdi@gmail.com',
                    subject: 'Security Scan Status Email',
                    body: 'Security Scan with SonarQube was successful'
                }
            }            
        }        
        stage('Deploy to Staging') {
            steps {
                echo "Deploy to Staging server, run command -- ssh $STAGING_SERVER"
                echo 'run command cd /path/to/deployment && ./deploy.sh' // deploy to staging server using SSH
            }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                echo "Integration Tests on Staging, run command -- ssh $STAGING_SERVER"
                echo 'run command -- cd /path/to/deployment && ./run_integration_tests.sh' // run integration tests on staging server using SSH
            }
        }
        
        stage('Deploy to Production') {
            steps {
                echo "Deploy to Production server, run command -- ssh $PRODUCTION_SERVER"
                echo 'run command cd /path/to/deployment && ./deploy.sh' // deploy to Production server using SSH
            }
        }
    }
}
