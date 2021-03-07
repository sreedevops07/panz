pipeline {
  agent any
  triggers {
    pollSCM '* * * * *'
  }
  stages {
    stage('Dotnet Restore') {
      steps {
        sh 'dotnet restore panz.csproj'
      }
    }
    stage('Dotnet Build') {
      steps {
        sh 'dotnet build panz.csproj -c Release' 
     }   
    }
    stage('Dotnet Publish') {
      steps {
        sh 'dotnet publish panz.csproj -c Release'
      }   
    }
   stage('Docker build and push') {
      steps {
        sh '''
          REPOSITORY_URI=971076122335.dkr.ecr.ap-south-1.amazonaws.com/sample
          DOCKER_LOGIN_PASSWORD=$(aws ecr get-login-password  --region ap-south-1)
          docker login -u AWS -p $DOCKER_LOGIN_PASSWORD https://971076122335.dkr.ecr.ap-south-1.amazonaws.com
          docker build -t $REPOSITORY_URI:PRUDHVI-PROJECT-${BUILD_NUMBER} .
          docker push $REPOSITORY_URI:PRUDHVI-PROJECT-${BUILD_NUMBER}
          
	  '''
     }   
   }
}
}
