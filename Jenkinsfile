node {
         stage("Activate python"){
             sh 'curl -O https://bootstrap.pypa.io/get-pip.py'
             sh 'python3 get-pip.py'
             sh 'export PATH=LOCAL_PATH:$PATH'
             sh 'source /home/ubuntu/.bashrc'
             sh 'pip install pip'
         }
         stage("Git Clone"){

         git credentialsId: 'Git-Hub-Credentials', url: "https://github.com/Devendra61/bentoml_ccfd.git"
         
         stage("Docker build"){
             sh 'docker version'
             sh 'pip install -r requirements.txt'
             sh 'python3 train.py'
             sh 'bentoml build .'
             sh 'bentoml containerize xgb_classifier:latest -t devbarahen61/xgb_classifier:latest'
         
         }
         stage("Docker Login"){
                   
             withCredentials([string(credentialsId: 'devbarahen61', variable: 'PASSWORD')]) {
        	    sh "docker login -u devbarahen61 -p ${PASSWORD}"
         }
         }
         stage("Push image to docker hub"){
             sh 'docker push devbarahen61/xgb_classifier:latest'
         }

         stage("Kubernetes deployment"){
             kubernetesDeploy (configs: 'deploymentservice.yaml', kubeconfigId: 'kuberneteskey')
           }

        }
    }
