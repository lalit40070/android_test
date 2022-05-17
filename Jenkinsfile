pipeline {
    agent any
    options {
        buildDiscarder logRotator(daysToKeepStr: '3', numToKeepStr: '3')
    }
    stages {
        stage('npm install') { 
            steps {

                sh "npm install"
            }
        }
        stage('android setup') {
            steps {
                sh "rm -rf android"
                sh "npx cap add android"
               
            }
        } 
    
        stage('Android Lint') {
            steps {
                sh "cd /var/lib/jenkins/workspace/android-test/android && ./gradlew lint --warning-mode all"       
            }
        }
      stage('Android test') {
            steps {
                sh "cd /var/lib/jenkins/workspace/android-test/android && ./gradlew test --warning-mode all"       
            }
        }
      stage('Android check') {
            steps {
                sh "cd /var/lib/jenkins/workspace/android-test/android && ./gradlew check --warning-mode all"       
            }
        }
       stage('SonarQube analysis')  {
            steps {
                 withSonarQubeEnv('sonarqube-8.9.1') {
                   sh 'cd /var/lib/jenkins/workspace/android-test/android && ./gradlew sonarqube --warning-mode all'
                 }
           }
        }
       stage('build app') {
            steps {

                sh "npm run build"
            }
        }
      
       stage('sync with android') {
            steps {

                sh "npx cap sync"
            }
        }
       stage('build android') {
            steps {
                sh "cd /var/lib/jenkins/workspace/android-test/android && ./gradlew clean"  
                sh "cd /var/lib/jenkins/workspace/android-test/android && ./gradlew assembleDebug"
            }
        }
    }
   }
