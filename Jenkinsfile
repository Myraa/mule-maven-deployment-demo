pipeline {
    agent any
    
    environment {

    PATH = "PATH;C:\\WINDOWS\\SYSTEM32"
    
    }
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                // Run Maven 
                cmd_exec("mvn clean install -Dmaven.test.skip=true")
                cmd_exec("echo %VERSION%")
               

            }
        }
        stage('Run munit Tests') {
            steps{
                cmd_exec("mvn -Dmaven.test.failure.ignore=true test")
            }
        }
        
        stage('Deploy to CloudHub'){
            environment {
                ANYPOINT_CREDENTIALS = credentials('anypoint.credentials')
            }
            steps{
                //deploy code to cloudHub
                cmd_exec("mvn deploy -P cloudhub -Dmule.version=3.9.4 -Danypoint.username=%ANYPOINT_CREDENTIALS_USR% -Danypoint.password=%ANYPOINT_CREDENTIALS_PSW%")
            }
        }
    }
}


def cmd_exec(command) {
    return bat(returnStdout: true, script: "${command}").trim()
}
