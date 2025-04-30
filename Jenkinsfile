pipeline {
  agent any
  stages {
    stage('Restore') {
      steps {
        bat 'dotnet restore eShopOnWeb.sln --verbosity normal'
      }
    }

    stage('Build') {
      steps {
        bat 'dotnet build eShopOnWeb.sln -c Release --no-restore'
      }
    }

    stage('Tests') {
      parallel {
        stage('UnitTests') {
          steps {
            bat 'dotnet test tests/UnitTests -c Release --no-build --verbosity normal'
          }
        }

        stage('IntegrationTests') {
          steps {
            bat 'dotnet test tests/IntegrationTests -c Release --no-build --verbosity normal'
          }
        }

        stage('FunctionalTests') {
          steps {
            bat 'dotnet test tests/FunctionalTests -c Release --no-build --verbosity normal'
          }
        }

      }
    }

    stage('Deployment') {
      steps {
        bat 'dotnet publish eShopOnWeb.sln -c Release -o "C:\\publish\\aspnet" --no-build'
      }
    }

  }
}