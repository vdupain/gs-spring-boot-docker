node("usine") {
    stage("checkout") {
        git changelog: false, poll: false, url: 'ssh://git@git.groupe.generali.fr:7999/sandbox/gs-spring-boot-docker.git'
    }
          
    stage("build maven") {
        def mvnHome = tool 'M325'
        def javaHome = tool 'JDK_8.0'
        env.JAVA_HOME=javaHome
         sh "${mvnHome}/bin/mvn clean package"
    }

    docker.withServer('tcp://127.0.0.1:2375') {
        stage("Build docker image") {
            def myEnv = docker.build 'generali-spring-boot-demo:snapshot'
        }
    
        stage("Run container") {
            docker.image('generali-spring-boot-demo:snapshot').withRun('-p 8080:8080') {c ->
                sh "docker logs ${c.id}"
                //sh "curl -i http://localhost:8080/"
                input 'Continue?'
            }
        }
    }
}