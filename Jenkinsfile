// pipeline {
//     agent { label 'Jenkins-Agent' }

//     tools {
//         jdk 'Java 17'
//         maven 'Maven3'
//     }

//     environment {
//         APP_NAME = "register-app-pipeline"
//         RELEASE = "1.0.0"
//         DOCKER_USER = "dhundhara"
//         DOCKER_CREDENTIALS = 'dockerhub' // Jenkins credentials ID for DockerHub
//         IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
//         IMAGE_TAG = "${RELEASE}-${env.BUILD_NUMBER}" // Use env.BUILD_NUMBER for proper runtime evaluation
//     }

//     stages {
//         stage("Cleanup Workspace") {
//             steps {
//                 cleanWs()
//             }
//         }

//         stage("Checkout from SCM") {
//             steps {
//                 git branch: 'main', credentialsId: 'github', url: 'https://github.com/rakeshdhundhara/register-app'
//             }
//         }

//         stage("Build Application") {
//             steps {
//                 sh "mvn clean package"
//             }
//         }

//         stage("Test Application") {
//             steps {
//                 sh "mvn test"
//             }
//         }

//         stage("SonarQube Analysis") {
//             steps {
//                 script {
//                     withSonarQubeEnv('Jenkins-sonarqube-token') { 
//                         sh "mvn sonar:sonar"
//                     }
//                 }    
//             }
//         }

//         stage("Build & Push Docker Image") {
//             steps {
//                 script {
//                     // Login to DockerHub and build the image
//                     docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
//                         def docker_image = docker.build("${IMAGE_NAME}:${IMAGE_TAG}") // Build image with versioned tag
//                         docker_image.push("${IMAGE_TAG}") // Push with versioned tag
//                         docker_image.push("latest") // Push with 'latest' tag
//                     }
//                 }
//             }
//         }
//     }
// }
pipeline {
    agent { label 'Jenkins-Agent' }

    tools {
        jdk 'Java 17'
        maven 'Maven3'
    }

    environment {
        APP_NAME = "register-app-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "dhundhara"
        DOCKER_CREDENTIALS= 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/rakeshdhundhara/register-app'
            }
        }

        stage("Build Application") {
            steps {
                sh "mvn clean package"
            }
        }

        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }

        stage("SonarQube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId:'Jenkins-sonarqube-token') { 
                        sh "mvn sonar:sonar"
                    }
                }    
            }
        }

       

        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    
                    docker.withRegistry('', DOCKER_CREDENTIALS) 
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}
