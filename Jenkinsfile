
pipeline {
  agent any

  stages {

    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }

    stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: "https://id.docker.com/login/?next=%2Fid%2Foauth%2Fauthorize%2F%3Fclient_id%3D43f17c5f-9ba4-4f13-853d-9d0074e349a7%26next%3D%252F%253Fref%253Dlogin%26nonce%3DeyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiI0M2YxN2M1Zi05YmE0LTRmMTMtODUzZC05ZDAwNzRlMzQ5YTciLCJleHAiOiIyMDIxLTExLTI1VDA3OjM0OjI0LjAwODM1MTIyN1oiLCJpYXQiOiIyMDIxLTExLTI1VDA3OjI5OjI0LjAwODM1MjE4NVoiLCJyZnAiOiJjdVFSWFM5YjlReVZlOWVUdDR6ZkZnPT0iLCJ0YXJnZXRfbGlua191cmkiOiIvP3JlZj1sb2dpbiJ9.Af2vPmbbgh-JT_UkZ-u0z0FYVgc-jRPxpBiB_QAdcVI%26redirect_uri%3Dhttps%253A%252F%252Fhub.docker.com%252Fsso%252Fcallback%26response_type%3Dcode%26scope%3Dopenid%26state%3DeyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiI0M2YxN2M1Zi05YmE0LTRmMTMtODUzZC05ZDAwNzRlMzQ5YTciLCJleHAiOiIyMDIxLTExLTI1VDA3OjM0OjI0LjAwODM1MTIyN1oiLCJpYXQiOiIyMDIxLTExLTI1VDA3OjI5OjI0LjAwODM1MjE4NVoiLCJyZnAiOiJjdVFSWFM5YjlReVZlOWVUdDR6ZkZnPT0iLCJ0YXJnZXRfbGlua191cmkiOiIvP3JlZj1sb2dpbiJ9.Af2vPmbbgh-JT_UkZ-u0z0FYVgc-jRPxpBiB_QAdcVI"]) {
          sh 'printenv'
          sh 'docker build -t ooeid/numeric-app:""$GIT_COMMIT"" .'
          sh 'docker push ooeid/numeric-app:""$GIT_COMMIT""'
        }
      }
    }
  }

  
}