pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/EvgeniyGerasimov/load_test.git', branch: 'master'
            }
        }
        stage('Run JMeter Tests') {
            steps {
                sh '/jmeter/bin/jmeter -n -t swapi.jmx -l test_results.jtl'
            }
        }
        stage('Generate HTML Report') {
            steps {
                sh '/jmeter/bin/jmeter -g test_results.jtl -o jmeter-report'
            }
        }
        stage('Publish Performance Report') {
            steps {
                perfReport(parsers: [
                    [parserName: 'jmeter', filePath: 'test_results.jtl']  // Укажите путь к вашему JTL файлу
                ])
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