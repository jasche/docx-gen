pipeline {
  agent {
    kubernetes {
      idleMinutes 5 
      yaml '''
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            example-label: foo
        spec:
          containers:
          - name: maven
            image: maven:alpine
            command:
            - cat
            tty: true
          - name: busybox
            image: busybox
            command:
            - cat
            tty: true
        '''
    }
  }
  stages {
    stage('Run maven') {
      steps {
        container('maven') {
          script {
              
            def binding = [
                 firstname : "Grace",
                 lastname  : "Hopper",
                 accepted  : true,
                 title     : 'Groovy for COBOL programmers'
             ]
             def engine = new groovy.text.SimpleTemplateEngine()
             def text = '''\
             Dear <%= firstname %> $lastname,
            
             We <% if (accepted) print 'are pleased' else print 'regret' %> \
             to inform you that your paper entitled
             '$title' was ${ accepted ? 'accepted' : 'rejected' }.
            
             The conference committee.
             '''
             def template = engine.createTemplate(text).make(binding)
             println template.toString()              
          }
        }
        container('busybox') {
          sh '/bin/busybox'
        }
      }
    }
  }
}
