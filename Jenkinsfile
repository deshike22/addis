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

podTemplate(name: 'kaniko', label: label, yaml: """
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: ifNotPresent
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /root
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: regcred
          items:
            - key: .dockerconfigjson
              path: .docker/config.json
"""
  ) 
podTemplate(name: 'frontend', label: 'myapp', yaml: """
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: bimehta/addis
        ports:
        - containerPort: 80
"""
  )
{

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
    stage('Deploy Container in Dev'){
      container(name: 'frontend')
    }
  }
}
