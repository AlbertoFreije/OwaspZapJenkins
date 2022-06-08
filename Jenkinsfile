import static groovy.json.JsonOutput.*

def mailBody = "Este es el informe de vulnerabilidades encontradas solicitado"
def mailSubject = "INFORME VULNERABILIDADES"
def mailFrom = 'AlbertoFreijeCarballo@gmail.com'
def mailTo = 'Alberto.Freije@Ricoh.es'
node("jmeter"){
      stage('Jmeter-Zap') {
             cleanWs()
             booleanParam defaultValue: true, name: 'GENERATE_REPORT'
             def inputFile = input message: 'Upload file', parameters: [file(name: 'jmetertest.jmx')]
             writeFile(file: 'jmetertest.jmx', text: inputFile.readToString())
             echo fileExists('jmetertest.jmx').toString()
             println("${workspace}")
             sh("pwd")
             sh("ls -la")
             sh "jmeter -Dhttp.proxyHost=192.168.56.10 -Dhttp.proxyPort=8092 -Dhttps.proxyHost=192.168.56.10 -Dhttps.proxyPort=8092 -n -t /tmp/workspace/pruebaNodo/jmetertest.jmx -l result.jtl"
        }
    }
    node("jenkinszap"){
        stage('Generacion Informe') {
            sh("pwd")
            sh("zap-cli --verbose  --api-key change-me-9203935709 -p 8090 report -o /zap/workspace/pruebaNodo/owasp-quick-scan-report.xml --output-format xml")
            sh("ls -la")
                 
            // emailext (
            //     attachmentsPattern: '**/owasp-quick-scan-report.xml',
            //     subject: mailSubject,
            //     body: mailBody,
            //     from: mailFrom,
            //     to: mailTo
            // )
        }
        stage('Build') {
           stash name: 'prueba', includes: '**/owasp-quick-scan-report.xml'
        }
    }
    node("jmeter"){
        unstash 'prueba'
        sh("ls -la")
        def file = readFile "owasp-quick-scan-report.xml"
        def xml = new XmlParser().parseText(file)
        echo "${xml}"
      //   xml.each{
      //      item -> item.each{

      //         item2 -> item2.each{
      //             item3 -> println("$item3.key")
      //         }
                 
              
      //      }
      //    }

      
      //print prettyPrint(toJson(xml))

      // xml.each{ key, val -> 

      //    println "Imprimiendo: $key variable $val"

      // }
           
        }
   
