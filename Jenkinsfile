stage("build") {
    node("usine") {
        checkout scm

        def mvnHome = tool 'M325'
        def javaHome = tool 'JDK_8.0'
        env.JAVA_HOME=javaHome
        sh "${mvnHome}/bin/mvn clean package"
        stash includes: 'target/gs-spring-boot-docker-*.jar', name: 'binaire'
    }
}

node("docker") {
    docker.withServer('tcp://127.0.0.1:2375') {
        stage("Build docker image") {
            unstash 'binaire'
            sh 'cp target/gs-spring-boot-docker-0.1.0-SNAPSHOT.jar target/gs-spring-boot-docker-0.1.0.jar'
            def myEnv = docker.build 'generali-spring-boot-demo:snapshot'
        }
    
        stage("Run container") {
            docker.image('generali-spring-boot-demo:snapshot').withRun('-p 8080:8080') {c ->
                sh "docker logs ${c.id}"
                timeout(1) {
                    input 'Continue?'
                }
            }
        }
    }
}
