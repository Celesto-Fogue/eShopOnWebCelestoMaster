pipeline {
  agent any
  stages {
    stage('Restore') {
      steps {
        bat 'dotnet restore eShopOnWeb.sh --verbosity normal'
      }
    }

    stage('Build') {
      steps {
        bat 'dotnet build  eShopOnWeb.sln -c Release --no-restore'
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
        bat '@echo off echo ================================ echo START DEPLOYMENT echo ================================  REM Variables set PROJECT_PATH=%WORKSPACE% set PUBLISH_PATH=%WORKSPACE%\\publish set IIS_SITE_NAME=eShopOnWeb set IIS_DEPLOY_PATH=C:\\inetpub\\wwwroot\\eShopOnWeb  REM Stop IIS site echo Stopping IIS site... %windir%\\system32\\inetsrv\\appcmd stop site "%IIS_SITE_NAME%"  REM Clean publish folder if exist "%PUBLISH_PATH%" (     rmdir /s /q "%PUBLISH_PATH%" )  REM Restore dependencies echo Restoring NuGet packages... dotnet restore "%PROJECT_PATH%"  REM Build project echo Building project... dotnet build "%PROJECT_PATH%" -c Release  REM Publish project echo Publishing project... dotnet publish "%PROJECT_PATH%" -c Release -o "%PUBLISH_PATH%"  REM Clean IIS deployment folder echo Cleaning IIS d'
      }
    }

  }
}