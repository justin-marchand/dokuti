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
        stage('Run Downstream Pipeline'){
            build 'dokuti-pipeline/master'
        }
 }
}