pipeline {                                    // 1  // Defines the start of the Jenkins pipeline block

    agent any                                 // Specifies the pipeline can run on any available agent

    environment {                             // 2  // Defines environment variables for the pipeline
        PATH = "/opt/maven/bin:$PATH"         // Adds Maven's path to the system's PATH variable
    }                                         // 2  // Ends the environment block

    stages {                                  // 3  // Defines the stages block where multiple stages are declared
        
        stage("build") {                      // 4  // Creates a stage named 'build'
            steps {                           // 5  // Defines the steps that will be executed in this stage

                sh 'mvn clean deploy' 
                                              // Runs Maven clean and deploy to build , if we give deploy build also will happen

            }                                 // 5  // Ends the steps block for 'build' stage
        }                        

        stage('SonarQube analysis') {         // 8  // Creates a stage named 'SonarQube analysis'
            environment {                     // 9  // Defines environment variables specific to this stage
                scannerHome = tool 'venu-sonar-scanner'  
                                              // Sets the SonarQube scanner tool, this we given in manage jenkins > Tools, same will give here
            }                                 // 9  // Ends the environment block for this stage

            steps {                           // 10  // Defines the steps that will be executed in this stage
                withSonarQubeEnv('venu-sonarqube-server') {
                                              // Executes the SonarQube analysis within the SonarQube environment
                    sh "${scannerHome}/bin/sonar-scanner"  
                                              // Runs the SonarQube scanner tool, this we given manage jenkins > syatem , same will give here
                }                             // Ends the withSonarQubeEnv block
            }                                 // 10  // Ends the steps block for 'SonarQube analysis' stage
        }                                     // 8  // Ends the 'SonarQube analysis' stage
     }
}


