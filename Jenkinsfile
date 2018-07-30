pipeline {
  agent any
  stages {
    stage('Nmap Scan') {
      stage('Download secureCodeBox CLI') {
        steps {
          sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/run_scanner.sh'
          sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/sslyze.template.json'
          sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/nmap.template.json'
          fileExists 'run_scanner.sh'
          sh 'chmod +x run_scanner.sh'
        }
      }
      parallel {
        stage('Nmap Scan') {
          steps {
            sh './run_scanner.sh -b http://engine-oss.secure-code-box-jannik-ansible.svc:8080 http://elasticsearch.secure-code-box-jannik-ansible.svc:9200 -i 500 -w 2 nmap http://localhost'
            archiveArtifacts 'job__nmap_result.json,job__nmap_result.readable'
          }
        }
        stage('SSLyze Scan') {
          steps {
            sh './run_scanner.sh -b http://engine-oss.secure-code-box-jannik-ansible.svc:8080 http://elasticsearch.secure-code-box-jannik-ansible.svc:9200 -i 500 -w 2 sslyze https://iteratec.de:443'
            archiveArtifacts 'job__sslyze_result.json,job__sslyze_result.readable'
          }
        }
      }
    }
  }
}