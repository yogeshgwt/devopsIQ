pipeline {
    agent {
        node {
            label 'slave1'
        }
    }
    parameters {
    	string(name: 'serverIP', defaultValue: 'None', description: 'Enter Server IP ')
    }
    stages {
        stage('SCM checkout'){
            steps {
		git "https://github.com/vistasunil/devopsIQ.git"
            }
	}
	stage('Remove dockers'){
	    steps {
		sh "if [ `sudo docker ps -a -q|wc -l` -gt 0 ]; then sudo docker rm -f \$(sudo docker ps -a -q);fi"
		}
	}
	stage('Build'){
	    steps {
    		sh "sudo docker build /home/ubuntu/jenkins/workspace/ansiblejob -t vistasunil/devopsdemo"
	   }
	}
	stage('Docker Push'){
		steps {
	            sh "cat /home/ubuntu/docker_login.txt |sudo docker login --username vistasunil --password-stdin"
                    sh "sudo docker push vistasunil/devopsdemo:latest"
	        }
	}
	stage('Configure servers with Docker and deploy website') {
            	steps {
                	sh 'ansible-playbook docker.yaml'
            	}
        }
	stage('Install Chrome browser') {
            	steps {
                	sh 'ansible-playbook chrome.yaml'
            	}
        }
	stage ('Testing'){
		steps {
			sh "sudo apt install python3-pip -y"
			sh "pip3 install selenium"
			sh "python3 selenium_test.py ${params.serverIP}"
		}
	}
    }
}
