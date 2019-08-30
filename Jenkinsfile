def label = "worker-${UUID.randomUUID().toString()}"

node(label) {

    def scmVars = checkout scm
    def branch = scmVars.GIT_BRANCH

    stage('Using Worktile Pipeline') {
        script {
            if (branch == 'origin/release' || branch == 'release' || branch == 'integration' || branch == 'origin/integration') {
                echo 'Run Integration Test'

                container('node') {
                    sh """
                        cd packages/gaea
                        export 'MONGOMS_DISABLE_POSTINSTALL=1' && MONGOMS_SYSTEM_BINARY=/usr/bin/mongod
                        export WTCTL_PLATFORM=2
                        cp -r /tmp/cache/node_modules ./
                        npm install
                        npm run integration-test
                    """
                }
            }
            else {
                container("wtctl") {
                    sh "wtctl"
                }
            }
        }
    }
}
