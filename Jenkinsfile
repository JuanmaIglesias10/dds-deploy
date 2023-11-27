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
  stage('Construir y Publicar Imagen Docker') {
            steps {
                script {
                    // Construir la imagen Docker
                    docker.build(ddsdeploy)

                    // Iniciar sesión en Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com','dockerhub-credentials') {
                        // Hacer push de la imagen construida a Docker Hub
                        docker.image(ddsdeploy).push()
                    }
                }
            }
    }
}
