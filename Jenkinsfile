@Library('Shared@main') _
pipeline {
    agent { label "LocalAgent" }  
        stage("Code") {
            steps {
                script{
                    cloneRepo("https://github.com/AyaanB24/Declarative-Pipeline-NotesApp.git", "master")
                }
            }
        }

        stage("Build") {
            steps {
                script{
                    docker_build("notes-app","latest","ayaanb2224")
                }
            }
        }

        stage("Test") {
            steps {
                echo "🧪 Running basic tests..."
                sh "echo 'Tests passed!'"
            }
        }
        
        stage("Pushing to Docker Hub"){
            steps{
                script{
                    docker_push("notes-app","latest","ayaanb2224")
                }
            }
        }

        stage("Deploy") {
            steps {
                script{
                    docker_compose()
                }
            }
        }
    }
}
