#### Example-1:

    println "From pipeline job"

    echo "From pipeline job"

#### Example-2: Run the other jobs uisng build command

    build "Job-1"
    
    build job: 'Job-1', parameters: [string(name: 'version', value: '1.0.${BUILD_NUMBER}')]
    
    build job: 'Job-1', parameters: [string(name: 'version', value: '2.0.0')]
    
#### Example-3: Run the stages parallelly

    parallel 'stage-1': {
            stage("job-1"){
                build "Job-1"
                }
            },
            'stage-2': {
            stage("job-2"){
                build "Job-2"
                }
            }

#### Example-4: Checkout/clone the code + Build

    node{
        git branch: 'web', credentialsId: 'jengit', url: 'https://github.com/venkatasykam/DevOpsWebApp.git'

        withEnv(["PATH+MAVEN=${tool 'maven-3.5.4'}/bin"]) {
            sh "mvn clean package"
        }
    }
    
#### Example-5: Checkout/clone the code + Build + git tag

    node{

        GIT_BRANCH = "web"

        git branch: "${GIT_BRANCH}", credentialsId: 'jengit', url: 'https://github.com/venkatasykam/DevOpsWebApp.git'

        tool name: 'maven-3.5.4', type: 'maven'
        withEnv(["PATH+MAVEN=${tool 'maven-3.5.4'}/bin"]) {

            sh "mvn clean package"

        }

        withCredentials([usernamePassword(credentialsId: 'jengit', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
            sh("git tag -a ${GIT_BRANCH}_DevOpsWebApp_${BUILD_NUMBER} -m 'Jenkins'")
            sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/venkatasykam/DevOpsWebApp.git --tags')
        }

    }
