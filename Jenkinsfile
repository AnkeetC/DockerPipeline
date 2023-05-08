def containerName="docker-pipeline"
def tag="latest"
def dockerHubUser="ankeetchauhan505"
def httpPort="8090"

node {

    stage('Checkout') {
        checkout scm
    }

    stage('Build'){
        sh "mvn clean install"
    }

    stage("Image Prune"){
         sh "docker image prune -a -f"
    }

    stage('Image Build'){
        sh "docker build -t ankeetchauhan505/docker-pipeline:latest  -t ankeetchauhan505/docker-pipeline --pull --no-cache ."
        echo "Image build complete"
    }

    stage('Push to Docker Registry'){
        withCredentials([usernamePassword(credentialsId: 'dockerHubAccount', usernameVariable: 'dockerUser', passwordVariable: 'dockerPassword')]) {
            sh "docker login -u $dockerUser -p $dockerPassword"
            sh "docker push $dockerUser/$containerName:$tag"
            echo "Image push complete"
        }
    }

   node('KubernetesMaster'){
		stage('Run App'){
			sh """
			   sudo kubectl get pods
			   sudo kubectl delete deployment kubernetes-bootcamp | true
			   sudo kubectl create deployment kubernetes-bootcamp --image=docker.io/ankeetchauhan505/docker-pipeline --port=8090
			   sudo kubectl get pods
			"""

    }

}
