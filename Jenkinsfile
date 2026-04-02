pipeline {
    agent any

    // Tool installations configured in:
    // Manage Jenkins → Global Tool Configuration
    tools {
        maven "MAVEN399"
        jdk "JDK17"
    }

    environment {
        // Nexus configuration (your values)
        SNAP_REPO       = 'vprofile-snapshot'
        NEXUS_USER      = 'admin'
        NEXUS_PASS      = 'admin123'
        RELEASE_REPO    = 'vprofile-release'
        CENTRAL_REPO    = 'vpro-maven-central'
        NEXUSIP         = '172.31.43.144'
        NEXUSPORT       = '8081'
        NEXUS_GRP_REPO  = 'vpro-maven-group'
        NEXUS_LOGIN     = 'nexuslogin'

        // Credentials stored in Jenkins
        TEAMS_WEBHOOK   = credentials('teams-webhook')
    }

    stages {

        stage('Build') {
            steps {
                echo "Building project using Maven..."
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }

        // Add more stages here (Test, Package, Upload, Deploy, etc.)
    }

    post {

        success {
            echo "Build succeeded — sending Teams notification"
            sh """
            /usr/local/bin/teams-notify.sh \
                "$TEAMS_WEBHOOK" \
                "✅ Jenkins Build Success" \
                "Build succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}" \
                "2ECC71"
            """
        }

        failure {
            echo "Build failed — sending Teams notification"
            sh """
            /usr/local/bin/teams-notify.sh \
                "$TEAMS_WEBHOOK" \
                "❌ Jenkins Build FAILED" \
                "Build failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}" \
                "E74C3C"
            """
        }
    }
}