currentBuild.displayName = "Final_Demo # "+currentBuild.number

   def getDockerTag(){
        def tag = sh script: 'git rev-parse HEAD', returnStdout: true
        return tag
        }

pipeline{
	   agent { label 'node01' }
//            tools{

//                maven '3.9.0'
//              } 
        environment{
	    Docker_tag = getDockerTag()
        }
        
        stages{


              stage('Quality Gate Statuc Check'){

               agent {
		 
                docker {
	        label 'node01'
                image 'maven'
                args '-v $HOME/.m2:/root/.m2'
                }
          }
                  steps{
                      script{
                      withSonarQubeEnv('sonar1') { 
                      sh "mvn sonar:sonar"
                       }
                      timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
		    sh "mvn clean install"
                  }
                }  
              }




              
	       
	       
        }	       
	      
    
}
