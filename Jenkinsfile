podTemplate(containers: [
    containerTemplate(
        name: 'cdk-agent', 
        image: 'lsolanki84/jkin-cdk',        
        command: 'sleep', 
        args: '30d'
        
        ),
  ]) {

    node(POD_LABEL) {

        stage('Project enviroment setup') {
            container('cdk-agent') {

               stage('Build CDK Template') {
                   withAWS(credentials: 'AWS_CREDENTIALS_ID', region: 'ap-south-1', role: 'arn:aws:iam::082008957495:role/awstests3fullaccess') 
                   {
                    git url: 'https://github.com/lsolanki84/testcdk.git', branch: 'main'
            
                    sh '''
                       mkdir cdk_workshop && cd cdk_workshop
                       cdk init sample-app --language python
                       . .venv/bin/activate
                       pip install -r requirements.txt
                       mkdir lambda
                       cp ../lambda/hello.py lambda/hello.py
                       cp ../cdk_workshop_stack.py cdk_workshop/cdk_workshop_stack.py
                       cdk doctor
                       cdk synth
                       cdk diff
                       cdk bootstrap
#                       cdk deploy --require-approval never
                       cdk destroy -f
                    '''
                    }
               }
            
                 
            }

        }
    }
}
