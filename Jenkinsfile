pipeline {
    agent any

    stages {
        stage('Checkout code') {
            //checkout the repository
            steps {
                git branch: 'main', url: 'https://github.com/Pain1003/JenkinsSeleniumIDE_DemoRepo_8_15'
            }
        }
        stage('Set up .Net Core') {
            //install dot net
            steps {
                bat '''
                Echo Downloading .Net 6 SDK
                curl -l -o dotnet-sdk-6.0.133-win-x86.exe https://download.visualstudio.microsoft.com/download/pr/0b870df1-1fae-489f-a035-2cbd41726cd4/d44c685cd37ac022c12eab695d14694b/dotnet-sdk-6.0.133-win-x86.exe
                echo Installing dotnet-sdk-6.0.133-win-x86.exe
                dotnet-sdk-6.0.133-win-x86 /quiet /norestart
                '''
            }
        }
        stage('Restore dependencies') {
            //install dependencies
            steps {
                bat 'dotnet restore SeleniumIde.sln'
            }
        }
        stage('Build') {
            //build
            steps {
                bat 'dotnet build SeleniumIde.sln --configuration Release'
            }
        }
        stage('Run Tests') {
            //run tests
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step ([
                $class: 'MSTestPublisher',
                TestResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}