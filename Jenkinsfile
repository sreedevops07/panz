pipeline {
  agent any
  triggers {
    pollSCM '* * * * *'
  }
  stages {
    stage('SonarQube Analysis') {
      steps {
        sh '''
	 whoami
	 echo $PATH
         dotnet sonarscanner begin /k:"sreedevops" /d:sonar.host.url=https://sonarcloud.io /d:sonar.login="45f927d889580dfb430a5e0b35a87b544ac551de"
         dotnet build panz.csproj -c Release
        dotnet sonarscanner end /d:sonar.login="45f927d889580dfb430a5e0b35a87b544ac551de"
        
        '''
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
         whoami
	 echo $access_key
         DOCKER_LOGIN_PASSWORD=$(aws ecr get-login-password  --region us-east-1)
         docker login -u AWS -p $DOCKER_LOGIN_PASSWORD https://190490853041.dkr.ecr.us-east-1.amazonaws.com
         docker build -t 190490853041.dkr.ecr.us-east-1.amazonaws.com/gang:SAMPLE-PROJECT-${BUILD_NUMBER} .
         docker push 190490853041.dkr.ecr.us-east-1.amazonaws.com/gang:SAMPLE-PROJECT-${BUILD_NUMBER}
          
	  '''
     }   
   }
    stage('ecs deploy') {
      steps {
        sh '''
          chmod +x changebuildnumber.sh
          ./changebuildnumber.sh $BUILD_NUMBER
	  sh -x ecs-auto.sh
          '''
     }    
    }
}
post {
    failure {
        mail to: 'unsolveddevops@gmail.com',
             subject: "Failed Pipeline: ${BUILD_NUMBER}",
             body: "Something is wrong with ${env.BUILD_URL}"
    }
     success {
        mail to: 'unsolveddevops@gmail.com',
             subject: "successful Pipeline:  ${env.BUILD_NUMBER}",
             body: "Your pipeline is success ${env.BUILD_URL}"
    }
}

}
