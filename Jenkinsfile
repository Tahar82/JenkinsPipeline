pipeline {
    agent any
    parameters {
        string(name : 'Git_URL', description: 'The git repository to use')
        string(name: 'Branch', defaultValue: 'main', description: 'Branch to use')
        choice(name: 'Java_Version', choices:['17','11','20','21','22'], description: 'Java version to use')

    }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
        jdk "jdk-${params.Java_Version}"

    }

    stages {
        stage('GIT') {
            steps {
                // Get some code from a GitHub repository
                git url :  "${params.Git_URL}",
                    branch: "${params.Branch}"
                    sh "sed -i -e 's@<maven.compiler.target>.*</maven.compiler.target>@<maven.compiler.target>${params.Java_Version}</maven.compiler.target>@' -e 's@<maven.compiler.source>.*</maven.compiler.source>@<maven.compiler.source>${params.Java_Version}</maven.compiler.source>@' pom.xml"
        }
        }
        stage('COMPILE'){
            steps{
                sh "mvn clean compile"
            }
        }
        stage('TEST'){
            steps{
                sh "mvn test"
            }
            post{
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
        }
        stage('PACKAGE'){
            steps{
                sh "mvn test -DskipTests -Dmaven.test.skip package"
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar,target/*.war'
                }

            }

        }
    }
}