pipeline {
    agent { label 'zowe-agent' }
    environment {
        // z/OS Host Information
        ZOWE_OPT_HOST=credentials('eosHost')            
    }
    stages {
        stage('local setup') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'zowe --version'
                sh 'zowe plugins list'
                sh 'npm install gulp-cli -g'
                //This line inserted
                sh 'rexx -version'
                sh 'npm install'

                //Create zosmf and fmp profiles, env vars will provide host, user, and password details
                sh 'zowe profiles create zosmf Jenkins --port 443 --ru false --host dummy --user dummy --password dummy'
                sh 'zowe profiles create fmp Jenkins --port 6001 --protocol http --host dummy --user dummy --password dummy'
            }
        }
        stage('Download Maintenance') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'eosCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASSWORD')]) {
                    sh 'echo download'
                }
            }
        }
        stage('Upload Maintenance') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'eosCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASSWORD')]) {
                    sh 'echo upload'
                }
            }
        }
        stage('Receive') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'eosCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASSWORD')]) {
                    sh 'echo receive'
                }
            }
        }
        stage('Apply-Check') {
            input {
                message "Proceed to Apply-Check?"
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'eosCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASSWORD')]) {
                    sh 'echo apply_check'
                }
            }
        }
        stage('Apply') {
            input {
                message "Proceed to Apply?"
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'eosCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASSWORD')]) {
                    sh 'echo apply'
                }
            }
        }
        stage('Deploy') {
            input {
                message "Proceed to Deploy?"
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'eosCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASSWORD')]) {
                    //To deploy the maintenace, an OPS profile needs to be created since profile options are not exposed on the command line
                    sh 'zowe profiles create ops Jenkins --host $ZOWE_OPT_HOST --port 6007 --protocol http --user $ZOWE_OPT_USER --password $ZOWE_OPT_PASSWORD'
                    sh 'echo deploy'
                }
            }
        }
        stage('Test') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'eosCreds', usernameVariable: 'ZOWE_OPT_USER', passwordVariable: 'ZOWE_OPT_PASSWORD')]) {
                    sh 'echo test'
                }
            }
        }
    }
    // post {
    //     always {
    //         archiveArtifacts artifacts: '*-archive/**/*.*' 
    //         publishHTML([allowMissing: false,
    //             alwaysLinkToLastBuild: true,
    //             keepAll: true,
    //             reportDir: 'mochawesome-report',
    //             reportFiles: 'mochawesome.html',
    //             reportName: 'Test Results',
    //             reportTitles: 'Test Report'
    //             ])
    //     }
    // } 
 }