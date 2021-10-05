pipeline {
    agent none
    stages {
        stage('Deliver') {
            agent any
            environment {
                VOLUME = '$(pwd)/sources:/src'
                IMAGE = 'cdrx/pyinstaller-linux:python3'
            }
            steps {
                dir(path: env.BUILD_ID) {
                    sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'"
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
