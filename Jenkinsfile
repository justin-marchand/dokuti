def label = "dokuti-${UUID.randomUUID().toString()}"

podTemplate(label: label, 
    containers: [
        containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
    ],
    volumes: [
            hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
            hostPathVolume(mountPath: '/root/.m2/repository', hostPath: '/root/.m2/repository'),
            hostPathVolume(mountPath: '/tmp', hostPath: '/tmp')
    ]) {

    node(label) {
    
    	def myRepo = checkout scm
        def gitCommit = myRepo.GIT_COMMIT
        def gitBranch = myRepo.GIT_BRANCH
        
        println "Git Commit: ${gitCommit} Git Branch: ${gitBranch}"
        
        stage('Run Downstream Pipeline'){
            //build 'dokuti-pipeline/887-link-upstream-and-downstream-repos'
            build job: 'dokuti-pipeline/887-link-upstream-and-downstream-repos', parameters: [[$class: 'StringParameterValue', name: 'DOKUTI_BRANCH', value: 887-link-upstream-and-downstream-repos], [$class: 'StringParameterValue', name: 'ParamB', value: paramBValue]]
        }
 }
}