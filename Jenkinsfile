def scan_type
def target
// node("jmeter"){
//          cleanWs()
//          booleanParam defaultValue: true, name: 'GENERATE_REPORT'
//          def inputFile = input message: 'Upload file', parameters: [file(name: 'jmetertest.jmx')]
//          writeFile(file: 'jmetertest.jmx', text: inputFile.readToString())
//          //new hudson.FilePath(new File("$workspace/jmetertest.jmx")).copyFrom(inputFile)
//          //inputFile.delete()
//          echo fileExists('jmetertest.jmx').toString()
//          println("${workspace}")
//          sh("pwd")
//          sh("ls -la")
//          //inputFile = input message: 'Upload file', parameters: [file(name: 'jmetertest.jmx')]
//         //  when {
//         //         environment name : 'GENERATE_REPORT', value: 'true'
//         //   }
//         //   steps {
//         //          script {
//         //              //determined_blackwell nombre del contenedor OWASP
//         //              sh "docker exec determined_blackwell"
//         //              //sh "mkdir /zap/wrk"
//         //                 //  sh """
//         //                 //      docker exec determined_blackwell \ 
//         //                 //      mkdir /zap/wrk
//         //                 //  """
//         //              }
//         //    }
//          sh "jmeter -Dhttp.proxyHost=192.168.56.10 -Dhttp.proxyPort=8092 -Dhttps.proxyHost=192.168.56.10 -Dhttps.proxyPort=8092 -n -t /tmp/workspace/pruebaNodo/jmetertest.jmx -l result.jtl"
// }
node("built-in"){

     sh "docker exec determined_blackwell"

}