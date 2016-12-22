node("usine") {
        stage("stage1") {
          checkout scm
          docker.withServer('tcp://lr2pr571v.groupe.generali.fr:2375') {
           docker.image('springio/gs-spring-boot-docker').withRun('-p 8080:8080') {c ->
            sh "docker logs ${c.id}"
            //sh "curl -i http://localhost:8080/"
            input 'Continue?'
           }
          }
        }
}
