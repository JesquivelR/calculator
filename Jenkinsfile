pipeline {
     agent any
     stages {
          stage("Compile") {
               steps {
                    sh "./gradlew compileJava"
               }
          }
          stage("Unit test") {
               steps {
                    sh "./gradlew test"
               }
          }
          stage("Package") {
               steps {
                    sh "./gradlew build"
               }
          }

          stage("Docker build") {
               steps {
                    sh "docker build -t jesquivel/calculator ."
               }
          }
          stage("Docker push") {
               steps {
                    sh "docker push jesquivel/calculator"
               }
          }
          
          stage("Deploy to staging") { 
               steps { 
                    sh "docker run -d --rm -p 8765:8081 --name calculator jesquivel/calculator" 
               } 
          }
          stage("Acceptance test") { 
               steps { 
                    sleep 60 
                    sh "./gradlew acceptanceTest -Dcalculator.url=http://localhost:8765"
               } 
          }
     }
     post { 
          always { 
               sh "docker stop calculator" 
          } 
     }
}
