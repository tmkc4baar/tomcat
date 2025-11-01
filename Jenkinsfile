pipeline {
    agent any

    tools {
        jdk 'jdk21'
        maven 'maven3'
    }

    environment {
        WAR_FILE = 'target/roshambo.war' // Path to the generated WAR file
        TOMCAT_URL = 'http://localhost:9090' // Tomcat server URL
        TOMCAT_USER = 'impu1s3e' // Tomcat Manager username
        TOMCAT_PASSWORD = 'Wahid@p12' // Tomcat Manager password
    }

    stages {
        stage('Clean Project') {
            steps {
                bat "mvn clean"
            }
        }

        stage('Build Project') {
            steps {
                bat "mvn package"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFilePath = "${WORKSPACE}\\${WAR_FILE}".replace('/', '\\')
                    echo "WAR file path: ${warFilePath}"

                    if (fileExists(warFilePath)) {
                        echo 'WAR file found, proceeding with deployment...'

                        // Use caret (^) for line continuation in Windows batch
                        bat """
                            curl --upload-file "${warFilePath}" ^
                            --user ${TOMCAT_USER}:${TOMCAT_PASSWORD} ^
                            "${TOMCAT_URL}/manager/text/deploy?path=/roshambo&update=true"
                        """
                    } else {
                        error('WAR file not found! Build might have failed.')
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Build completed'
        }
    }
}
