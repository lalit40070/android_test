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
                sh "cd /var/lib/jenkins/workspace/android-test/android && ./gradlew lint"       
            }
        }
      stage('Android test') {
            steps {
                sh "cd /var/lib/jenkins/workspace/android-test/android && ./gradlew test"       
            }
        }
      stage('Android check') {
            steps {
                sh "cd /var/lib/jenkins/workspace/android-test/android && ./gradlew check"       
            }
        }
      stage('Slack message') {
        steps {
        slackSend color: '#BADA55', message: 'Approval request from Jenkins'
    }
    }
       stage('Approval Request') {
             steps { 
               input "Deploy to Prod?"
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
