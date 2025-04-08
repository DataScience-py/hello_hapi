#!/usr/bin/env groovy

node {
  stage('SCM') {
    checkout scm
  }

  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner'
    withSonarQubeEnv('Имя вашего SonarQube сервера') {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }

  stage('Quality Gate') {
    timeout(time: 5, unit: 'MINUTES') {
      def qg = waitForQualityGate()
      if (qg.status != 'OK') {
        error "Сборка прервана: не пройден порог качества SonarQube (${qg.status})"
      }
    }
  }
}
