pipeline {
    agent any

    tools {
        maven "MAVEN399"
        jdk "JDK17"
    }

    environment {
        SNAP_REPO       = 'vprofile-snapshot'
        NEXUS_USER      = 'admin'
        NEXUS_PASS      = 'admin123'
        RELEASE_REPO    = 'vprofile-release'
        CENTRAL_REPO    = 'vpro-maven-central'
        NEXUSIP         = '172.31.43.144'
        NEXUSPORT       = '8081'
        NEXUS_GRP_REPO  = 'vpro-maven-group'
        NEXUS_LOGIN     = 'nexuslogin'

        TEAMS_WEBHOOK   = credentials('teams-webhook')
    }

    stages {
        stage('Build') {
            steps {
                echo "Building project using Maven..."
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }
    }

    post {
        success {
            echo "Build succeeded — sending Teams notification"
            sh """
            /usr/local/bin/teams-notify.sh \
                "$TEAMS_WEBHOOK" \
                "SUCCESS" \
                "Build succeeded" \
                "${env.JOB_NAME}" \
                "${env.BUILD_NUMBER}" \
                "${currentBuild.durationString}" \
                "${env.BUILD_URL}"
            """
        }

        failure {
            echo "Build failed — sending Teams notification"
            sh """
            /usr/local/bin/teams-notify.sh \
                "$TEAMS_WEBHOOK" \
                "FAILED" \
                "Build failed" \
                "${env.JOB_NAME}" \
                "${env.BUILD_NUMBER}" \
                "${currentBuild.durationString}" \
                "${env.BUILD_URL}"
            """
        }
    }
}