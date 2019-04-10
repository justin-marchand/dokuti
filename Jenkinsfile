def label = "dokuti-${UUID.randomUUID().toString()}"

podTemplate(label: label, 
    containers: [
        containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
    ]) {

    node(label) {
    	// Determine the branch information to send to the downstream pipeline.
    	def myRepo = checkout scm
        def gitCommit = myRepo.GIT_COMMIT
        def gitBranch = myRepo.GIT_BRANCH
        
        stage('Run Downstream Pipeline'){
            build job: 'dokuti-pipeline/887-link-upstream-and-downstream-repos', 
				    parameters: [
				        string(name: 'DOKUTI_BRANCH', value: gitBranch),
				        string(name: 'DOKUTI_COMMIT', value: gitCommit)
				    ]
        }
 }
}