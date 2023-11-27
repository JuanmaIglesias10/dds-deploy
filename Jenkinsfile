node {
stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def mvn = tool 'mavenTp';
    withSonarQubeEnv() {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=sonarQube -Dsonar.projectName='sonarQube'"
    }
  }
}
agent {label 'linux'} 
environment {
	DOCKERHUB_CREDENTIALS=credentials('dockerhub-credentials')}

	stages {
		stage('Build') {

			steps {
				sh 'docker build -t jiglesiass/ddsdeploy:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push jiglesiass/ddsdeploy:latest'
			}
		}
}
