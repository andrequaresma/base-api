pipeline {
    agent any // Pode rodar em qualquer agente disponível

    tools {
        // Se você quiser usar um Maven ou JDK específico instalado no agente,
        // pode configurá-los em "Manage Jenkins" -> "Tools"
        // maven 'M3' // Substitua 'M3' pelo nome da sua instalação Maven no Jenkins
        // jdk 'JDK_17' // Substitua 'JDK_17' pelo nome da sua instalação JDK no Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // O Jenkins já faz o checkout automaticamente ao usar "Pipeline script from SCM"
                // Mas você pode adicionar um 'git checkout' explícito se precisar de mais controle
                echo 'Checking out code...'
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Comando Maven para limpar, compilar e executar testes
                    // Se você usa Gradle, o comando seria './gradlew clean build'
                    sh 'mvn clean install -DskipTests' // Compila e instala, pulando testes por enquanto para agilizar
                    sh 'mvn test' // Executa os testes unitários
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Construir a imagem Docker. Assumindo que você tem um Dockerfile na raiz.
                    // Substitua 'sua-conta-dockerhub' pelo seu nome de usuário Docker Hub ou registry local
                    // e 'seu-app-quarkus' pelo nome da sua imagem.
                    // O tag 'latest' é para testes; em produção, use um tag baseado na versão/commit
                    def appImage = "dedevquaresma/base-api:${env.BUILD_NUMBER}"
                    sh "docker build -t ${appImage} ."
                    sh "docker push ${appImage}"
                }
            }
        }
    }

    post {
        always {
            // Ações que sempre acontecem após a pipeline (sucesso ou falha)
            echo 'Pipeline finished.'
            cleanWs() // Limpa o workspace do Jenkins
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
