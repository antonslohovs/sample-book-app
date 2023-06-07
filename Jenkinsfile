pipeline {
    agent any
    triggers{ pollSCM('*/1 * * * *') }
    

    stages {
        stage('Build') {
            steps {
                script{
                    build()
                }
            }
        }
        stage('Deploy to DEV') {
            steps {
                script{
                    deploy("DEV", 1010)
                }
            }
        }
        stage('Tests on DEV') {
            steps {
                script{
                    test("BOOKS", "DEV")
                }
            }
        }
        stage('Deploy to STG') {
            steps {
                script{
                    deploy("STG", 2020)
                }
            }
        }
        stage('Tests on STG') {
            steps {
                script{
                    test("BOOKS", "STG")
                }
            }
        }
        stage('Deploy to PRD') {
            steps {
                script{
                    deploy("PRD", 3030)
                }
            }
        }
        stage('Tests on PRD') {
            steps {
                script{
                    test("BOOKS", "PRD")
                }
            }
        }
    }
}

// for windows: bat "npm.."
// for linux/macos: sh "npm .."

def build(){
    echo "Building of node application is starting.."
    bat "npm install"
    bat "npm test"
}

def deploy(String environment, int port){
    echo "Deployment to ${environment} has started.."
    bat "pm2 delete \"books-${environment}\""
    bat "pm2 start -n \"books-${environment}\" index.js -- -- ${port}"
}

def test(String test_set, String environment){
    echo "Testing ${test_set} test set on ${environment} has started.."
    bat "npm run ${test_set} ${test_set}_${environment}"
}