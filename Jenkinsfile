pipeline {
    agent any
    stages {
        stage ('Checkout') {
            steps {
                git branch:'master', url: 'https://github.com/DamianNJJ/DSS_Lab8_vulnado.git'
            }
        }
        
        stage ('Build') {
            steps {
                sh '/var/jenkins_home/apache-maven-3.9.8/bin/mvn --batch-mode -V -U -e clean verify -Dsurefire.useFile=false -Dmaven.test.failure.ignore'
            }
        }

        stage ('Analysis') {
            steps {
                sh '/var/jenkins_home/apache-maven-3.9.8/bin/mvn --batch-mode -V -U -e checkstyle:checkstyle pmd:pmd pmd:cpd findbugs:findbugs'
            }
        }
    }

    post {
        always {
            junit testResults: '**/target/surefire-reports/TEST-*.xml'
            recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
            recordIssues enabledForFailure: true, tools: checkStyle(pattern: '**/target/checkstyle.xml')
            recordIssues enabledForFailure: true, tools: spotBugs(pattern: '**/target/findbugsXml.xml')
            recordIssues enabledForFailure: true, tools: cpd(pattern: '**/target/cpd.xml')
            recordIssues enabledForFailure: true, tools: pmdParser(pattern: '**/target/pmd.xml')
        }
    }
}
