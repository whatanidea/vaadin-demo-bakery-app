pipeline {
    agent any
    tools {
      // Install the Maven version configured as "M3" and add it to the path.
      maven "maven 3.6.3"
   }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checkout..'
                // Get some code from a GitHub repository
               git branch: 'master', url: 'https://github.com/igor-baiborodine/vaadin-demo-bakery-app.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                sh "mvn clean dependency:go-offline"
            }
        }
        stage('Integration/Linter Test') {
            steps {
                echo 'Integration and Linter testing..'
                sh "mvn verify -Pit"
            }
        }
        stage('Build Frontend') {
            steps {
                echo 'Building..'
                sh "mvn com.github.eirslett:frontend-maven-plugin:1.7.6:install-node-and-npm -DnodeVersion=v10.16.0"
            }
        }
        stage('Scalabilty test') {
            steps {
                echo 'Testing..'
                sh "mvn -Pscalability gatling:test -Dgatling.sessionCount=10 -Dgatling.sessionStartInterval=50"
            }
        }
        stage('Deploy The Package') {
            steps {
                echo 'Deploying....'
                sh "scp -i $KEY **/target/*.war ec2-user@http://18.196.237.195:8080/usr/local/tomcat9/webapps/"
            }
        }
    }
}