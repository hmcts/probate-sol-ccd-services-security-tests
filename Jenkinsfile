properties([
  [$class: 'GithubProjectProperty', displayName: 'SOL-CCD-SERVICES ZAP Tests', projectUrlStr: 'https://github.com/hmcts/probate-sol-ccd-services-security-tests.git'],
  parameters([
    string(description: 'SOL-CCD-SERVICES url', defaultValue: 'http://betaDevaprobateapp01.reform.hmcts.net:4104', name: 'SOL_CCD_SERVICES_BASE_URL'),
  ])
])

node ('master') {
  stage ("SOL-CCD-SERVICES ZAP Security test"){
    try {
      sh '''
            rm -rf $WORKSPACE/zap/
            mkdir $WORKSPACE/zap/
            chmod 777 $WORKSPACE/zap/
            docker pull owasp/zap2docker-weekly
            docker run -v $WORKSPACE/zap/:/zap/wrk -u zap -i owasp/zap2docker-weekly zap-api-scan.py -t $SOL_CCD_SERVICES_BASE_URL/v2/api-docs -f openapi -r report.html
                            '''
    } catch (Exception err) {
      currentBuild.result = 'UNSTABLE'
      echo "RESULT: ${currentBuild.result}"
      echo "MESSAGE:" + err.getMessage()
      echo "0: Success, 1:	At least 1 FAIL, 2:	At least one WARN and no FAILs, 3:	Any other failure"
    } finally {
      publishHTML target: [
        alwaysLinkToLastBuild: true,
        reportDir            : "${env.WORKSPACE}/zap",
        reportFiles          : "report.html",
        reportName           : "SOL-CCD-SERVICES Zap Security Test Report"
      ]
    }
  }
}
