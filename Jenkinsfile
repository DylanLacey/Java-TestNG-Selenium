node {
    tool name: 'OpenJDK 11', type: 'jdk'
    tool name: 'Maven 3.6.1', type: 'maven'
    jdk('OpenJDK 11')

    stage('Get Ready') {
        // Get some code from a GitHub repository
        checkout scm
        // Note: if this is copy and pasted into pipeline script, the following will work while the above handles branches and such
        // git url: 'https://github.com/saucelabs-sample-test-frameworks/Java-TestNG-Selenium.git'
        maven("mvn install") {
            sauce('dylan_USW') {
                sauceconnect(useGeneratedTunnelIdentifier: true, verboseLogging: true) {
                   maven("test") {
                       step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
                       step([$class: 'SauceOnDemandTestPublisher'])
                   }
                }
            }
        }
    }
}