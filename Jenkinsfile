pipeline{
  agent { label 'nodejs' }
  stages{
    stage ('checkout'){
      steps{
        git url: ''
      }
    }
    stage ('install modules'){
      steps{
        sh '''
          npm install --verbose -d 
          npm install --save classlist.js
        '''
      }
    }
    stage ('code quality'){
      steps{
        sh '$(npm bin)/ng lint'
      }
    }
    stage ('build') {
      steps{
        sh '$(npm bin)/ng build --prod --build-optimizer'
      }
    }
    stage ('build image') {
      steps{
        sh '''
          rm -rf node_modules
          oc new-build --strategy docker --binary --name angular-test
          oc start-build angular-test --from-dir=. --follow
        '''
      }
    }
    stage ('deploy image') {
      steps{
        sh '''
          oc new-app angular-test
          oc expose svc/angular-test
          oc get route angular-test
        '''
      }
    }
  }
}
