pipeline {
    agent any
    stages {
    	stage('Build') {
    		steps {
    			sh 'echo "[ $BRANCH_NAME ] Start building..."'
    			sh 'docker build -t filebeat-$BRANCH_NAME-build .'
    		}
    	}
        stage('Runing...') {
            when {
                branch 'main'
                beforeAgent true
            }
            parallel {
                stage('Node_94_34') {
                    agent {
                        node {
                            label 'Node_94_34'
                            customWorkspace '/data/jenkins/workspace/kenh14_filebeat_noti'
                        }
                    }
                    steps {
                        deploy()
                    }
                }
            }
        }
    }
}

void deploy() {
    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
        sh 'echo "[ $BRANCH_NAME ] Runing..."'
        sh 'docker run filebeat-$BRANCH_NAME-build'
        sh 'docker system prune -f'
        sh 'sleep 10s'
	}
}
