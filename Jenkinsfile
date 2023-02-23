pipeline {
    agent any
    tools {
        jdk 'openjdk-8'
        maven '3.8.7'
    }
    environment {
        BUILD_VERSION = "v${currentBuild.number}.RELEASE"
    }
    stages {
        stage('check source') {
            steps {
                echo 'Hello World'
                git branch: 'main', url: 'https://github.com/attiqbhai/jenkins-test'
            }
        }
        stage('compile maven') {
            steps {
                echo 'Hello World'
                configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
                    sh "mvn clean package -s $MAVEN_SETTINGS"
                }
            }
        }
        stage('deploy to nexus') {
            steps {
                echo 'Hello World'
                configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
                    sh "mvn clean deploy -DaltDeploymentRepository=nexus::default::http://localhost:8081/repository/maven-snapshots/ -s $MAVEN_SETTINGS"
                }
            }
        }
        stage('Pull from Nexus') {
            steps {
                echo 'deploy to development'
                configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
                    sh """mvn -s $MAVEN_SETTINGS org.apache.maven.plugins:maven-dependency-plugin:3.5.0:copy -Dartifact=com.mycompany:jenkins-test:1.0.0-20230223.030524-15:jar:mule-application -DoutputDirectory=./"""
                    sh """mvn -s $MAVEN_SETTINGS org.apache.maven.plugins:maven-dependency-plugin:3.5.0:copy -Dartifact=com.mycompany:jenkins-test:1.0.0-20230223.030524-15:pom -DoutputDirectory=./"""
                }
            }
            post {
                success {
                    echo "...Jar pulled successfully"
                } 
                unsuccessful {
                    echo "...Jar pull failed"
                }
            }
        }
        stage('Deploy to Dev') {
            steps {
                echo 'deploy to development'
                configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
                    sh """mvn -U --batch-mode -s $MAVEN_SETTINGS -Dmule.version=4.4.0 -Danypoint.applicationName=jenkins-test -Dartifact=jenkins-test-1.0.0-20230223.030524-15-mule-application.jar -Danypoint.mule.environment=DEV -Danypoint.username=feb-2023 -Danypoint.password=America100! -Dcloudhub.env=DEV -Dcloudhub.region=us-east-1 -Dcloudhub.workerType=MICRO -Dcloudhub.workers=1 -Dcloudhub.env.client_id=cd474d9d0881480ebbca5f74e16e83f2 -Dcloudhub.env.client_secret=e82faAEBFe824e179b962Aca3172E9Af mule:deploy"""
                }
            }
            post {
                success {
                    echo "...Application deployed successfully"
                } 
                unsuccessful {
                    echo "...Application deployment failed"
                }
            }
        }
    }
}