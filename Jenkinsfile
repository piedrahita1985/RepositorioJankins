pipeline {
    agent any
    stages {
        
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarQubePruebas') {
                    bat "mvn clean verify sonar:sonar"
                }
            }
        }
        stage("Quality gate") {
            steps {
                timeout(time: 2, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        
    }
    post{
            success{
                bat "curl http://localhost:8080/job/job_122022/job/Despeglar_en_produccion/build?token=desplegarproduccion"
                bat "echo Tarea Desplegar en servidor de produccion Iniciada correctamente"
            }
            failure{
                bat "curl http://localhost:8080/job/job_122022/job/Despeglar_en_pruebas/build?token=desplegarpruebas"
                bat "echo Tarea notificar al correo Iniciada correctamente"
            }
        }
}
