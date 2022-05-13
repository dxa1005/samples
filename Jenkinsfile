pipeline{
    agent any
    
    environment {
        dotnet ='C:\\Program Files (x86)\\dotnet'
        }
        
 
parameters{
  string defaultValue: '', description: 'Please input the Voya application name prod', name:'APP_NAME_PROD', trim: true
  string defaultValue: '', description: 'Please input the Voya resource group name', name:'RESOURCE_GROUP', trim: true
  string defaultValue: '', description: 'Please input the Voya App service Credential ID', name:'AZURE_CREDENTIAL_ID', trim: true
 }

stages{
 stage('Checkout') {
    steps {
      git url: 'https://github.com/dxa1005/samples.git/', branch: 'main'
     }
  }


stage('Restore packages'){
   steps{
      bat "dotnet restore ./samples/aspnetcore/blazor/BinarySubmit/BinarySubmit.csproj"
     }
  }


stage('Clean'){
    steps{
        bat "dotnet clean ./samples/aspnetcore/blazor/BinarySubmit/BinarySubmit.csproj"
     }
   }


stage('Build'){
   steps{
      bat "dotnet build ./samples/aspnetcore/blazor/BinarySubmit/BinarySubmit.csproj --configuration Release"
    }
 }

stage('Test: Unit Test'){
   steps {
     bat "dotnet test ./samples/aspnetcore/blazor/BinarySubmit/BinarySubmit.csproj"
     }
  }
       
 stage('Test: Integration Test'){
    steps {
       bat "dotnet test ./samples/aspnetcore/blazor/BinarySubmit/BinarySubmit.csproj"
      }
   }

stage('Publish'){
     steps{
       bat "dotnet publish ./samples/aspnetcore/blazor/BinarySubmit/BinarySubmit.csproj "
     }
}


        
stage('Deploy to Azure (DEV)') {
	steps {
		azureWebAppPublish azureCredentialsId: params.AZURE_CREDENTIAL_ID, 
		resourceGroup: params.RESOURCE_GROUP, 
		appName: params.APP_NAME_DEV, 
		sourceDirectory: "samples/aspnetcore/blazor/BinarySubmit/"
	}
}


}

post{
  always{
    emailext body: "${currentBuild.currentResult}: Job   ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
    recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
    subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
    }
  }
 } 
  
  
