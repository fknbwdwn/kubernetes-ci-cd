node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName
    slackSend channel: "#jenkins-test", message: "Build started: ${env.JOB_NAME} ${env.BUILD_NUMBER}"

    stage "Build"
    
        sh "docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
    stage "Push"

        sh "docker push ${imageName}"
    
    stage "Smoke Test"
    
        sh "echo 'some bullshit here'"
        slackSend channel: "#jenkins-test", message: "Deployment pending approval: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        input "Deploy to prod?"

    stage "Deploy"

        sh "sed 's#127.0.0.1:30400/hello-kenzan:latest#'$BUILDIMG'#' applications/hello-kenzan/k8s/deployment.yaml | kubectl apply -f -"
        sh "kubectl rollout status deployment/hello-kenzan"
        slackSend channel: "#jenkins-test", message: "Build deployed: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
}
