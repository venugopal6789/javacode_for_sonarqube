pipeline {                                    // 1  // Defines the start of the Jenkins pipeline block

    agent any                                 // Specifies the pipeline can run on any available agent

    environment {                             // 2  // Defines environment variables for the pipeline
        PATH = "/opt/maven/bin:$PATH"         // Adds Maven's path to the system's PATH variable
    }                                         // 2  // Ends the environment block

    stages {                                  // 3  // Defines the stages block where multiple stages are declared

        stage("build") {                      // 4  // Creates a stage named 'build'
            steps {                           // 5  // Defines the steps that will be executed in this stage
                echo "----------- build started ----------"
                                              // Logs a message indicating the start of the build
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                                              // Runs Maven clean and deploy commands, skipping tests
                echo "----------- build completed ----------"
                                              // Logs a message indicating the build completion
            }                                 // 5  // Ends the steps block for 'build' stage
        }                                     // 4  // Ends the 'build' stage

        stage("test") {                       // 6  // Creates a stage named 'test'
            steps {                           // 7  // Defines the steps that will be executed in this stage
                echo "----------- unit test started ----------"
                                              // Logs a message indicating the start of unit tests
                sh 'mvn surefire-report:report'
                                              // Runs the Maven Surefire report to execute unit tests
                echo "----------- unit test completed ----------"
                                              // Logs a message indicating unit test completion
            }                                 // 7  // Ends the steps block for 'test' stage
        }                                     // 6  // Ends the 'test' stage

        
stage('SonarQube analysis') {         // 8  // Creates a stage named 'SonarQube analysis'     ----> below is sonar qube stages
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
        }                       

stage("Quality Gate") {               // 11  // Creates a stage named 'Quality Gate'
            steps {                           // 12  // Defines the steps that will be executed in this stage
                script {                      // 13  // Allows running custom Groovy script inside the pipeline
                    timeout(time: 1, unit: 'HOURS') {  
                                              // Sets a timeout of 1 hour for the quality gate check
                        def qg = waitForQualityGate()  
                                              // Waits for the quality gate result from SonarQube
                        if (qg.status != 'OK') {  
                                              // Checks if the quality gate status is not OK
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"  
                                              // Aborts the pipeline if the quality gate fails
                        }
                    }
                }                             // 13  // Ends the script block for the Quality Gate stage
            }                                 // 12  // Ends the steps block for 'Quality Gate' stage
        }                                     // 11  // Ends the 'Quality Gate' stage
    }                                         // 3  // Ends the stages block
}                                             // 1  // Ends the pipeline block
