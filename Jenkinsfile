pipeline {
    agent {
        kubernetes {
            label 'ansible'
        }
    }
    stages {
        stage('Prepare [ build env ]') {
            steps {
                container('centos-6') {
                    sh 'yum install -y epel-release'
                    sh 'yum install -y ansible'
                }
            }
        }
        stage('Build [ pull request ]') {
            when {
                changeRequest()
            }
            steps {
                container('centos-6') {
                    ansiblePlaybook(
                            inventory: 'inventory_local.ini',
                            playbook: 'playbook.yml',
                            extraVars: [
                                    run_as_gcc  : true,
                                    experimental: true,
                                    ci: true
                            ])
                }

            }
        }
        stage('Build [ master ]') {
            when {
                branch 'master'
            }
            steps {
                milestone 1
                container('centos-6') {
                    ansiblePlaybook(
                            inventory: 'inventory_local.ini',
                            playbook: 'playbook.yml',
                            extraVars: [
                                    run_as_gcc  : true,
                                    experimental: true,
                                    ci: true
                            ])
                }
            }
        }
        stage('Release [ master ]') {
            when {
                branch 'master'
            }
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    script {
                        input(
                                message: 'Do you want to release?',
                                ok: 'Release'
                        )
                    }
                }
                milestone 2
                container('centos-6') {
                    ansiblePlaybook(
                            inventory: 'inventory_local.ini',
                            playbook: 'playbook.yml',
                            extraVars: [
                                    run_as_gcc  : true,
                                    experimental: true,
                                    ci: true
                            ])
                }
            }
        }
    }
}