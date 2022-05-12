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
                /*sh "export ANDROID_HOME=/var/lib/jenkins/android-sdk"
                sh "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64"*/
                sh "cd /var/lib/jenkins/workspace/Demo/android && ./gradlew clean"  
                sh "cd /var/lib/jenkins/workspace/Demo/android && ./gradlew assembleDebug"
            }
        }
      }
   }
