// @Library('shared_libraries_for_pipelines')_

def getOS(){
	String osname = System.getProperty('os.name');
	if (osname.startsWith('Windows')) {
		bat "echo 'windows'";
		return 'windows';
	}
	else if (osname.startsWith('Mac'))
		return 'macosx';
	else if (osname.contains('nux'))
		return 'linux';
	else
		throw new Exception("Unsupported os: ${osname}");
}

pipeline {	
    agent {label 'slave_1'}
	
	libraries {
		lib('shared_libraries_for_pipelines')
	}
	
	triggers {
        cron('H/2 * * * *')
    }
	
	tools {
        // maven "C:\Users\Sougata Ghosh\Development software sources or binaries\apache-maven-3.8.1" 
		maven 'apache-maven-3.8.1'
	}
	
	environment { 
        OS = getOS()
    }
	
	options {
        timeout(time: 5, unit: 'MINUTES') 
    }
	
	parameters {
        string(name: 'PERSON', defaultValue: 'Sougata Ghosh', description: 'night owl')
		
		// string(name: 'OPSYS', defaultValue: '$OS', description: 'the OS')

        text(name: 'BIOGRAPHY', defaultValue: 'This is my biography', description: 'some biography...')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle switch')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }

    stages {
	
		stage('shared_libraries'){
			steps{
				sayHello 'Ghosh'
				echo 'The value of batman is : ' + GlobalVars.batman
				script {
                    def person = new SampleClass()
                    person.age = 13
                    person.increaseAge(10)
                    echo 'Incremented age, is now : ' + person.age
                }
			}
		}
	
		stage('init'){
			steps{
				echo "Hello ${params.PERSON}"
				// echo "the OS: ${params.OPSYS}"
				echo "Biography: ${params.BIOGRAPHY}"
				echo "Toggle: ${params.TOGGLE}"
				echo "Choice: ${params.CHOICE}"
				echo "Password: ${params.PASSWORD}"
			}
		}
        
        stage('clone'){
            steps{
                git branch: 'master', url: 'https://github.com/SougataGhosh314/spring_security_jpa.git'
            }
        }
        
        stage('build'){
            steps{
                parallel (
                    "Taskone" : {
                        echo 'Taskone'
                    },
                    "Tasktwo" : {
                        echo 'Taskone'
                    }
                )
            }
           
        }
        
        stage('test & QA'){
            steps{
			
				script {
					def browsers = ['chrome', 'firefox', 'opera', 'edge', 'safari']
                    for (int i = 0; i < browsers.size(); ++i) {
                        echo "Testing the ${browsers[i]} browser"
                    }
				}
				
                echo 'test phase'
			    // bat 'echo "Operating system is: $OS"'
				// echo '$OS'
            }
            
        }
        
        stage('deploy'){
		
			// when {
            //     branch 'master'
            //     environment name: 'DEPLOY_TO', value: 'master deployments'
            // }
			
            steps{
                script{
                    try {
                        bat 'mvn clean'
                        bat 'mvn install'
                    }catch (exc) {
                        echo 'Something didn\'t work and got some exceptions'
                    }
                }
            }
            
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    echo 'success'
                    // junit '**/target/surefire-reports/TEST-*.xml'
                    // // archiveArtifacts 'target/*.jar'
                    // archiveArtifacts 'target/*.war'
                }
				
				always { 
					echo 'This runs always, no matter the state of the build'
				}
            }
        }

    }
}
