node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = "latest"
    appName = "config-service"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Compile"

       sh "./mvnw clean package"

    stage "Build"
    
        sh "docker build -t ${imageName} -f Dockerfile ."
    
    stage "Push"

        sh "docker push ${imageName}"

    stage "Deploy"

        kubernetesDeploy configs: "deploy/*.yml", kubeconfigId: 'minikube_kubeconfig', enableConfigSubstitution: true

}
