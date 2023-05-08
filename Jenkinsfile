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
            sh "docker login -u ankeetchauhan505 -p Ankit@123"
            sh "docker tag docker-pipeline:latest ankeetchauhan505/ankeetchauhan505/docker-pipeline:latest"
            sh "docker push ankeetchauhan505/ankeetchauhan505/docker-pipeline:latest"
            echo "Image push complete"
        }
    }

    stage('Run App'){
        /*sh "docker rm $containerName -f"
        sh "docker pull $dockerHubUser/$containerName"
        sh "docker run -d --rm -p $httpPort:$httpPort --name $containerName $dockerHubUser/$containerName:$tag"
        echo "Application started on port: ${httpPort} (http)"
        */
        sh """
           kubectl get pods
           kubectl delete deployment kubernetes-bootcamp | true
           kubectl create deployment kubernetes-bootcamp --image=docker.io/anujsharma1990/docker-pipeline --port=8090
           kubectl get pods
        """
    }

}
