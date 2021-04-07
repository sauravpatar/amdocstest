pipeline {
    agent any
    parameters{
      string defaultValue: '', description: 'Please provide the title to update the Index.html file', name: 'Title', trim: true
    }
    stages {
        stage('Updating User Input'){
           when { 
             expression { 
             env.Title != '';
             } 
          }
           steps {
             echo 'Updating the user inpurt to title'
             sh "sed -i 's/Page Title/${Title}/g' index.html"
           }
        }
        stage('Updating Build Info'){
          steps {
            echo 'Updating build number'
            sh "sed -i '\\|</body>|i <p>The Build Number is ${BUILD_NUMBER}</p>' index.html"
            sh "sed -i '\\|</body>|i <p>The JOB NAME is ${JOB_NAME}</p>' index.html"
            sh "sed -i '\\|</body>|i <p>The BUILD ID is ${BUILD_ID}</p>' index.html"
          }
        }
        stage('Building images') {
            steps {
                echo 'Building..'
                sh "sudo docker build -t=amdocs-html:latest ."
            }
        }
        stage('Deploying the changes'){
          steps{
            echo 'Changes'
            sh "sudo docker-compose down -v"
            sh "sudo docker-compose up -d"
          }

        }
    }
    post { 
        always { 
            cleanWs()
        }
    }
}
