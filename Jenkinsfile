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

    //def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        checkout scm
    }

   
     stage('Create Scratch Org') {

            rc = bat (returnStatus: true, script: 'sfdx force:auth:jwt:grant --clientid 3MVG9YDQS5WtC11oFIMX1lJLuBxuBK.li4OED4JOyldL5T8M8zx8bayYiM8G2kUnVXj6Q39r4zZB1O9NNJlCn --username 18.abhimanyu@gmail.com --jwtkeyfile server.key --setdefaultdevhubusername -a my-devhub-org')
            println(rc)
	    if (rc != 0) { error 'hub org authorization failed' }
	   // rmsg = bat (returnStdout: true, script: 'sfdx force:org:create --definitionfile config/project-scratch-def.json --json --setdefaultusername')
	   // println (rmsg)
	   // def jsonSlurper = new JsonSlurperClassic()
           // def robj = jsonSlurper.parseText(rmsg)
           // if (robj.status != 0) { error 'org creation failed: ' + robj.message }
           // SFDC_USERNAME=robj.result.username
           // robj = null
        }
	
	stage('Push To Test Org') {
            rc = bat (returnStatus: true, script: 'sfdx force:source:push --targetusername test-emoazgqlq2wi@example.com')
            if (rc != 0) {
                error 'push failed'
            }
            // assign permset
            rc = bat (returnStatus: true, script: 'sfdx force:user:permset:assign --targetusername test-emoazgqlq2wi@example.com --permsetname DreamHouse')
            if (rc != 0) {
                error 'permset:assign failed'
            }
        }
	
	      stage('Run Apex Test') {
                timeout(time: 120, unit: 'SECONDS') {
                rc = bat (returnStatus: true, script: 'sfdx force:apex:test:run --testlevel RunLocalTests --outputdir tests --resultformat tap --targetusername test-emoazgqlq2wi@example.com')
                if (rc != 0) {
                    error 'apex test run failed'
                }
            }
        }
         
	      stage('collect results') {
            junit keepLongStdio: true, testResults: 'tests/**/*-junit.xml'
        }
        
    }
