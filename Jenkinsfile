pipeline {
    agent any
    environment {
        IONIC_TOKEN = credentials('Ionic-Token')
    }

    stages {
        stage('Install Ionic Cloud CLI') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'SUCCESS') {
                    sh 'curl -fsSL https://ionic.io/get-ionic-cloud-cli | bash'
                }
            }
        }
        stage('build iOS'){
            steps {
                script{
                env.APPFLOW_BUILD_ID = sh(returnStdout:true, script:'/usr/local/bin/ionic-cloud build ios app-store --app-id 089867d5 --commit 89f8c9 --signing-cert delpoydemo --token=$IONIC_TOKEN --json | /opt/homebrew/bin/jq -r ".buildId"').trim()
                }
                sh 'echo $APPFLOW_BUILD_ID'
            }
        }
        
        stage('Deploy ios') {
            steps {
                sh '/usr/local/bin/ionic-cloud deploy ios --app-id=089867d5 --build-id="$APPFLOW_BUILD_ID" --destination "deploydemo" --token=$IONIC_TOKEN'
            }
        }
    }
}
