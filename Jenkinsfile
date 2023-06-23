pipeline {
    agent any
    parameters {
        choice(name: 'NUMBER',
            choices: [10,20,30,40,50,60,70,80,90],
            description: 'Select the value for F(n) for the Fibonnai sequence.')
    }
    options {
        // only keep 10 logs for no more than 10 days
        buildDiscarder(logRotator(daysToKeepStr: '10', numToKeepStr: '10'))
        // cause the build to time out if it runs for more than 12 hours
        timeout(time: 12, unit: 'HOURS')
        // add timestamps to the log
        timestamps()
    }
    triggers {
          // create a cron trigger that will run the job every day at midnight
          // note that the time is based on the time zone used by the server
          // where Jenkins is running, not the user's time zone
          cron '@midnight'
    }
    stages {
        stage('Make executable') {
            steps {
                sh('chmod +x ./scripts/fibonacci.sh')
            }
        }
        stage('Relative path') {
            steps {
                sh("./scripts/fibonacci.sh ${env.NUMBER}")
            }
        }
        stage('Full path') {
            steps {
                sh("${env.WORKSPACE}/scripts/fibonacci.sh ${env.NUMBER}")
            }
        }
        stage('Change directory') {
            steps {
                dir("${env.WORKSPACE}/scripts"){
                    sh("./fibonacci.sh ${env.NUMBER}")
                }
            }
        }
    }

    post {
        always {
            // THhe "steps{...}" section is not needed.
            echo "This step will run after all other steps have finished.  It will always run, even in the status of the build is not SUCCESS"
        }
    }
}