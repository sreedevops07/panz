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
          
          docker build -t phani .
		'''
     }   
   }
}
}
