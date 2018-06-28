node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/jeromenaylies/training-app'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'MAVEN 3'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -DskipTests -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -B -DskipTests -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Execute Jar') {
       if (isUnix()) {
           sh "java -jar target/*.jar"
       } else {
           bat(/java -jar target\/training-app-1.2-jar-with-dependencies.jar/)
       }
   }
   stage('Test') {
       if (isUnix()) {
           sh "'${mvnHome}/bin/mvn ' test"
       } else {
           bat(/"${mvnHome}\bin\mvn" test/)
       }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
   }
   stage('Publish Artefact') {
       if (isUnix()) {
           sh "'${mvnHome}/bin/mvn' -DdeployOnly deploy"
       } else {
            bat(/"${mvnHome}\bin\mvn" -DdeployOnly deploy -s "D:\integ_continue\apache-maven-3.5.3\conf\settings.xml"/)
       }
   }
}