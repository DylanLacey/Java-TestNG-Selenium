node {
tool name: 'Maven 3.6.1', type: 'maven'
tool name: 'OpenJDK 11', type: 'jdk'

stage('Get Ready') {
    // Get some code from a GitHub repository
    checkout scm
    // Note: if this is copy and pasted into pipeline script, the following will work while the above handles branches and such
    // git url: 'https://github.com/saucelabs-sample-test-frameworks/Java-TestNG-Selenium.git'
    sh "mvn install"
}

stage('Go') {
    sauce('dylan_USW') {
        sauceconnect(useGeneratedTunnelIdentifier: true, verboseLogging: true) {
          sh "mvn test"
        }
    }
}

stage('Collect Results') {
    step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
    step([$class: 'SauceOnDemandTestPublisher'])
}
