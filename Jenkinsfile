pipeline {
    agent any
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '10')
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
                sh "cd /var/lib/jenkins/workspace/android-test_Dev/android/ && ./gradlew lint"       
            }
        }
      stage('Android test') {
            steps {
                sh "cd /var/lib/jenkins/workspace/android-test_Dev/android/ && ./gradlew test"       
            }
        }
      stage('Android check') {
            steps {
                sh "cd /var/lib/jenkins/workspace/android-test_Dev/android/ && ./gradlew check"       
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
                sh "cd /var/lib/jenkins/workspace/android-test_Dev/android/ && ./gradlew clean"  
                sh "cd /var/lib/jenkins/workspace/android-test_Dev/android/ && ./gradlew assembleDebug"
            }
        }
      }
   }
