pipeline {

    agent any

    tools { 

        maven 'Maven' 

        jdk 'Jdk8' 

    }

    stages {

       

            

        stage('Code Checkout') {

            steps {

             

            git credentialsId: 'Github-Veena', url: 'https://github.com/VeenaMarlla/spring-petclinic.git'

        

            }

            

        }

        

       

        stage('Build') {

           

            steps {

               

               echo "Building App"

                sh "mvn -version"

                sh "mvn clean install -f pom.xml"

                

            }

            post {

                success {

                    junit 'target/surefire-reports/**/*.xml' 

                }

            }

        }

       

       stage('Code Analysis') {

           steps {

              withSonarQubeEnv('My Sonarqube Server') {

                sh 'mvn sonar:sonar'

              }

            }

           

       }

       stage("Quality Gate") {

            steps {

              timeout(time: 1, unit: 'HOURS') {

                waitForQualityGate abortPipeline: true

              }

            }

          }

          

       stage("Build Docker image") {

           steps {

               sh "docker build -t petclinic:1.0 . -f Dockerfile.txt"

               sh " docker images"

           }

       }

       

       stage("Push Image to Image Registry") {

           steps {

               sh "docker tag petclinic:1.0 veenam/petclinic:1.0"

               sh " docker login -u veenam -p veena123"

               sh "docker push veenam/petclinic:1.0"

           }

       }

       

       stage("Pull Image and Deploy to Container") {

           steps {

               

                sh "docker stop petclinic || true && docker rm petclinic || true"



               sh "docker run -d -p 7070:8080 --name petclinic veenam/petclinic:1.0"

               

           }

       }

       

     

        }

        

    

}

