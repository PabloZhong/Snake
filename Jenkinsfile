podTemplate(name: 'jnlp', label: 'jnlp', namesapce: 'default', cloud: 'kubernetes',
  containers: [
        containerTemplate(
            name: 'jnlp',
            //请按需修改Jenkins Slave镜像名称
            image: 'hub.easystack.io/3dc70621b8504c98/jenkins-slave:v1',
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
    stage('CICD for Snake Game demo') {
        container('jnlp') {
            stage("Clone source code of Snake game") {
                //请按需修改Git源代码库地址
                git 'http://172.16.6.30:30080/easystack/snake-demo.git'
            }
                      
            stage('Build & push docker image') {
                //请按需修改镜像仓库的账号和密码
                sh """
                    docker login -u 3dc70621b8504c98 -p Tcdf4f05247d79dd7 hub.easystack.io  
                    docker build -t hub.easystack.io/3dc70621b8504c98/snake:${BUILD_NUMBER} . 
                    docker push hub.easystack.io/3dc70621b8504c98/snake:${BUILD_NUMBER}
                """
            }
            
            stage('Deploy app to EKS') {
                //请按需修改Deployment名称和Snake镜像名称
                sh """kubectl set image deployment/snake-demo-snake-demo-euqh6zvl snake-demo-snake-demo-euqh6zvl=hub.easystack.io/3dc70621b8504c98/snake:${BUILD_NUMBER}"""
            }
        }
    }
 }
}
