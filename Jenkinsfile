stage("build") {
    node("usine") {
        checkout scm

        withEnv(["JAVA_HOME=${ tool 'JDK_8.0' }", "PATH+MAVEN=${tool 'M325'}/bin:${env.JAVA_HOME}/bin"]) {        
            sh "mvn clean package"
        }
        stash includes: "target/app.jar", name: 'binary'
        stash includes: "Dockerfile", name: 'dockerfile'
    }
}

node("docker") {
    docker.withServer('tcp://127.0.0.1:2375') {
        stage("Build docker image") {
            unstash 'binary'
            def myEnv = docker.build 'spring-boot-demo:snapshot'
        }
    
        stage("Run container") {
            unstash 'dockerfile'
            docker.image('spring-boot-demo:snapshot').withRun('-p 8080:8080') {c ->
                sh "docker logs ${c.id}"
                timeout(1) {
                    input 'Continue?'
                }
            }
        }
    }
}
