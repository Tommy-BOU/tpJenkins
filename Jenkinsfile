pipeline {
    agent {
        label 'newNode'
    }

    stages {

        stage("Dependances") {
            steps {
                echo "Installation d'Apache2..."
                sh '''
                    sudo apt-get update -y
                    sudo apt-get install -y apache2
                    sudo systemctl start apache2
                    sudo systemctl enable apache2
                '''
            }
        }

        stage('Checkout') {
            steps {
                echo "Récupération du code source..."
                // Exemple avec Git : adapte l’URL selon ton dépôt
                git branch: 'main', url: 'https://github.com/Tommy-BOU/tpJenkins'
            }
        }

        stage('Deploy') {
            steps {
                echo "Déploiement de l’application web statique..."
                sh '''
                    sudo rm -rf /var/www/html/*
                    sudo cp -r * /var/www/html/
                    cat index.html
                    sudo chown -R www-data:www-data /var/www/html
                    sudo systemctl restart apache2
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Vérification du déploiement..."
                sh '''
                    STATUS_CODE=$(curl -o /dev/null -s -w "%{http_code}" http://localhost)
                    if [ "$STATUS_CODE" -ne 200 ]; then
                        echo "Le site ne répond pas correctement (code: $STATUS_CODE)"
                        exit 1
                    else
                        echo "Le site est déployé et accessible (HTTP 200)"
                        
                    fi
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Déploiement réussi !"
        }
        failure {
            echo "❌ Échec du pipeline. Vérifiez les logs."
        }
        always {
            echo "Nettoyage du serveur..."
            sh '''
                sudo rm -rf /var/www/html/*
                sudo apt-get remove -y apache2
                sudo apt-get autoremove -y
            '''
        }
    }
}
