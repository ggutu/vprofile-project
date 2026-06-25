pipeline {
    agent any
    tools {
        maven 'MAVEN3.9'
        jdk 'JDK21'
    }
    
    environment {
        SNAP_REPO = 'vprofile-snapshot'
		// #NEXUS_USER = 'admin'
		// #NEXUS_PASS = 'admin123'
		RELEASE_REPO = 'vprofile-release'
		CENTRAL_REPO = 'vpro-maven-central'
        // 100% can be Dynamic: Pulling from Jenkins Global Configuration
		NEXUSIP = '172.31.36.143'
		NEXUSPORT = '8081'
		NEXUS_GRP_REPO = 'vpro-maven-group'
        // #NEXUS_LOGIN = 'nexuslogin'
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
        NEXUS_AUTH     = credentials('nexuslogin')
    }

    stages {
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post{
                success{
                    echo "Now archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -s settings.xml test'
            }
        }  
        stage('checkstyle Analysis') {
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }
        stage('Sonar Analysis') {
    steps {
        // Inject the JVM flag to allow access to java.lang
        withEnv(["SONAR_SCANNER_OPTS=--add-opens=java.base/java.lang=ALL-UNNAMED"]) {
            withSonarQubeEnv('sonarserver') {
                sh '''
                /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonarscanner/bin/sonar-scanner \
                -Dsonar.projectKey=vprofile \
                -Dsonar.projectName=vprofile \
                -Dsonar.projectVersion=1.0 \
                -Dsonar.sources=src/ \
                -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                -Dsonar.junit.reportsPath=target/surefire-reports/ \
                -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
                '''
            }
        }
    }
        }
         stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage("UploadArtifact"){
            steps{
                nexusArtifactUploader(
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                  groupId: 'QA',
                  version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                  repository: "${RELEASE_REPO}",
                  credentialsId: "${NEXUS_LOGIN}",
                  artifacts: [
                    [artifactId: 'vproapp',
                     classifier: '',
                     file: 'target/vprofile-v2.war',
                     type: 'war']
                  ]
                )
            }
        }

    }
}