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

	}
}