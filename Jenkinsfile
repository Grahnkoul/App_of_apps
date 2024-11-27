def frontendImage="jozefowiczadam/frontend"
def backendImage="jozefowiczadam/backend"

pipeline 
{
    agent 
    {
        label 'agent'
    }
    tools 
    {
        terraform 'Terraform'
    }


    environment 
    {
        PIP_BREAK_SYSTEM_PACKAGES = 1
    }
	
	parameters 
	{
        string(name: 'backendDockerTag', defaultValue: 'latest', description: 'Backend docker image tag')
        string(name: 'frontendDockerTag', defaultValue: 'latest', description: 'Frontend docker image tag')
    }
	
	stages 
    {
        stage('Get_Code') 
        {
            steps 
            {
                checkout scm
            }
        }
		
		stage('Show version') 
		{
            steps 
            {
                script
                {
                    currentBuild.description = "Backend: ${backendDockerTag}, Frontend: ${frontendDockerTag}"
                }
            }
		}

        stage('Clean running containers') 
        {
            steps 
            {
                sh "docker rm -f frontend backend"
            }
        }
        stage('Deploy application') 
        {
            steps 
            {
                script 
                {
                    withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag", 
                             "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) 
                    {
                        sh "docker-compose up -d"
                    }
                }
            }
        }
        stage('Selenium tests') 
        {
            steps 
            {
                sh "pip3 install -r test/selenium/requirements.txt"
                sh "python3 -m pytest test/selenium/frontendTest.py"
            }
        }

        stage('Run terraform') 
        {
            steps 
            {
                dir('Terraform') 
                {                
                    git branch: 'main', url: 'https://github.com/Grahnkoul/Terraform'
                    withAWS(credentials:'AWS', region: 'us-east-1') 
                    {
                            sh 'terraform init -backend-config=bucket=adam-jozefowicz-panda-devops-core-19'
                            sh 'terraform apply -auto-approve -var bucket_name=adam-jozefowicz-panda-devops-core-19'
                            
                    } 
                }
            }
        }


	}
    
    post 
    {
        always 
        {
          sh "docker-compose down"
          cleanWs()
        }
    }
}