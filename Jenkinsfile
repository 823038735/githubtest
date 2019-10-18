pipeline {
  agent any
  parameters {        
    string(name: 'ITEMUSER', defaultValue: 'zjbdp', description: '以什么环境用户执行此次构建？')        
    string(name: 'BRANCH', defaultValue: 'master', description: '使用什么分支用于此次构建？')  
  }
  stages {
    stage('pull') {
      steps {
        dir(path: "./${ITEMNAME}") {
          git(url: "http://132.151.46.142:8081/zjbdp/cs_signal/${ITEMNAME}.git", branch: "${params.BRANCH}", credentialsId: 'username_with_password_jenkins_readonly')
        }
      }
    }
    stage('deployment') {
      steps {
        dir(path: "./${ITEMNAME}") {
          ansiblePlaybook(playbook: "./ansible/${ITEMNAME}.yaml", credentialsId: "ssh_username_with_private_key_${params.ITEMUSER}", disableHostKeyChecking: true, extras: '--extra-vars \'{"src_dir":"${WORKSPACE}/${ITEMNAME}/deploy/","ITEMUSER":"${ITEMUSER}"}\'', colorized: true, inventory: "./ansible/inventory/${ITEMUSER}")
        }
      }
    }
  }
  environment {
    ITEMNAME = 'csmap_signal_deposit_deploy'
  }
}