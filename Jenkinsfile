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
	}
	}
     stage (‘pakage dockerfile’) {
	steps {
		sh "cat > Dockerfile.prod <<EOF
                FROM node:13.12.0-alpine as build
                WORKDIR /app
                ENV PATH /app/node_modules/.bin:$PATH
                COPY package.json ./
                COPY package-lock.json ./
                COPY . ./

                FROM nginx:stable-alpine
                COPY --from=build /app/build /usr/share/nginx/html
                EXPOSE 80
                CMD ["nginx", "-g", "daemon off;"]

                EOF"
	}
	}
      stage (‘Package docker compose’) {
	steps {
		sh "cat > docker-compose.yaml <<EOF
                version: '3.7'
                
                services:
                  webserver-prod:
                    container_name: webserver-prod
                    build:
                      context: .
                      dockerfile: Dockerfile.prod
                    ports:
                      - '80:80'
                
                EOF"
        }
	}

        stage (‘Deploy’) {
	steps {
		sh "docker-compose -f docker-compose.yaml up -d --build"

        }
	}
}
}

