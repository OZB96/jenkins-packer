pipeline {
    agent { docker { image 'ubuntu' } }
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
    stages {
    stage('install_dependinces'){
	steps{
	sh 'apt update -y && apt install -y curl vim jq git make docker.io'
	}
	}
    stage('cloneRepo'){
	    steps {
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
	sh 'touch jenkins-packer/creds/credentials'
	sh "echo '[kh-labs]' >> ./jenkins-packer/creds/credentials"
	sh 'echo aws_access_key_id="${aws_access}" >> ./jenkins-packer/creds/credentials'
	sh 'echo aws_secret_access_key="${aws_secret}" >> ./jenkins-packer/creds/credentials'
	}}}
   stage('build') {
            steps {
		sh "cd jenkins-packer && make init && make start && docker docker exec -it \$(basename $PWD) make build"
            }
        }
        }
}
