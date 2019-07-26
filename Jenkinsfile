node('master') {
    stage('Preparation') {
        echo "Preparation stage begins."
        git branch: 'kskorin', url: 'https://github.com/konstantinskorin/mntlab-pipeline.git'
    }
    
    stage('Building code') {
        echo "Building stage begins."
        sh 'gradle build'
    }

    stage('Testing code'){
        echo "Testing stage begins."
        parallel (
                Unit_tests: {echo "Unit Test  stage begins."; sh 'gradle test' },
                Jacoco_tests: {echo "Jacoco Test stage begins."; sh 'gradle jacocoTestReport' },
                Cucumber_tests: {echo "Cucumber Test stage begins."; sh 'gradle cucumber' }
        )
    }
    stage('Triggeringg job and fetching artefact after finishing'){
        echo "Trigger stage begins."
        build job: "MNTLAB-kskorin-child-1-build-job", parameters: [string(name: 'BRANCH_NAME', value: 'kskorin' )]
        step ([$class: 'CopyArtifact',
        projectName: "MNTLAB-kskorin-child-1-build-job",
        filter: 'kskorin_dsl_script.tar.gz']);
    }
    stage('Packaging and Publishing') {
        echo "Packaging and Publishing stage begins."
        sh 'tar xvf kskorin_dsl_script.tar.gz'
        sh 'cp build/libs/mntlab-ci-pipeline.jar gradle-simple.jar'
        sh 'tar -czvf pipeline-kskorin-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile gradle-simple.jar'
        archiveArtifacts 'pipeline-kskorin-${BUILD_NUMBER}.tar.gz'
    }
    stage('Asking for manual approval') {
        echo "Approval stage begins."
        input 'Approve deploy?'
    }
    stage('Deployment') {
        echo "Deployment stage begins."
        sh "java -jar gradle-simple.jar"
    }

    stage('Sending status') {
    echo "SUCCESS!"
   }
    

}
