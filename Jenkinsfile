def frontendImage="jozefowiczadam/frontend"
def backendImage="jozefowiczadam/backend"

pipeline 
{
    agent 
    {
        label 'agent'
    }
	
	 parameters 
	 {
        string(name: 'backendDockerTag', defaultValue: 'lastest', description: 'Backend docker image tag')
        string(name: 'frontendDockerTag', defaultValue: 'lastest', description: 'Frontend docker image tag')
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