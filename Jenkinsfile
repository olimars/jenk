pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        docker {
          image 'python:2-alpine'
        }

      }
      steps {
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
      }
    }
     failFast true
    parallel {
    stage('Teste') {
      when {
                branch 'master'
            }
      agent {
        docker {
          image 'qnib/pytest'
        }

      }
      post {
        always {
          junit 'test-reports/results.xml'

        }

      }
      steps {
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      }
    }
    
    stage('Teste 1') {
      when {
                branch 'teste'
            }
      agent {
        docker {
          image 'qnib/pytest'
        }

      }
      post {
        always {
          junit 'test-reports/results.xml'

        }

      }
      steps {
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calcs.py'
      }
    }
    }
    stage('Entrega') {
      agent {
        docker {
          image 'cdrx/pyinstaller-linux:python2'
        }
        
        

      }
      post {
        success {
          archiveArtifacts 'dist/add2vals'

        }

      }
      steps {
        sh 'pyinstaller --onefile sources/add2vals.py'
        input "Podemos fazer entrega?"
      }
      
      
    }
  }
}
