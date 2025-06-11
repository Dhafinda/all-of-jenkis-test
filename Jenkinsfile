pipeline {
    agent any

    environment {
        //
        PDI_HOME = "C:\Users\dhafinda.diara\Desktop\PENTAHO\data-integration\Spoon.bat"
        
        // 
        PDI_JOB_PATH = "C:/Users/dhafinda.diara/Downloads/test data/job_test.kjb"
        PDI_TRANS_PATH = "C:/Users/dhafinda.diara/Downloads/test data/Transformation to pg.ktr"
    }

    stages {
        stage('Checkout') {
            steps {
                // Adjust or remove if not using SCM
                checkout scm
            }
        }

        stage('Run Pentaho Job') {
            steps {
                script {
                    def kitchenCmd = "\"${env.PDI_HOME}/kitchen.bat\" /file=\"C:\Users\dhafinda.diara\Desktop\PENTAHO\data-integration\Kitchen.bat" /level=Basic"
                    echo "Running Pentaho Job with command: ${kitchenCmd}"
                    def result = bat(script: kitchenCmd, returnStatus: true)
                    if (result != 0) {
                        error "Pentaho Job execution failed with exit code: ${result}"
                    }
                }
            }
        }

        stage('Run Pentaho Transformation') {
            steps {
                script {
                    def panCmd = "\"${env.PDI_HOME}/pan.bat\" /file=\"C:\Users\dhafinda.diara\Desktop\PENTAHO\data-integration\Pan.bat" /level=Basic"
                    echo "Running Pentaho Transformation with command: ${panCmd}"
                    def result = bat(script: panCmd, returnStatus: true)
                    if (result != 0) {
                        error "Pentaho Transformation execution failed with exit code: ${result}"
                    }
                }
            }
        }
    }
}
