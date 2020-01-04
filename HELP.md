### Building Spring Boot + AngularJS App
```node
npm install --save-dev rimraf
npm install --save-dev mkdirp
npm install --save-dev copyfiles
```

```bash
mvn clean && npm --prefix src/main/frontend run build && mvn package
```

```groovy
pipeline {
   agent any
   stages {
      stage('Staging') {
         steps {
//https://issues.jenkins-ci.org/browse/JENKINS-57269             
            script {
                def remote = [:]
                remote.name = 'staging'
                remote.user = 'vagrant'
                remote.allowAnyHosts = true
                remote.host = 'staging.local'
                remote.identityFile = '~/.ssh/staging.key'
                sshCommand remote: remote, command: "docker --version"
            }
            echo 'Hello World'
            sh '''
                node --version
            '''
         }
      }
      stage('Production') {
         steps {
            script {
                def remote = [:]
                remote.name = 'production'
                remote.user = 'vagrant'
                remote.allowAnyHosts = true
                remote.host = 'production.local'
                remote.identityFile = '~/.ssh/production.key'
                sshCommand remote: remote, command: "docker --version"
            }
            echo 'Hello World'
            sh '''
                node --version
            '''
         }
      }   
   }
}
```