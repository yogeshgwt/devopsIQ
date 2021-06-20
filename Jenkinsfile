pipeline {
    agent {
        node {
            label 'slave1'
        }
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
    		sh "sudo docker build /home/ubuntu/workspace/an1 -t vistasunil/devopsdemo"
	   }
	}
	stage('Docker Push'){
		steps {
	            sh "cat /home/ubuntu/docker_login.txt |sudo docker login --username vistasunil --password-stdin"
                    sh "sudo docker push vistasunil/devopsdemo:latest"
	        }
	}
	stage('Configure servers and Deploy') {
            steps {
                sh 'ansible-playbook docker.yaml'
            }
        }
    }
}
