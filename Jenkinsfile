pipeline {
  agent any

  stages {
    stage("Dependances") {
      steps {
        // Installer apache2
        sh "apt install apache2 -y"
      }
    }

    stage('Checkout') {
      steps {
        // Récupération du code
        sh "git clone https://github.com/Tommy-BOU/tpJenkins/"
      }
    }

    stage('Deploy') {
      steps {
        // Copie des fichiers vers le serveur web (/var/www/html/)
        echo 'building the app....'
        sh 'cp -r * /var/www/html/'
      }
    }

    stage('Test') {
      steps {
        // Vérification du déploiement
        sh ''''
            cd var/www/html/
            index.html || true
          '''
      }
    }
  }

  post {
    success {
      // Action en cas de succès (echo, ou autre)
      echo "Success !"
    }

    failure {
      // Action en cas d'échec
      echo "Failuret"
    }

    always {
      // Nettoyage
      // Supprimer les fichiers copiés dans /var/www/html
      echo 'clean up directory'
      sh 'rm -rf /var/www/html/* '
      // Desinstaller apache2
    }
  }
}
