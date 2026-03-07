pipeline {

    agent any

    environment {
        // Docker registry details
        registry = "esraaghazal/vprofileapp"

        // Docker registry credentials ID in Jenkins
        registryCredential = 'dockerhub'
    }

    stages{

        // if you want to fetch code from git, you can use the below stage, otherwise you can remove it and start with build stage
    /*  stage('fetching code') {
             steps {
                git'http://xxx'
        } */

        // build the application using maven and skip tests to save time, we will run tests in the next stages
        stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        // run unit tests using maven and fail the build if any test fails, you can also generate test reports using maven suref
        stage('UNIT TEST'){
            steps {
                sh 'mvn test'
            }
        }

        // run integration tests using maven and skip unit tests to save time, you can also generate test reports using maven suref
        stage('INTEGRATION TEST'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }


        // run code analysis using checkstyle plugin and generate reports, you can also fail the build if any violation is found
        stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }

        // build docker image using dockerfile in the project git hub repository
        stage('Building image') {
            steps{
              script {
                dockerImage = docker.build registry + ":$BUILD_NUMBER"
              }
            }
        }
        // push the image to docker hub registry using the credentials stored in Jenkins
        stage('Deploy Image') {
          steps{
            script {
              docker.withRegistry( '', registryCredential ) {
                dockerImage.push("$BUILD_NUMBER")
                dockerImage.push('latest')
              }
            }
          }
        }
        
        // remove the image from local docker registry to free up space
        stage('Remove Unused docker image') {
          steps{
            sh "docker rmi $registry:$BUILD_NUMBER"
          }
        }


        // deploy the application to kubernetes cluster using helm charts
        stage('Kubernetes Deploy') {
	    agent { label 'k8s' }
            steps {
                    sh "helm upgrade --install --force vproifle-stack helm/vprofilecharts --set appimage=${registry}:${BUILD_NUMBER} --namespace prod"
            }
        }

    }


}