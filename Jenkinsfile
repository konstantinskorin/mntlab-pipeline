node('master') {
stage('Preparation') {
    echo "Preparation stage begins."
    git branch: 'kskorin', url: 'https://github.com/konstantinskorin/mntlab-pipeline.git'
    }
    
stage('Building code') {
     echo "Building stage begins."
     sh '/opt/gradle/gradle-5.6.2/bin/gradle build'
    }


stage('Testing code'){
     echo "Testing stage begins."
     parallel (
     Unit_tests: {echo "Unit Test  stage begins."; sh '/opt/gradle/gradle-5.6.2/bin/gradle test' },
     Jacoco_tests: {echo "Jacoco Test stage begins."; sh '/opt/gradle/gradle-5.6.2/bin/gradle jacocoTestReport' },
     Cucumber_tests: {echo "Cucumber Test stage begins."; sh '/opt/gradle/gradle-5.6.2/bin/gradle cucumber' }
        )
    }

stage('Packaging and Publishing') {
     echo "Packaging and Publishing stage begins."
     sh 'tar -xvf kskorin_dsl_script.tar.gz'
     sh 'tar -czvf pipeline-kskorin-${BUILD_NUMBER}.tar.gz -C build/libs/ gradle-simple.jar'
     archiveArtifacts 'pipeline-kskorin-${BUILD_NUMBER}.tar.gz'
    }
stage('Asking for manual approval') {
     echo "Approval stage begins."
     input 'Approve deploy?'
    }
stage('Deployment') {
     echo "Deployment stage begins."
     sh "java -jar build/libs/gradle-simple.jar"
    }


stage('Sending status') {
     echo "SUCCESS!"
   }
}
