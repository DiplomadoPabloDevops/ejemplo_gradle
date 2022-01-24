pipeline {
    agent any
    parameters {
        choice choices: ['gradle', 'maven'], description: 'Indicar herramienta de construcción', name: 'buildTool'
    }   
    stages {
        stage('Pipeline') {
            steps {
                script {
                        println('Pipeline')
                        println params.buildTool
                        if (params.buildTool == 'gradle') {
                            def ejecucion = load 'gradle.groovy'
                             ejecucion.call()
                        }
                        else{
                            def ejecucion = load 'maven.groovy'
                            ejecucion.call()
                        }     
                }
            }
        }
    }
}