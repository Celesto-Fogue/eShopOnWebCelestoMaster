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
            warnError(message: 'Functional') {
              bat 'dotnet test tests/FunctionalTests -c Release --no-build --verbosity normal'
            }

          }
        }

      }
    }

    stage('Deployment') {
      steps {
        bat '@echo off echo === DEPLOYMENT DES PROJETS INDIVIDUELS ===  echo Publication de Web... dotnet publish src\\Web\\Web.csproj -c Release -o "C:\\publish\\aspnet\\Web" --no-build  echo Publication de PublicApi... dotnet publish src\\PublicApi\\PublicApi.csproj -c Release -o "C:\\publish\\aspnet\\PublicApi" --no-build  echo Publication de BlazorAdmin... dotnet publish src\\BlazorAdmin\\BlazorAdmin.csproj -c Release -o "C:\\publish\\aspnet\\BlazorAdmin" --no-build  echo === DEPLOYMENT TERMINEE ==='
        dir(path: 'C:\\publish\\aspnet') {
          archiveArtifacts(artifacts: '*', onlyIfSuccessful: true)
        }

      }
    }

  }
}