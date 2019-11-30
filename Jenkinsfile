pipeline {
  agent any
  stages {
    stage('StageTest1'){
      steps {
        sh 'echo "Jenkins StageTest1"' 
      }
    }
    stage('StageTest2') {
      steps { 
        sh 'echo "Jenkins StageTest2"'
        sh 'cat  nonexistingFile'
      }
    }
    stage('StageTest3') {
      steps { 
        sh 'cat "Jenkins StageTest3"'
      }
    }
  }
}
