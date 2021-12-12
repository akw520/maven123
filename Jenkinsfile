pipeline
{
    agent any
    stages
    {
        stage('ContinuousDownload')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/selenium-saikrishna/maven.git'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins is unable to download the dev code from github', cc: '', from: '', replyTo: '', subject: 'Download Failed', to: 'git.team@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousBuild')
        {
            steps
            {
                script
                {
                    try
                    {
                    sh 'mvn package'
                    }
                    catch(Exception e2)
                    {
                        mail bcc: '', body: 'Jenkins is unable to create an artifact from the downloaded code', cc: '', from: '', replyTo: '', subject: 'Build Failed', to: 'developers.team@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousDeployment')
        {
            steps
            {
                script
                {
                    try
                    {
                        deploy adapters: [tomcat9(credentialsId: 'a617bd4b-b51e-4385-99d2-35a8c69a6dd9', path: '', url: 'http://18.206.254.36:8080')], contextPath: 'testapp', war: '**/*.war'
                    }
                    catch(Exception e3)
                    {
                        mail bcc: '', body: 'Jenkins is unable to deploy an artifact into tomcat on QAservers', cc: '', from: '', replyTo: '', subject: 'Build Failed', to: 'developers.team@gmail.com'
                        exit(1)
                    }
                }    
            }
        }
         stage('ContinuousTesting')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/selenium-saikrishna/FunctionalTesting.git'
                sh 'java -jar /var/lib/jenkins/workspace/DeclarativePipeline3/testing.jar'
                    }
                    catch(Exception e4)
                    {
                        
                        mail bcc: '', body: 'Selenium test scripts are failing', cc: '', from: '', replyTo: '', subject: 'Testing Failed', to: 'qa.team@gmail.com'
                        exit(1)
                    }
                }
            }
    }
     stage('ContinuousDelivery')
    {
        steps
        {
            script
            {
                try
                {
                    deploy adapters: [tomcat9(credentialsId: 'a617bd4b-b51e-4385-99d2-35a8c69a6dd9', path: '', url: 'http://23.23.57.211:8080')], contextPath: 'prodapp', war: '**/*.war'
                }
                catch(Exception e5)
                {
                    
                }
            }
        }
    }
}
