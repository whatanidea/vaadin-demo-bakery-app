import com.cloudbees.hudson.plugins.modeling.*

pipeline {
    agent any
    
    stages {
        stage('Build') { 
            steps {
                sh 'mvn clean deploy' 
            }
        }
    }
}