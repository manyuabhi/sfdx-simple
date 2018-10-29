#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME="test-4wwywaypihbn@example.com"

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    //def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

   
     stage('Create Scratch Org') {

            rc = bat 'sfdx force:auth:jwt:grant --clientid 3MVG9YDQS5WtC11oFIMX1lJLuBxuBK.li4OED4JOyldL5T8M8zx8bayYiM8G2kUnVXj6Q39r4zZB1O9NNJlCn --username 18.abhimanyu@gmail.com --jwtkeyfile server.key --setdefaultdevhubusername -a my-devhub-org'
            rmsg = bat 'force:org:create --definitionfile config/project-scratch-def.json --json --setdefaultusername'
            echo "$rc"
            echo "$rmsg"
            
        }
        
    }
