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
    //comentario
    stage('Teste') { //@id
      
      agent {
        docker {
          image 'qnib/pytest'
        }
      }

      
      post {
        always {
          junit 'test-reports/results.xml'

        }
        when {
                branch 'master'
            }

      }
      steps {
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      }
    }
    
    stage('Teste 1') {
      
      agent {
        //comentario
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
        when {
                branch 'teste'
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
       
      }
      
      
    }
  }
  post { 
        always { 
            echo 'I will always say Hello again!'
        }
    }

}

