pipeline {
  agent any
    environment {
      // The following variable is required for a Semgrep Cloud Platform-connected scan:
      SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')

      // Uncomment the following line to scan changed 
      // files in PRs or MRs (diff-aware scanning): 
      SEMGREP_BASELINE_REF = "master"

      // Troubleshooting:

      // Uncomment the following lines if Semgrep Cloud Platform > Findings Page does not create links
      // to the code that generated a finding or if you are not receiving PR or MR comments.
      SEMGREP_JOB_URL = "${BUILD_URL}"
      SEMGREP_COMMIT = "${GIT_COMMIT}"
      SEMGREP_BRANCH = "${GIT_BRANCH}"
      SEMGREP_REPO_NAME = env.GIT_URL.replaceFirst(/^https:\/\/github.com\/(.*)$/, '$1')
      SEMGREP_REPO_URL = env.GIT_URL.replaceFirst(/^(.*).git$/,'$1')
      SEMGREP_PR_ID = "${env.CHANGE_ID}"
    }
    stages {
      stage('Semgrep-Scan') {
        steps {
            sh '''docker pull returntocorp/semgrep && \
            docker run \
            -e SEMGREP_APP_TOKEN=$SEMGREP_APP_TOKEN \
            -e SEMGREP_REPO_URL=$SEMGREP_REPO_URL \
            -e SEMGREP_BRANCH=$SEMGREP_BRANCH \
            -e SEMGREP_REPO_NAME=$SEMGREP_REPO_NAME \
            -e SEMGREP_BRANCH=$SEMGREP_BRANCH \
            -e SEMGREP_COMMIT=$SEMGREP_COMMIT \
            -e SEMGREP_PR_ID=$SEMGREP_PR_ID \
            -v "$(pwd):$(pwd)" --workdir $(pwd) \
            returntocorp/semgrep semgrep ci --oss-only'''
      }
    }
  }
}
