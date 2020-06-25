node {
   agent any

   tools {
      // Install the Maven version configured as "M3" and add it to the path.
      maven "m2_home"
   }

   stages {
      stage('Build') {
         steps {
            // Get some code from a GitHub repository
            git 'https://github.com/samplehealthcaredomain/Deployment.git'

            // Run Maven on a Unix agent.
            sh "mvn -Dmaven.test.failure.ignore=true clean package"

            // To run Maven on a Windows agent, use
            // bat "mvn -Dmaven.test.failure.ignore=true clean package"
         }

         post {
            // If Maven was able to run the tests, even if some of the test
            // failed, record the test results and archive the jar file.
            success {
               junit '**/target/surefire-reports/TEST-*.xml'
               archiveArtifacts 'target/*.war'
            }
         }
      }
      stage('Code Analysis'){
          steps{
		  script{
          tool name: 'sonarqubescanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
          }
          
          }
      }
      stage('Deploy to Nexus'){
          steps{
              script{
                  nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'Maven1Repository', packages: []
              }
          }
      }
          
   }
}

