pipeline {
    agent label:""
    tools {
        maven "mvn3.3.9"
        jdk "jdk8"
    }
    stages {
        stage("Build") {
            sh "mvn clean install"
            sh "mkdir -p deliverables"
        }
        stage("Prepare") {
            parallel(
                "Copy Javadoc": {
                    sh "cp -r target/site/apidocs deliverables"
                },
                "Copy Jar": {
                    sh "cp target/*.jar deliverables"
                }
            )
        }
    }
    postBuild {
        always {
            junit "target/surefire-reports/*.xml"
            step([$class: 'FindBugsPublisher', pattern: 'target/findbugs.xml'])
        }
        success {
            archiveArtifacts artifacts: 'deliverables/**'
        }
    }
}