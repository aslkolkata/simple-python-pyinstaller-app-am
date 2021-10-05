pipeline {
    agent none
    stages {
        stage('Deliver') {
            agent any
            environment {
                //VOLUME = '$(pwd)/sources:/src'
                //IMAGE = 'cdrx/pyinstaller-linux:python3'
                VOLUME = '$(pwd)/sources:/src'
                IMAGE = 'pschmitt/pyinstaller:3.9@sha256:250740779789a1002717ff4a0d5b2f708e56d29e79e7711b6b7fd371287fabc2'
            }
            steps {
                stash(name: 'compiled-results', includes: 'sources/*.py*')
                dir(path: env.BUILD_ID) {
                    unstash(name: 'compiled-results')
                    sh "docker run --rm -v ${VOLUME} ${IMAGE} '/app/pyinstaller -F add2vals.py'"
                }
            }
            post {
                success {
                    sh "mkdir -p add"
                    sh "cp ${env.BUILD_ID}/sources/dist/add2vals add/"
                    sh "tar -czvf add.tgz add"
                    archiveArtifacts "add.tgz"
                    //archiveArtifacts "${env.BUILD_ID}/sources/dist/add2vals"
                    //sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
                }
            }
        }
    }
}
