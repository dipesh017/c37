pipeline{
    agent any
    options{
        buildDiscarder(logRotator(daysToKeepStr: '15'))
        retry(2)
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }
    triggers{
        cron('H */4 * * *')
        pollSCM('H */4 * * *')
    }
    parameters{
        string(name: 'BRANCH', defaultValue: 'develop')
        booleanParam(name: 'Boolean', defaultValue: true)
        choice(name: 'OPERATION', choices: ['A','S','M','D'])
    }
    stages{
        stage('Git pull'){
            steps{
                sh "echo git pull"
            }
        }
        stage('Build Stage'){
            steps{
                sh "echo docker build -t imagename ."
                sh "echo docker push imagename"
            }
        }
        stage('Testing'){
            when{
                expression {
                    return env.BRANCH.equals('master');
                }
            }
            parallel{
                stage('Linux Testing'){
                    steps{
                        sh "echo linux testing"
                        sh "sleep 60"
                    }
                }
                stage('Windows Testing'){
                    steps{
                        sh "echo windows testing"
                        sh "sleep 60"
                    }
                }
                stage('Others Testing'){
                    steps{
                        sh "echo others testing"
                        sh "sleep 60"
                    }
                }
            }
        }
        stage('Deploy Stage'){
            steps{
                sh "echo new TD"
                sh "echo update service"
            }
        }
    }
    post{
        always{
            echo 'Some final actions'
        }
    }
}
