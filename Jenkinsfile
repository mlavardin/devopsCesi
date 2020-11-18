pipeline {
  agent any
  stages {
    stage('Create Dockerfile') {
      steps {
        sh '''# Creation du Dockerfile
touch Dockerfile

# Renseignement du Dockerfile
echo "FROM nginx:alpine
COPY index.html /usr/share/nginx/html
" >> Dockerfile

echo "Check dockerfile"
cat Dockerfile

echo ""
echo "Check index.html"
cat index.html'''
      }
    }

    stage('Docker image') {
      steps {
        sh '''# Build Docker Image 
docker build -t mysiteweb:latest .
'''
        sh '''# Del of old image
docker rmi 172.17.0.3:5000/mysiteweb:latest

# Upload to the local registry
docker tag mysiteweb:latest 172.17.0.3:5000/mysiteweb:latest 
docker push 172.17.0.3:5000/mysiteweb:latest '''
      }
    }

    stage('Launch Web Site') {
      steps {
        sh '''# Run docker website
docker run --name mywebsite -d -p 80:80 172.17.0.3:5000/mysiteweb:latest'''
      }
    }

  }
}
