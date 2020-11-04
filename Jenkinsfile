node('master') {
 // Note : this step is only needed if you're using direct Groovy scripting
 stage 'Checkout Git project'
 git url: 'https://github.com/mycodebase-1/simple-java-maven-app.git'
 def appVersion = version()
 if (appVersion) {
 echo "Building version ${appVersion}"
 }
 stage 'Build Maven project'
 // def mvnHome = which mvn
 sh "mvn -B -Dmaven.test.failure.ignore verify"
 step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
 stage 'Security Scan using the SG Tool'
 sh "cd ${env.WORKSPACE}"
 sh "curl https://raw.githubusercontent.com/propelup/sg-scripts/master/sg-scanner.sh | sh"
}
def version() {
 def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
 matcher ? matcher[0][1] : null
}
