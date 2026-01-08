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
            bat 'dotnet test tests\\UnitTests -c Release --no-build --verbosity normal'
          }
        }

        stage('IntegrationTests') {
          steps {
            bat 'dotnet test tests\\IntegrationTests -c Release --no-build --verbosity normal'
          }
        }

        stage('FunctionalTests') {
          steps {
            warnError(message: 'Functional tests failed') {
              bat 'dotnet test tests\\FunctionalTests -c Release --no-build --verbosity normal'
            }
          }
        }

      }
    }

    stage('Publish') {
      steps {
        bat '''
        @echo off
        echo === PUBLISH PROJECTS ===

        dotnet publish src\\Web\\Web.csproj -c Release -o C:\\publish\\aspnet\\Web --no-build
        dotnet publish src\\PublicApi\\PublicApi.csproj -c Release -o C:\\publish\\aspnet\\PublicApi --no-build
        dotnet publish src\\BlazorAdmin\\BlazorAdmin.csproj -c Release -o C:\\publish\\aspnet\\BlazorAdmin --no-build

        echo === PUBLISH DONE ===
        '''
      }
    }

    stage('Archive') {
      steps {
        dir('C:\\publish\\aspnet') {
          archiveArtifacts artifacts: '**', onlyIfSuccessful: true
        }
      }
    }
  }
}
