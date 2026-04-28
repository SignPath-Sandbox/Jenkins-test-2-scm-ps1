pipeline {
    agent any

    stages {
        stage('Create PowerShell file') {
            steps {
                sh '''
                    mkdir -p build-output
                    echo 'Write-Host "Hello World 2 - revision 1 - main branch"' > build-output/script.ps1
                    ls -lh build-output
                    cat build-output/script.ps1
                '''
            }
        }

        stage('Archive artifact') {
            steps {
                archiveArtifacts artifacts: 'build-output/**', fingerprint: true
            }
        }

        stage('Sign with SignPath') {
            steps {
                submitSigningRequest(
                    apiTokenCredentialId: 'SignPath.ApiToken.QA1',
                    projectSlug: 'Jenkins-Project',
                    signingPolicySlug: 'Jenkins_signing_policy',
                    artifactConfigurationSlug: 'Powershell',
                    inputArtifactPath: 'build-output/script.ps1',
                    waitForCompletion: true
                )
            }
        }
    }
}