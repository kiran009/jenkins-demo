node() {
    stage('Checkout') {
        checkout(scm)
        sh 'git clean -xdf'
        lastChanges since: 'LAST_SUCCESSFUL_BUILD', format:'SIDE', matching: 'LINE'
    }
    stage('Build and test') {
        sh './gradlew build'
        junit 'build/test-results/test/*.xml'
    }
    stage('Create a Manifest') {
        String manifestName = 'MANIFEST.txt'
        String revision = sh(returnStdout: true,
          script: 'git rev-parse HEAD').trim()
        writeFile file: manifestName, text: revision
        archive manifestName
    }
}
