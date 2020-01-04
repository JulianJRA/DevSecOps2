pipeline {
   agent any
   stages {
      stage('Build') {
         steps {
            sh '''
               mvn clean
               sudo npm --prefix src/main/frontend install
               npm --prefix src/main/frontend run build
               mvn package
            '''
         }
      }
      stage('Archive') {
         steps {
         sh '''
               export COMMIT_ID=`cat .git/HEAD`
               sudo cp ${WORKSPACE}/target/*.jar ${WORKSPACE}/target/${COMMIT_ID}.war
            '''            
            script {
                def remote = [:]
                remote.name = 'archiver'
                remote.user = 'vagrant'
                remote.allowAnyHosts = true
                remote.host = 'archiver.local'
                remote.identityFile = '~/.ssh/archiver.key'
                    sshPut remote: remote, from: '${COMMIT_ID}.jar', into: '/home/vagrant/'
            }
         }
      }   
   }
}