properties([[$class: 'JiraProjectProperty'], parameters([choice(choices: ['RELEASE', 'DEVELOP'], description: '', name: 'RELEASE'), choice(choices: ['0.1.0'], description: '', name: 'RELEASE_VER')])])
pipeline{
  agent {
    label('vladriabets')
  }
  tools {
    nodejs('Node12')
  }
  stages{
    // stage('CheckoutSCM') {
    //   steps{
    //     git poll: false, url: 'https://github.com/dbielik/mdt-lab.git'
    //   }
    // }
    stage('Build') {            
      parallel {
        stage ('build css') {
          steps{
            script{
              sh """
                cd ${WORKSPACE}/www/css
                cleancss -d style.css > ../min/custom-min.css """ 
            }
          }    
        }
        stage ('build js') {
          steps {
            script{
              sh """
                cd ${WORKSPACE}/www/js
                uglifyjs --timings init.js -o ../min/custom-min.js """
            }
          }    
        }        
      }
    }
    stage ('create Archive'){
      steps {
        script { 
          sh """
            cd ${WORKSPACE}/www
            tar --exclude='./css' --exclude='./js' -c -z -f ../site-archive-${params.RELEASE}-${params.RELEASE_VER}-${BUILD_NUMBER}.tgz ."""
        }
      }
    }        
    stage('Archive when') {
      when {
        expression {
          params.RELEASE == 'RELEASE' 
        }
      }
      steps {
        archive '*.tgz'
      }
    }
  }
}
