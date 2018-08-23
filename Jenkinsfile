podTemplate(name: 'jnlp', label: 'jnlp', cloud: 'kubernetes',
  containers: [
        containerTemplate(
            name: 'jnlp',
            //请按需修改Jenkins Slave镜像名称
            image: 'docker.io/jenkins/jnlp-slave:alpine',
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
    stage('CICD for Snake Game Demo') {
        container('jnlp') {
            stage("Clone source code of Snake Game") {
                //请按需修改Git源代码库地址
                //如果是Private项目，参考示例如下（需使用GitLab Access Token）
                //sh """
                    //git clone http://oauth2:QK6YdGhk3muxTPzAQC1B@172.16.6.28:30080/easystack/snake-demo.git
                //"""
                //如果是Public项目，参考示例如下
                git 'http://172.16.6.28:30080/easystack/snake-demo.git'
            }
                      
            stage('Build & push docker image') {
                //请按需修改镜像仓库的账号和密码，并注意docker build命令中Dockerfile所在路径
                sh """
                    docker login -u 3dc70621b8504c98 -p Tcdf4f05247d79dd7 hub.easystack.io  
                    docker build -t hub.easystack.io/3dc70621b8504c98/snake:v${BUILD_NUMBER} ./
                    docker push hub.easystack.io/3dc70621b8504c98/snake:v${BUILD_NUMBER}
                """
            }
            
            //stage('Deploy app to EKS') {
                //请按需修改Deployment名称和Snake镜像名称
                //sh """kubectl set image deployment/snake-demo-snake-demo-cao7ea5d snake-demo-snake-demo-cao7ea5d=hub.easystack.io/3dc70621b8504c98/snake:v${BUILD_NUMBER}"""
            //}
        }
    }
 }
}
