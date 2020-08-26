pipeline {
  agent  {
    label 'master'
  }
  tools {
      nodejs "NodeJS 10.21"
  }
  environment {
    VERSION="0.1"
    APP_NAME="CLI Demo"
    PATH="$PATH:$WORKSPACE/tools/attest/v2.11.2"
    rtServer = ''
    downloadSpec = """{
        "files": [
            {
                "pattern": "devtools-npm-qa/%40axe-devtools/cli/-/%40axe-devtools/cli-2.11.2-d2fba777.0.tgz!/package/pkgs/%40axe-devtools/cli-linux",
                "target": "tools/"
            }
        ]
    }"""
  }
  stages {
    stage('Get binaries') {
      steps {
          withCredentials([usernamePassword(credentialsId:'artifactory', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            script {
                rtServer = Artifactory.newServer url: 'https://agora.dequecloud.com/artifactory/', username: USERNAME, password: PASSWORD
                rtServer.download spec: downloadSpec
            }
          }
      }
    }
    stage("use the command line tools") {
        steps {
            sh "axe broken--deque-workshop.netlify.app dequeuniversity.com/demo/mars/ --headless --browser=chrome --dir=./scan-results"
            sh "axe reporter ./scan-results --dest ./reports --format xml"
        }
    }
  }
  post {
    always {
        junit '**/reports/*.xml'
    }
  }
}
