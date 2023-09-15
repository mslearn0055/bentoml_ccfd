node {
         
         stage("Git Clone"){

         git credentialsId: 'Git-Hub-Credentials', url: "https://github.com/prasadpgosavi/bentoml_ccfd.git"
         
         stage("Docker build"){
             sh 'docker version'
             sh 'pip install -r requirements.txt'
             sh 'python3 train.py'
             sh 'bentoml build .'
             sh 'bentoml containerize xgb_classifier:latest -t pgdocker07/xgb_classifier:latest'
         
         }
         stage("Docker Login"){
                   
             withCredentials([string(credentialsId: 'pgdocker07', variable: 'PASSWORD')]) {
        	    sh "docker login -u pgdocker07 -p ${PASSWORD}"
         }
         }
         stage("Push image to docker hub"){
             sh 'docker push pgdocker07/xgb_classifier:latest'
         }

         stage("Kubernetes deployment"){
             kubernetesDeploy (configs: 'deploymentservice.yaml', kubeconfigId: 'kuberneteskey')
           }

        }
    }
