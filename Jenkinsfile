pipeline {
  agent { label 'workstation' }

   options {
     ansiColor('xterm')
   }

  stages {

    stage('Code Quality'){
      when {
        allOf {
        expression { env.TAG_NAME != env.GIT_BRANCH }
        }
      }
      steps {
        //sh 'sonar-scanner -Dsonar.host.url=http://172.31.41.26:9000 -Dsonar.login=admin -Dsonar.password=admin123 -Dsonar.projectKey=frontend -Dsonar.qualitygate.wait=true'
        echo 'OK'
      }
    }

    stage('Unit Tests'){
      when {
        allOf {
        expression { env.TAG_NAME != env.GIT_BRANCH }
        branch 'main'
        }
      }
      steps {
      // Ideally we should run Unit Test , but in this practice we skipped it. Below is the command to run the unit test
      // sh 'npm test'
        echo 'CI'
      }
    }

    stage('Release') {
      when {
        expression { env.TAG_NAME ==~ ".*" }
      }
      steps {
        sh 'zip -r frontend-${TAG_NAME}.zip static asset-manifest.json index.html robots.txt'
        sh 'curl -sSf -u "admin:Admin123" -X PUT -T frontend-${TAG_NAME}.zip "http://artifactory.yamunadevops.online:8081/artifactory/frontend/frontend-${TAG_NAME}.zip"'
      }
    }

  }
}
//