pipeline {
    agent any

    stages {
        stage('Build and Test') {
            steps {
                script {
                        bat  "./gradlew clean build"           
                }
            }
        }
	stage('SonarQube analysis 2') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner';
                    withSonarQubeEnv('sonar-scanner') {
                    bat "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=ejemplo-maven2 -Dsonar.sources=src/main/java/  -Dsonar.java.binaries=build -Dsonar.projectBaseDir=${env.WORKSPACE} -Dsonar.login=8e8236752890bf7bb18bc071593360e27a3d0346"
                    }
                }
            }
        }
        stage('Run') {
            steps {
                   bat  "start /min gradlew bootRun &"
            }
        }
	    stage('Test') {
            steps {
                   bat  'curl -X GET "http://localhost:8081/rest/mscovid/test?msg=testing"'
            }
        }
	stage('Nexus Upload') {
            steps {
		        nexusPublisher nexusInstanceId: 'nexus_test', nexusRepositoryId: 'test-repo', 
		        packages: [[$class: 'MavenPackage', 
			    mavenAssetList: [[classifier: '', 
			    extension: '', 
			    filePath: '${env.WORKSPACE}/build/DevOpsUsach2020-0.0.1.jar']], 
			    mavenCoordinate: [artifactId: 'DevOpsUsach2020', 
			    groupId: 'com.devopsusach2020', 
			    packaging: 'jar', 
			    version: '0.0.1']
			    ]]
            }
    	}
    }
}
