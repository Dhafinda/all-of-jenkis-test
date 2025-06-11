pipeline {
    agent any

    environment {
        PDI_HOME = "C:\\Users\\dhafinda.diara\\Desktop\\PENTAHO\\data-integration"
        PDI_JOB_PATH = "Pentaho_Jobs\\job_test.kjb"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate Job File') {
            steps {
                script {
                    def jobFile = "${env.WORKSPACE}\\${env.PDI_JOB_PATH}"

                    // Cek keberadaan file job
                    if (!fileExists(jobFile)) {
                        error "File Pentaho Job tidak ditemukan di path: ${jobFile}"
                    } else {
                        echo "File Pentaho Job ditemukan: ${jobFile}"
                    }
                }
            }
        }

        stage('Run Pentaho Job') {
            steps {
                script {
                    def kitchenCmd = "\"${env.PDI_HOME}\\kitchen.bat\" /file=\"${env.WORKSPACE}\\${env.PDI_JOB_PATH}\" /level=Basic"
                    echo "Running Pentaho Job with command: ${kitchenCmd}"

                    def result = bat(script: kitchenCmd, returnStatus: true)
                    if (result != 0) {
                        error "Pentaho Job execution failed with exit code: ${result}"
                    }
                }
            }
        }
    }
}
