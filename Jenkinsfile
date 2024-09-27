pipeline {
    agent any
    parameters {
        string(name: 'JMETER_PATH', defaultValue: '/jmeter', description: 'Path to JMeter')
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/EvgeniyGerasimov/load_test.git', branch: 'master'
            }
        }
        stage('Run JMeter Tests') {
            steps {
                sh """
                ${params.JMETER_PATH}/bin/jmeter -n -t load_test.jmx -l test_results.jtl
                """
            }
        }
        stage('Generate HTML Report') {
            steps {
                sh """
                ${params.JMETER_PATH}/bin/jmeter -g test_results.jtl -o jmeter-report
                """
            }
        }
        stage('Publish HTML Report') {
            steps {
                publishHTML(target: [
                    reportName : 'JMeter HTML Report',
                    reportDir  : 'jmeter-report',
                    reportFiles: 'index.html',
                    alwaysLinkToLastBuild: true,
                    keepAll: true
                ])
            }
        }
    }
}