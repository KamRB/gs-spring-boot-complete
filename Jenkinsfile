node {
  try{
    stage 'checkout project'
    git url: 'https://github.com/KamRB/gs-spring-boot-complete.git'

    stage 'check env'

    sh "mvn -v"
    sh "java -version"

    stage 'test'
    sh "mvn test"

    stage 'package'
    sh "mvn package"

    stage 'preview'
    sh 'make deploy-default'

    stage 'report'
    step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])

    stage 'Artifact'
    step([$class: 'ArtifactArchiver', artifacts: '**/target/*.jar', fingerprint: true])

    try{
      stage 'Approve, go production'
      def url = 'http://localhost:8000/'
      input message: "Does staging at $url look good? ", ok: "Deploy to production"
    }finally{
      sh "ssh jenkins@localhost 'kill `cat deploy/release/run.pid`'"
    }

    stage 'deploy'
    sh 'make deploy-default'

     }catch(e){
    throw e;
  }
}
