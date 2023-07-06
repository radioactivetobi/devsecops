pipeline {
    agent any
    
    stages {
        
        stage ('Initialization') {
            steps {
                sh 'echo "Starting the CI pipeline."'
            }
        }
        
        stage ('Build') {
            steps {
                sh 'npm install'
            }
        }
        
        stage ('Secrets Scan') {
            steps {
                sh 'gitleaks detect --source=/var/jenkins_home/workspace/overboard-app --verbose --report-path=/var/jenkins_home/workspace/overboard-app/reports/gitleaks_results.csv'
            }
        }
        
        stage ('SAST - NodeJsScan') {
            steps {
                sh 'nodejsscan --directory `pwd` --output /var/jenkins_home/workspace/overboard-app/reports/nodejsscan-report'
            }
        }
        
        stage ('Software Composition Analysis - Retire.js') {
            steps {
                sh 'retire --path `pwd` --outputformat json --outputpath /var/jenkins_home/workspace/overboard-app/reports/retirejs-report --exitwith 0'
            }
        }
        
        stage ('Dependency-Check - OWASP') {
            steps {
                sh '/var/jenkins_home/dependency-check/bin/dependency-check.sh --scan `pwd` --format JSON --out /var/jenkins_home/workspace/overboard-app/reports/dependency-check-report --prettyPrint'
            }
        }
                
    }
}
