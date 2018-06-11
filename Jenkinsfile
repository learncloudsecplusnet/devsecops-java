pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Maven Dependency Check') {
            steps {
                sh 'mvn verify'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Security Test'){
            steps {
                dependencyCheckAnalyzer datadir: '',
                hintsFile: '',
                includeCsvReports: false,
                includeHtmlReports: false,
                includeJsonReports: false,
                includeVulnReports: false,
                isAutoupdateDisabled: false,
                outdir: '',
                scanpath: '',
                skipOnScmChange: false,
                skipOnUpstreamChange: false,
                suppressionFile: '',
                zipExtensions: ''
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
