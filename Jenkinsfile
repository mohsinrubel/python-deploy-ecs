
pipeline {
    agent any

    stages {
        stage('Check Connection') {
                
            steps {
                script{

                        sshagent(['ca00f31c-69a2-4ef5-b05f-b56f060f337f']) {
                            sh '''
                            ssh ${env.SSH_MACHINE} "echo "server running....""
                            '''
               
                        }
              
                    }
                   
                }

            
        }

        stage('update code') {
            steps {
                script{

                       sshagent(['ca00f31c-69a2-4ef5-b05f-b56f060f337f']) {
                            sh '''
                            ssh ${env.SSH_MACHINE} "
                            cd ${env.PROJECT_DIRECTORY}  &&
                            sudo git reset .  && 
                            sudo git clean -df &&
                            sudo git stash &&
                            sudo git pull
                            
                            "
                            '''
               
                        }
              
                    }
                   
                }

       
        }
 

         stage('login ecr') {
            steps {
                script{

                       sshagent(['ca00f31c-69a2-4ef5-b05f-b56f060f337f']) {
                            sh '''
                            ssh ${env.SSH_MACHINE} "
                                    cd ${env.PROJECT_DIRECTORY} &&
                                    sudo docker login -u AWS -p $(aws ecr get-login-password --region ${env.REGION}) ${env.ECR_URL}
                            
                            "
                            '''
               
                        }
              
                    }
                   
                }
        }

        
          stage('build code') {
            steps {
                script{

                       sshagent(['ca00f31c-69a2-4ef5-b05f-b56f060f337f']) {
                            sh '''
                            ssh ${env.SSH_MACHINE} "
                                    cd ${env.PROJECT_DIRECTORY} &&
                                    sudo docker build -t ${env.ECR_URL}/python-todo:${env.BUILD_NUMBER} .
                            
                            "
                            '''
               
                        }
              
                    }
                   
                }

        }


         stage('push code') {
            steps {
                script{

                       sshagent(['ca00f31c-69a2-4ef5-b05f-b56f060f337f']) {
                            sh '''
                            ssh ${env.SSH_MACHINE} "
                                    cd ${env.PROJECT_DIRECTORY} &&
                                    sudo docker push ${env.ECR_URL}/python-todo:${env.BUILD_NUMBER} .
                            
                            "
                            '''
               
                        }
              
                    }
                   
                }
        } 
    }

}

    