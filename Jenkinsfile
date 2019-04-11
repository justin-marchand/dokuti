def label = "dokuti-${UUID.randomUUID().toString()}"

podTemplate(label: label, 
    containers: [
        containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
    ]) {

    node(label) {
        // Determine the branch information to send to the dokuti-pipeline as parameters.
        def myRepo = checkout scm
        def gitCommit = myRepo.GIT_COMMIT
        def gitBranch = myRepo.GIT_BRANCH
        
        stage('Run Downstream Pipeline'){
            // builds the current dokuti commit using the latest version on the master branch of the dokuti-pipeline project.
            build job: 'dokuti-pipeline/master', 
				    parameters: [
				        string(name: 'DOKUTI_BRANCH', value: gitBranch),
				        string(name: 'DOKUTI_COMMIT', value: gitCommit)
				    ]
        }
 }
}