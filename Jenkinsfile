pipeline {
    agent any
    parameters {
        string(name: 'JMETER_PATH', defaultValue: '/jmeter', description: 'Path to JMeter')
        string(name: 'NUMBER_OF_THREADS', defaultValue: '1', description: 'Number of threads for JMeter tests')
        string(name: 'PERIOD_LOOPS', defaultValue: '1', description: 'Number of loops for JMeter tests')
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/EvgeniyGerasimov/load_test.git', branch: 'master'
            }
        }
        stage('Clean Reports') {
            steps {
                // Удаляем старые отчеты перед новым запуском
                sh 'rm -rf jmeter-report test_results.jtl'
            }
        }
        stage('Run JMeter Tests') {
            steps {
                sh "${params.JMETER_PATH}/bin/jmeter -n -t swapi.jmx -l test_results.jtl -JNUMBER_OF_THREADS=${params.NUMBER_OF_THREADS} -JPERIOD_LOOPS=${params.PERIOD_LOOPS}"
            }
        }
        stage('Generate HTML Report') {
            steps {
                sh "${params.JMETER_PATH}/bin/jmeter -g test_results.jtl -o jmeter-report"
            }
        }
        stage('Publish Performance Report') {
            steps {
                perfReport(sourceDataFiles: 'test_results.jtl', 
                           failOnError: true, 
                           canRunOnFailed: true, 
                           graphTitle: 'Performance Graph', // Заголовок графика
                           group: 'By response code') // Группировка данных на графиках
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