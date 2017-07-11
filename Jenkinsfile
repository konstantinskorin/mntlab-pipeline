node(env.SLAVE) {
	env.PATH=env.PATH+":/opt/gradle-4.0.1/bin"

stage('Preparation (Checking out)') { 
		git branch: 'pyurchuk', url: 'https://github.com/MNT-Lab/mntlab-pipeline'
}

stage('Builing code') {
     sh "gradle build"  
}
    
stage('Testing code') {
parallel (
    'Cucumber Tests': { sh "gradle cucumber" },
    'Jacoco Tests': { sh "gradle jacocoTestReport" },
    'Unit Tests': { sh "gradle test" }
    )
}

stage ('Triggering job and fetching artefact after finishing') {
build job: "EPBYMINW6405/MNTLAB-${student}-child1-build-job", parameters: [string(name: 'BRANCH_NAME', value: "${student}")]
echo WORKSPACE
step(
    [$class: 'CopyArtifact',
    filter: "${student}_dsl_script.tar.gz",
    projectName: "EPBYMINW6405/MNTLAB-${student}-child1-build-job" ])
    }

stage ('Packaging and Publishing results') {
    sh 'tar -xf ${student}_dsl_script.tar.gz jobs.groovy'
    sh 'tar -czf pipeline-${student}-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile -C build/libs gradle-simple.jar'
    }
}
