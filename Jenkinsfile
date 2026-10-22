pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/shikharadhikari/Facelook-FSD-.git'
            }
        }

        stage('Validate') {
            steps {
                sh '''
                    set -eu
                    test -f index.html
                    test -f styles.css
                    test -f Groups.html
                    test -f Messages.html
                    test -f profile.html
                '''
            }
        }

        stage('Deploy Facelook') {
            steps {
                sh '''
                    set -eu
                    rsync -av --delete \
                        --exclude='.git/' \
                        --exclude='Jenkinsfile' \
                        ./ /var/www/website-two/
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'test -s /var/www/website-two/index.html'
                sh 'curl -fsS http://localhost:8082/ > /dev/null'
            }
        }
    }
}
