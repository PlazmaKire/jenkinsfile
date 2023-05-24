pipeline {
    agent any

    stages {
        stage('install-pip-deps') {
            steps {
                clone_app()
                echo 'Installing dependencies'
                bat "pip3 install -r requirements.txt"
            }
        }
        stage('deploy-to-dev') {
            steps {
                deploy("dev", "7001")
            }
        }
        stage('tests-on-dev') {
            steps {
                test("dev")
            }
        }
        stage('deploy-to-staging') {
            steps {
                deploy("staging", "7002")
            }
        }
        stage('tests-on-staging') {
            steps {
                test("staging")
            }
        }
        stage('deploy-to-preprod') {
            steps {
                deploy("preprod", "7003")
            }
        }
        stage('tests-on-preprod') {
            steps {
                test("preprod")
            }
        }
        stage('deploy-to-prod') {
            steps {
                deploy("prod", "7004")
            }
        }
        stage('tests-on-prod') {
            steps {
                test("prod")
            }
        }
    }
}

def clone_app() {
    echo 'Cloning app'
    git(
        url: "https://github.com/mtararujs/python-greetings",
        branch: "main",
        changelog: false,
        poll: false
    )
}

def deploy(String vides_nosaukums, String porta_numurs) {
    clone_app()
    echo 'Deploying app'
    bat "pm2 delete greetings-app-${vides_nosaukums} & EXIT /B 0"
    bat "pm2 start app.py --name greetings-app-${vides_nosaukums} -- --port ${porta_numurs}"
}

def test(String vides_nosaukums) {
    echo 'Cloning tests'
    git(
        url: "https://github.com/mtararujs/course-js-api-framework",
        branch: "main",
        changelog: false,
        poll: false
    )
    echo 'Installing dependencied and running tests'
    bat "npm install"
    bat "npm run greetings greetings_${vides_nosaukums}"
}