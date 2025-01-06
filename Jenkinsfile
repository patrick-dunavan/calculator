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
          stage("Code coverage") { 
               steps { 
                    sh "./gradlew jacocoTestReport" 
                    sh "./gradlew jacocoTestCoverageVerification" 
               } 
          } 
          stage("Static code analysis") { 
               steps { 
                    sh "./gradlew checkstyleMain" 
               } 
          } 
     } 
} 