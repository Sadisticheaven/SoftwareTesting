pipeline {
    agent {
        docker {
            image 'maven:3.3.3'
            args '-v //C/Users/Administrator.UI5IK1XUKUTPTVR/.m2:/root/.m2'
        }
    }
    stages {
        stage('Compile') {
            steps {
                sh 'mvn -B clean compile'
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
        stage('Site') {
            steps {
                sh 'mvn site'
            }
            post {
                always {
                    cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: 'site report'])
                }
            }
        }
//         stage('tzs check') {
//             steps {
//                 input "Does the staging environment look ok?"
//             }
//         }
        stage('Deliver') {
            steps {
                sh 'echo "deb http://mirrors.aliyun.com/debian wheezy main contrib non-free">/etc/apt/sources.list'
                sh 'echo "deb-src http://mirrors.aliyun.com/debian wheezy main contrib non-free">>/etc/apt/sources.list'
                sh 'echo "deb http://mirrors.aliyun.com/debian wheezy-updates main contrib non-free">>/etc/apt/sources.list'
                sh 'echo "deb-src http://mirrors.aliyun.com/debian wheezy-updates main contrib non-free">>/etc/apt/sources.list'
                sh 'echo "deb http://mirrors.aliyun.com/debian-security wheezy/updates main contrib non-free">>/etc/apt/sources.list'
                sh 'echo "deb-src http://mirrors.aliyun.com/debian-security wheezy/updates main contrib non-free">>/etc/apt/sources.list'
                sh 'apt-get update && apt-get -y install sudo'
                sh 'sudo ./jenkins/scripts/deliver.sh'
            }
        }
    }
}