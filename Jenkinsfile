pipeline {
    agent any
    environment {
        IONIC_TOKEN = credentials('Ionic-Token')
    }

    stages {
        stage('Install Cloud CLI') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    sh 'curl -fsSL https://ionic.io/get-ionic-cloud-cli | bash'
                }
            }
        }
        stage('build iOS'){
            steps {
                script{
                env.APPFLOW_BUILD_ID = sh(returnStdout:true, script:'/usr/local/bin/ionic-cloud build ios app-store --app-id 190847cd --commit="$GIT_COMMIT" --signing-cert delpoydemo --token=$IONIC_TOKEN --json | /opt/homebrew/bin/jq -r ".buildId"').trim()
                }
            }
        }
        
        stage('Deploy iOS') {
            steps {
                sh (returnStdout:true, script:'/usr/local/bin/ionic-cloud deploy ios --app-id=190847cd --build-id="$APPFLOW_BUILD_ID" --destination "TestFlight" --token=$IONIC_TOKEN')
            }
        }
    }
}
