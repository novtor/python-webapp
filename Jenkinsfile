#!/usr/bin/groovy

pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }

    stages {
        stage("Build") {
            steps{ buildApp() }
        }

        stage("Deploy dev") {
            steps{ deploy("dev") }
        }
    }
}

def buildApp() {
    dir('.') {
        def appImage = docker.build("novtor/python-webapp:${BUILD_NUMBER}")
    }
}

def deploy(env) {
    def containerName = ''
    def port = ""

    if("${env}" == "dev") {
        containerName = "python-app-dev"
        port = '8888'
    } else {
        println "Environment ${env} is not valid"
        System.exit()
    }

    sh "docker ps -f name=${containerName} -q | xargs -no-run-if-empty docker stop"
    sh "docker ps -a -f name=${containerName} -q | xargs -r docker rm"
    sh "docker run -d -p ${port}:5000 --name ${containerName} novtor/python-webapp:${BUILD_NUMBER}"

}