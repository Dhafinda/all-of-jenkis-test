pipeline {
    agent any

    environment {
        PDI_HOME = "C:\\Users\\dhafinda.diara\\Desktop\\PENTAHO\\data-integration"
        PDI_JOB_PATH = "Pentaho_Jobs\\job_test.kjb"
        PDI_TRANS_PATH = "Pentaho_Transformation\\Transformation_to_pg.ktr"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate Files') {
            steps {
                script {
                    def jobFile = "${env.WORKSPACE}\\${env.PDI_JOB_PATH}"
                    def transFile = "${env.WORKSPACE}\\${env.PDI_TRANS_PATH}"

                    // Cek keberadaan file job
                    if (!fileExists(jobFile)) {
                        error "File Pentaho Job tidak ditemukan di path: ${jobFile}"
                    } else {
                        echo "File Pentaho Job ditemukan: ${jobFile}"
                    }

                    // Cek keberadaan file transformation
                    if (!fileExists(transFile)) {
                        error "File Pentaho Transformation tidak ditemukan di path: ${transFile}"
                    } else {
                        echo "File Pentaho Transformation ditemukan: ${transFile}"
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

        stage('Run Pentaho Transformation') {
            steps {
                script {
                    def panCmd = "\"${env.PDI_HOME}\\pan.bat\" /file=\"${env.WORKSPACE}\\${env.PDI_TRANS_PATH}\" /level=Basic"
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
