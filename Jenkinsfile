pipeline {
  agent any
  environment {
    CI = 'true'
  }
  stages {
    stage('Build') {
      steps {
        echo "build"
      }
    }
    stage('Test') {
      steps {
        echo "test"
      }
    }
    stage('Cleanup') {
      steps {
        echo "Builld & Test Cleanup"
      }
    }
    stage('Master-Deploy') {
      when {
        expression { env.BRANCH_NAME == 'master' }
      }
      steps {
        sh 'sshpass -p $OPENROOMPASSWORD scp -r -oStrictHostKeyChecking=no $WORKSPACE/* openroom@$SERVER:$OPENROOMLOCATION/'
        sh 'sshpass -p $OPENROOMPASSWORD ssh -oStrictHostKeyChecking=no openroom@$SERVER "(cd $OPENROOMLOCATION/ && docker-compose down)"'
        sh 'sshpass -p $OPENROOMPASSWORD ssh -oStrictHostKeyChecking=no openroom@$SERVER "(cd $OPENROOMLOCATION/ && docker-compose build && docker-compose up -d)"'
      }
    }
  }
}
