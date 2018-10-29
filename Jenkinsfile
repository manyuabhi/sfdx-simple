#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME="test-emoazgqlq2wi@example.com"

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }


        stage('Push To Test Org') {
            bat 'sfdx force:config:set defaultusername=${SFDC_USERNAME}'
            rc = bat 'sfdx force:source:push --targetusername ${SFDC_USERNAME}'
            if (rc != 0) {
                error 'push failed'
            }
            // assign permset
            rc = bat 'sfdx force:user:permset:assign --targetusername ${SFDC_USERNAME} --permsetname DreamHouse'
            if (rc != 0) {
                error 'permset:assign failed'
            }
        }

        stage('Run Apex Test') {
            sh "mkdir -p ${RUN_ARTIFACT_DIR}"
            timeout(time: 120, unit: 'SECONDS') {
                rc = sh returnStatus: true, script: "sfdx force:apex:test:run --testlevel RunLocalTests --outputdir ${RUN_ARTIFACT_DIR} --resultformat tap --targetusername ${SFDC_USERNAME}"
                if (rc != 0) {
                    error 'apex test run failed'
                }
            }
        }

        stage('collect results') {
            junit keepLongStdio: true, testResults: 'tests/**/*-junit.xml'
        }
    }
