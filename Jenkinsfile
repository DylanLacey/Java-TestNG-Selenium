node {
    tool name: 'OpenJDK 11', type: 'jdk'
    tool name: 'Maven 3.6.1', type: 'maven'
    jdk('OpenJDK 11')

    stage('Get Ready') {
        // Get some code from a GitHub repository
        checkout scm
        // Note: if this is copy and pasted into pipeline script, the following will work while the above handles branches and such
        // git url: 'https://github.com/saucelabs-sample-test-frameworks/Java-TestNG-Selenium.git'
        withMaven(jdk: 'OpenJDK 11') {sh "mvn install"}
    }

    stage('Go') {
        sauce('dylan_USW') {
            sh "echo $SAUCE_USERNAME"
            sauceconnect(useGeneratedTunnelIdentifier: true, verboseLogging: true) {
                withMaven(jdk: 'OpenJDK 11') {sh "mvn test"}
            }
        }
    }

    stage('Collect Results') {
        step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
        step([$class: 'SauceOnDemandTestPublisher'])
    }
}
