pipeline {
  agent any
  environment {
    folder_build_lib = "cpp/build"
  }

  stages {
    stage('Build') {
      steps {
        echo 'Building..'
        dir ('lib') {
          git branch: "master", changelog: false, poll: false, url: 'https://github.com/googlei18n/libphonenumber.git'
          sh 'rm -rf cpp/build'
          sh 'mkdir cpp/build'
        }
        dir ('lib/cpp/build') {
          sh 'cmake ".."'
          sh 'LDFLAGS=\"-licuuc -lgeocoding\" make'
          sh 'sudo make install'
        }
      } 
    }

    stage('Test') {
      steps {
        echo 'Testing..'
        dir ('lib/cpp/build') {
          sh './libphonenumber_test'
        }
      }
    }
    stage('Deploy') {
      steps {
          echo 'Deploying....'
      }
    }
    stage('Archive') {
      steps {
        echo 'Archiving....'
        nexusArtifactUploader (
        nexusVersion: 'nexus3',
        protocol: 'http',
        nexusUrl: 'localhost:8081',
        groupId: 'pragma.lib',
        version: '0.0.1',
        repository: 'NexusArtifactUploader',
        credentialsId: 'f470582e-bbd7-4b11-9860-ce0de28da45e',
        artifacts: [
          [artifactId: 'libphonenumer_test',
           classifier: '',
           file: 'lib/cpp/build/libphonenumber_test', type: '']
          ]
        )
      }
    }
  }
}
