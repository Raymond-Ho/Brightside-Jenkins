pipeline {
    agent { label 'ca-brightside-2.0-agent' }
    environment {
        BUILD_COBOL = "./jenkins/build.sh"
        DEPLOY_SCRIPT = "./jenkins/deploy.sh"
        TEST_SCRIPT = "./jenkins/test.sh"

        ZOWE_OPT_HOST = credentials('apimlHost')
        ZOWE_OPT_PORT = credentials('apimlPort')
        ZOWE_OPT_BASE_PATH = credentials('zosmfBasePath')
        ZOWE_OPT_REJECT_UNAUTHORIZED = false

        BUILD_JCL = credentials('buildJCL')
        TEST_LOADLIB = credentials('testLOADLIB')
        COPY_JCL = credentials('copyJCL')
        RUN_JCL = credentials('runJCL')
    }
    stages {
        stage('build') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'bright --version'
                sh 'npm install'
                withCredentials([usernamePassword(credentialsId: 'zosmfCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASS')]) {
                    sh "chmod +x $BUILD_COBOL && $BUILD_COBOL"
                }
            }
        }
        stage('deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'zosmfCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASS')]) {
                    sh "chmod +x $DEPLOY_SCRIPT && $DEPLOY_SCRIPT"
                }
            }
        }
        stage('test') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'zosmfCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASS')]) {
                    sh "chmod +x $TEST_SCRIPT && $TEST_SCRIPT"
                }
            }
        }
    }
}