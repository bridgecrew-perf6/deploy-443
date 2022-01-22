pipeline {
	agent any 
	stages {
    stage(‘Update’) {
	steps {
        sh "sudo rm -rf webserver/ "
	}
	}
	stage (‘install’) {
	steps {
		sh "sudo yum install npm -y"
		sh "sudo npm install -g create-react-app@3.4.1"
	}
	}
    stage (‘Initialization’) {
	steps {
		sh "npm init react-app webserver --use-npm"
	}
    	}
    stage (‘Build’) {
	steps {
		sh "cd webserver && npm run build"
		sh "sudo cp /home/centos/project/* /var/lib/jenkins/workspace/SSR/webserver/"
	}
	}

        stage (‘Deploy’) {
	steps {
		sh "docker-compose -f docker-compose.yaml up -d --build"

        }
	}
}
}

