def label = "worker-${UUID.randomUUID().toString()}"

podTemplate(label: label, cloud: 'kubernetes',
    containers: [
        containerTemplate(name: 'node', image: 'worktile/dind-helm-k8s-mongod:1.0.0', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'wtctl', image: 'worktile/wtctl:1.2.8', ttyEnabled: true, command: 'cat')
    ],
    volumes: [
        hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
        hostPathVolume(mountPath: '/mongod', hostPath: '/data/cache/mongod'),
        hostPathVolume(mountPath: '/root/.npm/node-sass/4.11.0/linux-x64-64_binding.node', hostPath: '/data/cache/binding.node.4.11.0'),
        hostPathVolume(mountPath: '/root/.npm/node-sass/4.12.0/linux-x64-64_binding.node', hostPath: '/data/cache/binding.node.4.12.0'),
        hostPathVolume(mountPath: '/tmp/cache', hostPath: '/tmp/cache/wt-rd-pipeline'),
        hostPathVolume(mountPath: '/root/.ssh', hostPath: '/root/.ssh')
    ]
) {
    node(label) {

        def scmVars = checkout scm
        def branch = scmVars.GIT_BRANCH

        stage('Using Worktile Pipeline') {
            script {
                container("wtctl") {
                    sh "wtctl"
                }
            }
        }
    }
 }