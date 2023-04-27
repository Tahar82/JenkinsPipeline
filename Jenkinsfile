pipeline {
    agent any
    parameters {
        string(name: 'URL_MOVIE', defaultValue: '', description: 'Link of url ')
        string(name: 'BRANCH_GIT', defaultValue: '', description: 'branch of github link')
    }
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Git') {
            steps {
                // Get some code from a GitHub repository
                 git branch:"${params.BRANCH_GIT}",
                     url:"${params.URL_MOVIE}"
            }
        }
        stage('Compile') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn clean compile"

                // To run Maven on a Windows agent, use  // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('Test') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit '**/target/surefire-reports/TEST-.xml'
                }
            }
        }
        stage('Package') {
            steps {
                sh "mvn -DskipTests -Dmaven.test.skip package"
            }
            post {
                success {
                     archiveArtifacts 'target/.jar, target/.war'
                }

            }
        }
    }
}

