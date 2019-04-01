#### Example-1:

    println "From pipeline job"

    echo "From pipeline job"

#### Example-2:

    build "Job-1"
    
    build job: 'Job-1', parameters: [string(name: 'version', value: '1.0.${BUILD_NUMBER}')]
    
    build job: 'Job-1', parameters: [string(name: 'version', value: '2.0.0')]
    
#### Example-3:

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
