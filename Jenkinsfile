/**
 * This pipeline will build and deploy a Docker image with Kaniko
 * https://github.com/GoogleContainerTools/kaniko
 * without needing a Docker host
 *
 * You need to create a jenkins-docker-cfg secret with your docker config
 * as described in
 * https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-in-the-cluster-that-holds-your-authorization-token
 */

def label = "worker-${UUID.randomUUID().toString()}"

podTemplate(name: 'kaniko', label: label, yamlFile: 'kaniko.yaml'
  ) {

  node(label) {
    stage('Build with Kaniko') {
      git branch: 'Development', url:'https://github.com/deshike22/addis.git'
      container(name: 'kaniko', shell: '/busybox/sh') {
        withEnv(['PATH+EXTRA=/busybox']) {
          sh '''#!/busybox/sh
          /kaniko/executor -f `pwd`/Dockerfile -c `pwd` --skip-tls-verify --destination=bimehta/addis:latest
          '''
        }
      }
    }
  }
}
