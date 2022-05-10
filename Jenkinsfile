pipeline {
  agent any

  environment {
    SS_POC_PROJECT_NAME = "SS-DEMO-insecure-bank"
    SS_POC_PROJECT_VERSION = "1.0"
    POLARIS_ACCESS_TOKEN = credentials('sipse')
  }

  stages {
    stage('Build') {
      steps {
        sh 'mvn -e clean package -DskipTests'
      }
    }
    stage('SAST - Coverity') {
      steps {
        echo "Running Coverity on Polaris"
        sh '''
          rm -fr /tmp/polaris
          wget -q ${POLARIS_SERVER_URL}/api/tools/polaris_cli-linux64.zip
          unzip -j polaris_cli-linux64.zip -d /tmp
          /tmp/polaris --persist-config --co project.name="${SS_POC_PROJECT_NAME}" --co project.branch="main" --co capture.build.buildCommands="null" --co capture.build.cleanCommands="null" --co capture.fileSystem="null" --co capture.coverity.autoCapture="enable"  configure
          /tmp/polaris analyze -w
        '''
      }
    }
    
    stage('Clean Workspace') {
      steps {
        cleanWs()
      }
    }
  }
}
