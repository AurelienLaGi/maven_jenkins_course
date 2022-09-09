pipeline {

   agent any

   tools{
      maven 'Premier_maven'
   }

   stages {
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
	  stage('Compile'){
         steps{
            withMaven(maven:'Premier_maven')
            {
              sh "mvn compile"
            }
         }
      }
      stage('Test'){
         steps{
            withMaven(maven:'Premier_maven')
            {
              sh "mvn test"
            }
         }
         
      }
      
      stage('Build'){
         steps{
            sh "mvn -B -DskipTests clean install"
         }
      }
      stage ('SonarQube analysis') {
         steps {
            withSonarQubeEnv(installationName: 'My local Sonar', credentialsId: '2306bde6-67e3-4161-87f7-6086863aeff7') {
               sh 'mvn -B -DskipTests clean package sonar:sonar -Dsonar.login=$Login -Dsonar.password=$Password'
            }
         }
      }
      
      
	stage('Code Coverage') {
        steps {
            sh 'mvn clean cobertura:cobertura'
        }
        
        post {
	        always {
	            step([$class: 'CoberturaPublisher', autoUpdateHealth: true, autoUpdateStability: true, coberturaReportFile: '**/target/site/cobertura/*.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 2, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: true])
	        }
	    }
    }

   }
}