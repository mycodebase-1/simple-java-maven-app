node('master') {
 // Note : this step is only needed if you're using direct Groovy scripting
 stage 'Checkout Git project'
 git url: 'https://github.com/propelup/simple-java-maven-app.git'
 def appVersion = version()
 if (appVersion) {
 echo "Building version ${appVersion}"
 }
 stage 'Build Maven project'
 // def mvnHome = which mvn
 sh "mvn -B -Dmaven.test.failure.ignore verify"
 step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
 stage 'Security Scan using the SG Tool'
 sh "echo 'pwd' and ${env.WORKSPACE} "
 sh "docker run --rm --net=host -v "pwd":/project -v /var/run/docker.sock:/var/run/docker.sock --name scancont -e SEC_BASE_PATH=http://54.79.131.67:8080 -e CLAIR_ADDR=54.79.131.67 sg-scanner"
}
def version() {
 def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
 matcher ? matcher[0][1] : null
}
