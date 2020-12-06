def label = "worker-${UUID.randomUUID().toString()}"

podTemplate(label: label, containers: [
  containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)])

 {

  node(label) {
    def app
    stage('Clone Repository') {
        /* Cloning the Repository to our Workspace */
        figlet "Checkout - development"
        checkout scm
        GIT_COMMIT_HASH = sh (script: "git log -n 1 --pretty=format:'%h'", returnStdout: true)
    }


    stage('Deploy k8s') {
      container('helm') {
        figlet 'Deployment in progress'
        sh "helm package nginx/ --app-version ${GIT_COMMIT_HASH}"
        sh "helm upgrade --install nginx nginx-* -n development"
      }
    }
  }
}
