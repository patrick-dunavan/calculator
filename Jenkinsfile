pipeline { 
     agent any 
    tools {
        jdk 'JDK 21'
        jdk 'JDK 17' // Use the JDK 17 configured in Jenkins
    }
         
    
     stages { 
        stage('Checkout') {
            steps {
                
                git url: 'https://github.com/patrick-dunavan/calculator.git', branch: 'main'
            }
        }        
          stage("Compile") { 
               steps { 
                    sh "chmod 775 gradlew"
                    sh "./gradlew compileJava" 
               } 
          } 
          stage("Unit test") { 
               steps { 
                    sh "./gradlew test" 
               } 
          }

        stage("Stage with input") {
            steps {
            def userInput = false
                script {
                    timeout(time: 30, unit: 'SECONDS') {
                        def userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                        echo 'userInput: ' + userInput
                    }

                    if(userInput == true) {
                        echo "Looks good"
                    } else {
                        // not do action
                        echo "Action was aborted."
                    }

                }    
            }  
        }          

          stage("Code coverage") { 
               steps { 
                    sh "./gradlew jacocoTestReport"
                    publishHTML ( target: [
                        reportDir: 'build/reports/jacoco/test/html',
                        reportFiles: 'index.html',
                        reportName: 'Code Coverage Report'
                    ])
                    sh "./gradlew jacocoTestCoverageVerification" 
               } 
          } 
//          stage("Static code analysis") { 
//               steps { 
//                    sh "./gradlew checkstyleMain" 
//               } 
//          } 
     } 
} 