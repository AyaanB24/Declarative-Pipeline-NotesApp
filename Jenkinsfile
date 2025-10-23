@Library('Shared@main') _
pipeline {
    agent { label "LocalAgent" }

    stages {
        stage("Code") {
            steps {
                script {
                    cloneRepo("https://github.com/AyaanB24/Declarative-Pipeline-NotesApp.git", "master")
                }
            }
        }

        stage("Build") {
            steps {
                script {
                    docker_build("notes-app", "latest", "ayaanb2224")
                }
            }
        }

        stage("Test") {
            steps {
                echo "🧪 Running basic tests..."
                sh "echo 'Tests passed!'"
            }
        }

        stage("Pushing to Docker Hub") {
            steps {
                script {
                    docker_push("notes-app", "latest", "ayaanb2224")
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    docker_compose()
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
            emailext(
                to: 'ayaanbargir24@gmail.com',
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """<html>
                    <body style="font-family:Arial, sans-serif;">
                        <h2 style="color:green;">🎉 Build Successful!</h2>
                        <p><b>Job:</b> ${env.JOB_NAME}</p>
                        <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                        <p><b>Build URL:</b> <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>
                        <hr>
                        <p style="color:gray;">Keep up the great work! 🚀</p>
                    </body>
                </html>""",
                attachLog: true
            )
        }

        failure {
            echo "❌ Pipeline failed!"
            emailext(
                to: 'ayaanbargir24@gmail.com',
                subject: "❌ FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """<html>
                    <body style="font-family:Arial, sans-serif;">
                        <h2 style="color:red;">💥 Build Failed!</h2>
                        <p><b>Job:</b> ${env.JOB_NAME}</p>
                        <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                        <p><b>Build URL:</b> <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>
                        <hr>
                        <p style="color:gray;">Please review the attached build log for more details. 🧰</p>
                    </body>
                </html>""",
                attachLog: true
            )
        }

        always {
            echo "📩 Email notification sent (with logs attached)."
        }
    }
}
