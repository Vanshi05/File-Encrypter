pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                echo "Building Java project..."
                echo "Listing workspace contents:"
                ls
                cd "Password Protection"
                mkdir -p build
                javac -d build src/*.java
                echo "Build successful"
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                echo "Running JUnit tests for File-Encrypter..."
                cd "Password Protection"

                # Download JUnit jar if not present
                if [ ! -f junit-platform-console-standalone-1.10.0.jar ]; then
                    echo "Downloading JUnit..."
                    curl -L -o junit-platform-console-standalone-1.10.0.jar \
                    https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar
                fi

                # Compile test files (test folder beside src)
                mkdir -p test-build
                javac -cp junit-platform-console-standalone-1.10.0.jar:build -d test-build test/*.java

                # Run JUnit tests
                java -jar junit-platform-console-standalone-1.10.0.jar \
                    --class-path build:test-build \
                    --scan-class-path
                '''
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
