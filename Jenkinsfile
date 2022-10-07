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
        '''
    }
  }
  stages {
    stage('Generate docx') {
      steps {
        container('maven') {
          script {

            dir('xml') {

              fileOperations(
                  [
                      fileCopyOperation(excludes: '', flattenFiles: false, includes: '**/*.*' , targetLocation: '../target')
                  ]
              
              )
            }

              
            def binding = [
                 packageName : "Foo",
                 version  : "1.2.3"
            ]

            dir('templates') {

              ['word/settings.xml', 'docProps/custom.xml'].each{ f->
                
                def t = readFile file: f
                writeFile file: '../target/' + f, text: renderTemplate(t, binding)

              }
            }
            
            zip zipFile: 'test.docx', archive: false, dir: 'target'
            archiveArtifacts artifacts: 'test.docx', fingerprint: true


          }
        }
      }
    }
  }
}

@NonCPS
def renderTemplate(String  t, Map b) {
    def e =  new groovy.text.StreamingTemplateEngine()
    return e.createTemplate(t).make(b).toString()

}