pipeline {
    agent any

    tools {
        maven "MAVEN3.9"
        jdk "JDK17"
    }

    environment {
        SNAP_REPO     = 'vprofile-snapshot'
        NEXUS_USER    = 'admin'
        NEXUS_PASS    = 'admin123'
        RELEASE_REPO  = 'vprofile-release'
        CENTRAL_REPO  = 'vpro-maven-central'
        NEXUSIP       = '172.31.43.144'
        NEXUSPORT     = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN    = 'nexuslogin'

        // ✅ MUST match your Jenkins credential ID
        TEAMS_WEBHOOK = credentials('teams-webhook')
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }
    }

    post {

        success {
            sh """
            teams-notify.sh \
                "$TEAMS_WEBHOOK" \
                "✅ Jenkins Build Success" \
                "Build succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}" \
                "2ECC71"
            """
        }

        failure {
            sh """
            teams-notify.sh \
                "$TEAMS_WEBHOOK" \
                "❌ Jenkins Build FAILED" \
                "Build failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}" \
                "E74C3C"
            """
        }
    }
}