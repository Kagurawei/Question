pipeline {
    agent {
        docker {
            image 'composer:latest'
        }
    }
    stages {
        stage('Build') {
            steps {
                dir('webapp') { // Setting working directory to 'webapp'
                    sh 'composer install'
                }
            }
        }
        stage('UnitTest') {
            steps {
                dir('webapp') { // Setting working directory to 'webapp'
                    sh './vendor/bin/phpunit --log-junit ../logs/unitreport.xml -c test/phpunit.xml test'
                }
            }
        }
        stage ('Checkout') {
	   steps {
	    git branch:'master', url: 'https://github.com/Kagurawei/Vulnerable-Web-Application.git'
	    }
	 }

	 stage('Code Quality Check via SonarQube') {
	  steps {
	   script {
	    def scannerHome = tool 'SonarQube';
	    withSonarQubeEnv('SonarQube') {
	    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=."
	    }
	  }
	}
     }
    }
    post {
        always {
            junit testResults: 'logs/unitreport.xml'
            recordIssues enabledForFailure: true, tool: sonarQube()
        }
    }
}

