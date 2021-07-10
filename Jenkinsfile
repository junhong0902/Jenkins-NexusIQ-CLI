pipeline {
    agent any
    
    stages {
        stage('Sonatype CLI Preparation') {
            steps {
                script {
                    if (fileExists('nexus-iq-cli')) {
                        echo 'Skip download CLI'
                    } else {
                        echo 'Downloading CLI'
                        withCredentials([usernamePassword(credentialsId: 'nexus-repo-admin', passwordVariable: 'pass', usernameVariable: 'user')]) {
                            sh 'wget -q --no-check-certificate --user "${user}" --password "${pass}" "http://ip-10-60-1-86.ap-southeast-1.compute.internal:8081/repository/fileserver/sonatype/nexusiq/nexus-iq-cli"'
                            sh 'chmod +x nexus-iq-cli'
                        }
                    }
                }
            }
        }
        stage('Nuget Project Preparation') {
            steps {
                script {
                    if (fileExists('cyberark.csproj')) {
                        echo 'Skip download projects'
                    } else {
                        echo 'Downloading projects'
                        withCredentials([usernamePassword(credentialsId: 'nexus-repo-admin', passwordVariable: 'pass', usernameVariable: 'user')]) {
                            sh 'wget -q --no-check-certificate --user "${user}" --password "${pass}" "http://ip-10-60-1-86.ap-southeast-1.compute.internal:8081/repository/fileserver/devops/sampleproject/nuget/cyberarkticketing/cyberark.csproj"'
                            sh 'wget -q --no-check-certificate --user "${user}" --password "${pass}" "http://ip-10-60-1-86.ap-southeast-1.compute.internal:8081/repository/fileserver/devops/sampleproject/nuget/cyberarkticketing/packages.config"'
                        }
                    }
                }
            }
        }
        stage('Sonatype Scan') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'nexus-iq-admin', passwordVariable: 'pass', usernameVariable: 'user')]) {
                            sh './nexus-iq-cli -a "${user}:${pass}" -i NuGet-Sample1 -s "https://nexusiq.nssg.jhdomain.com/" -t "build" "cyberark.csproj" "packages.config" '
                        }
                }
            }
        }
    }
    post {
        cleanup{
            deleteDir()
        }
}
}
