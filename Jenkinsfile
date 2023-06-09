pipeline {
    agent any

    tools {
        //Que herramientas queremos utilizar
        maven "Maven3"
        jdk "Java11"
    }

    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE = 'sqlite'
    }

    stages {
        stage('Hello') {
            steps {
                echo "Database engine is ${DB_ENGINE}"
                echo "DISABLE_AUTH is ${DISABLE_AUTH}"
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                echo "Job Name: ${env.JOB_NAME}"
                echo "Java Name: ${env.JAVA_HOME}"
            }
        }
        stage('Git polling'){
            steps {
                //git branch: 'master', url: 'https://github.com/jejecristian/time-tracker.git'
                git branch: 'master', credentialsId: 'ssh', url: 'git@github.com:jejecristian/time-tracker.git'
            }
        }
        stage('Buidl con Maven'){
            steps{
                // bat "mvn clean package" //Windows
                // sh "mvn clean package" //Unix
                bat "mvn clean package -DskipTests" //no ejecuta los test
                // bat "mvn -Dmaven.test.failure.ignore=true clean package" //si existe falla la ignora, y contruye de todas maneras
            }
            post{
                success{
                    echo "Archivar Artefactos"
                    archiveArtifacts "core/target/*.jar"
                    archiveArtifacts "web/target/*.war"
                }
            }
        }
        stage('Test Maven'){
            steps{
                bat "mvn test"
            }
        }
    }
}
