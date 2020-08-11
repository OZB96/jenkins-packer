pipeline {
    agent { docker { 
		image 'bryandollery/terraform-packer-aws-alpine'
		args "-u root"
		}
	 }
    environment {
	TF_NAMESPACE="omar"
	OWNER="omar"
	AWS_PROFILE="kh-labs"
	PROJECT_NAME="web-server"
	}
    options {
        skipStagesAfterUnstable()
    }
    parameters {
    credentials(
        credentialType: 'com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl',
        defaultValue: '',
        description: 'The credentials needed to deploy.',
        name: 'aws_omar_creds',
        required: true
    )
}
stages{	
   stage('cloneRepo'){
	    steps {
	sh 'rm -rf jenkins-packer || true '
	sh 'git clone https://github.com/OZB96/jenkins-packer'	
	echo 'Done clonning'
	}
	}
   stage('addCredsfile'){
   steps {
    withCredentials([usernamePassword(
        credentialsId: '${aws_omar_creds}',
        usernameVariable: 'aws_access',
        passwordVariable: 'aws_secret',
    )]){
	sh 'echo aws_access_key_id="${aws_access}" >> ./jenkins-packer/creds/credentials'
	sh 'echo aws_secret_access_key="${aws_secret}" >> ./jenkins-packer/creds/credentials'
	sh 'export AWS_ACCESS_KEY_ID="${aws_access}"'
	sh 'export AWS_SECRET_ACCESS_KEY="${aws_secret}"'
	}}}
   stage('build') {
            steps {
            	sh "cd jenkins-packer && packer build"
		}
        }
}
}
