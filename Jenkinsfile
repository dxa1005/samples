pipeline{
    agent any
    
    environment {
        dotnet ='C:\\Program Files (x86)\\dotnet\\'
        }
        
    triggers {
        pollSCM 'H * * * *'
    }
 
parameters{
  string defaultValue: description: 'Please input the Voya' application name prod', name:APP_NAME_PROD, trim: true
  string defaultValue: description: 'Please input the Voya' resource group name', name:RESOURCE_GROUP, trim: true
  string defaultValue: description: 'Please input the Voya' App service Credential ID', name:AZURE_CREDENTIAL_ID, trim: true
 }

stages{
 stage('Checkout') {
    steps {
     git credentialsId: 'Give Your Credential ID', url: 'https://github.com/YourAcc/YourRepoName.git/', branch: 'Branch on which you want to set the CI'
     }
  }


stage('Restore packages'){
   steps{
      bat "dotnet restore YourProjectPath\\Your_Project.csproj"
     }
  }


stage('Clean'){
    steps{
        bat "dotnet clean YourProjectPath\\Your_Project.csproj"
     }
   }


stage('Build'){
   steps{
      bat "dotnet build YourProjectPath\\Your_Project.csproj --configuration Release"
    }
 }

stage('Test: Unit Test'){
   steps {
     bat "dotnet test YourProjectPath\\UnitTest_Project.csproj"
     }
  }
       
 stage('Test: Integration Test'){
    steps {
       bat "dotnet test ProjectPath\\IntegrateTest_Project.csproj"
      }
   }

stage('Publish'){
     steps{
       bat "dotnet publish YourProjectPath\\Your_Project.csproj "
     }
}


        
stage('Deploy to Azure (DEV)') {
	steps {
		azureWebAppPublish azureCredentialsId: params.AZURE_CREDENTIAL_ID, 
		resourceGroup: params.RESOURCE_GROUP, 
		appName: params.APP_NAME_DEV, 
		sourceDirectory: "MyBudget/bin/Release/netcoreapp2.2/publish/"
	}
}

stage('Deploy to Azure (PROD)') {  
	steps {
		input 'Do you approve deployment to PRO?'
		
		azureWebAppPublish azureCredentialsId: params.AZURE_CREDENTIAL_ID, 
		resourceGroup: params.RESOURCE_GROUP, 
		appName: params.APP_NAME_PROD, 
		sourceDirectory: "MyBudget/bin/Release/netcoreapp2.2/publish/"
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
  
  