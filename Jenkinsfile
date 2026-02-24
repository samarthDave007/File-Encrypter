node('agent') {

    // CLEAN WORKSPACE (this replaces "Delete workspace before build")
    deleteDir()

    stage('Build') {
        sh '''
        echo "Building Java project..."
        echo "Workspace contents:"
        ls
        cd "Password Protection"
        mkdir -p build
        javac -d build src/*.java
        '''
    }

    stage('Test') {
        sh '''
        cd "Password Protection"

        if [ ! -f junit-platform-console-standalone.jar ]; then
            curl -L -o junit-platform-console-standalone.jar \
            https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0.jar
        fi

        mkdir -p test-build
        javac -cp junit-platform-console-standalone.jar:build -d test-build test/*.java

        java -jar junit-platform-console-standalone.jar \
        --class-path build:test-build \
        --scan-class-path
        '''
    }

    stage('Deploy') {
        sh '''
        cd "Password Protection"
        jar cf FileEncrypter.jar -C build .
        '''
    }
}
