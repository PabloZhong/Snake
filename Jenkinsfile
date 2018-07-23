podTemplate(name: 'jnlp', label: 'jnlp', namesapce: 'default', cloud: 'kubernetes',
  containers: [
        containerTemplate(
            name: 'jnlp',
            image: 'hub.easystack.io/3dc70621b8504c98/jenkins-slave:v1', //请按需修改Jenkins Slave镜像名称
            command: '',
            args: '${computer.jnlpmac} ${computer.name}',
            privileged: true,
            alwaysPullImage: false,
            ttyEnabled: true, 
        ),
  ],
  volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock'),
            hostPathVolume(hostPath: '/usr/bin/docker', mountPath: '/usr/bin/docker'),
            hostPathVolume(hostPath: '/usr/bin/docker-current', mountPath: '/usr/bin/docker-current'),
            hostPathVolume(hostPath: '/etc/sysconfig/docker', mountPath: '/etc/sysconfig/docker'),
            hostPathVolume(hostPath: '/usr/bin/kubectl', mountPath: '/usr/bin/kubectl')]
  ) {

  node('jnlp') {
    stage('devops for snake game') {
        container('jnlp') {
            stage("Clone source code of Snake game") {
                git 'http://172.16.6.30:30080/easystack/snake-new.git'
            }
            
            stage('Unit test') {
                sh 'echo "unit test command"'
            }
            
            stage('Build docker image') {
                sh """
                    docker login -u admin -p Passw0rd hub.easystack.io
                    docker build -t hub.easystack.io/captain/snake:${BUILD_NUMBER} .
                    docker push hub.easystack.io/captain/snake:${BUILD_NUMBER}
                """
            }
            
            stage('Deploy app to EKS') {
                //请按需修改Deployment名称和Snake镜像名称
                sh """kubectl set image deployment/snake-snake-e8fluud7 snake-snake-e8fluud7=hub.easystack.io/3dc70621b8504c98/snake:${BUILD_NUMBER}"""
            }
        }
    }
 }
}
