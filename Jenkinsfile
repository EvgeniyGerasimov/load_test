pipeline {
    agent any
    stages {
        stage('Clean Previous Results') {
            steps {
                sh 'rm -rf test_results.jtl jmeter-report'
            }
        }
        stage('Checkout') {
            steps {
                git url: 'https://github.com/EvgeniyGerasimov/load_test.git', branch: 'master'
            }
        }
        stage('Run JMeter Tests') {
            steps {
                sh '/opt/apache-jmeter-5.6.3/bin/jmeter -n -t swapi.jmx -l test_results.jtl'
            }
        }
        stage('Generate HTML Report') {
            steps {
                sh '/opt/apache-jmeter-5.6.3/bin/jmeter -g test_results.jtl -o jmeter-report'
            }
        }
        stage('Publish Performance Report') {
            steps {
                perfReport(sourceDataFiles: 'test_results.jtl')
            }
        }
        stage('Publish HTML Report') {
            steps {
                publishHTML(target: [
                    reportName: 'JMeter HTML Report',
                    reportDir: 'jmeter-report',
                    reportFiles: 'index.html',
                    alwaysLinkToLastBuild: true,
                    keepAll: true
                ])
            }
        }
    }
}