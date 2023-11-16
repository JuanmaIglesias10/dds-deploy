pipeline {
    agent any
    
    stages {
        stage('Obtener Código') {
            steps {
                git 'https://github.com/JuanmaIglesias10/dds-deploy.git'
            }
        }
        
        stage('Construir') {
            steps {
                sh 'mvn clean install'
            }
        }
        
        stage('Pruebas') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Desplegar') {
            steps {
                sh 'deploy-script.sh'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline exitoso. ¡Felicidades!'
        }
        failure {
            echo 'El pipeline falló. Revisar los logs para más detalles.'
        }
    }
}
