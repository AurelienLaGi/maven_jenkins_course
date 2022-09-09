pipeline{
    agent any
    stages{
        stage("Etape 1") {
            steps {
                echo 'Debut etape 1'
                echo 'Fin step 1'
            }
            post {
                always {
                    echo 'Fin etape 1 - passage etape 2'
                }
            }
        }
        stage("Etape 2") {
            steps {
                echo 'Debut etape 2'
                echo 'Fin step 2'
            }
        }
        stage('Clone') {
         steps {
            checkout([$class: 'GitSCM',
                branches: [[name: '*/master' ]],
                extensions: scm.extensions,
                userRemoteConfigs: [[
                    url: 'https://github.com/AurelienLaGi/maven_jenkins_course.git',
                    credentialsId: '2306bde6-67e3-4161-87f7-6086863aeff7'
                ]]
            ])

            //List all directories
            sh "ls -lart ./*"
         }
      }

    }
}