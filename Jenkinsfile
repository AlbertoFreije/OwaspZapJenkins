import static groovy.json.JsonOutput.*

def mailBody = "Este es el informe de vulnerabilidades encontradas solicitado"
def mailSubject = "INFORME VULNERABILIDADES"
def mailFrom = 'AlbertoFreijeCarballo@gmail.com'
def mailTo = 'Alberto.Freije@Ricoh.es'
def nombreXML = "owasp-quick-scan-report.xml"

@NonCPS
def sluper(xmlData){
    def adocSource = "";
    def tablaAlerts = "";
    def tablaAlertsDetail = "";
    println(xmlData);
    def xmlContent = new XmlSlurper().parseText(xmlData)
    println("aqui2")
    adocSource += ":doctype: book\n"
    adocSource += ":hardbreaks:\n"
    adocSource += ":experimental:\n"
    adocSource += ":pdf-stylesdir: /tmp/workspace/pruebaNodo/templates-main/Ricoh \n"
    adocSource += ":pdf-style: template.yml \n"
    adocSource += ":Ruta_Base: ..\\ \n"
    adocSource += ":imagesdir: .\\ \n"
    adocSource += ":icons: image\n"
    adocSource += ":iconsdir: {pdf-stylesdir}\\iconos \n"
    adocSource += ":document_name: Informe ZAP \n"
    adocSource += ":version-label: Ver. \n"
    adocSource += ":revnumber: 1.0 \n"
    adocSource += ":revdate: {localdate} \n"
    adocSource += ":Titulo: OWASP ZAP \n"
    adocSource += ":Subtitulo: Generación de Informe de alertas de ZAP \n"
    adocSource += ":author: Ricoh España \n"
    adocSource += ":Subject: {Subtitulo} \n"
    adocSource += ":Keywords: devops,jenkins,docker,swarm \n"
    adocSource += ":email: alberto.freije@ricoh.es \n"
    adocSource += ":PDFFileName: Informe_Alertas_Zap_v{revnumber}.docx \n"
    adocSource += ":showtitle: \n"
    adocSource += ":toc: \n"
    adocSource += ":toc-title: Índice de contenidos \n"
    adocSource += ":toclevels: 6 \n"
    adocSource += ":sectnums: \n"
    adocSource += ":sectnumlevels: 6 \n"
    adocSource += ":chapter-label: \n"
    adocSource += ":checkedbox: pass:normal[&#9745;] \n"
    adocSource += ":uncheckedbox: pass:normal[&#9744;] \n"
    adocSource += ":SingleLeftArrow: pass:normal[&#8592;] \n"
    adocSource += ":SingleRightArrow: pass:normal[&#8594;] \n"
    adocSource += ":DoubleLeftArrow: pass:normal[&#8656;] \n"
    adocSource += ":DoubleRightArrow: pass:normal[&#8658;] \n"
    adocSource += ":WhiteSpace: pass:normal[&nbsp;] \n"
    adocSource += "= {Titulo} : {Subtitulo} \n"
    adocSource +="\n"
    adocSource += "[width=\"100%\",cols=\"10,11,30,15,34\",options=\"header\"]\n"
    adocSource += "|===\n"
    adocSource += "^.^| Versión ^.^| Fecha ^.^| Autor ^.^| Unidad - Departamento - Empresa ^.^| Observaciones\n"
    adocSource += "| {revnumber} | {revdate} | Alberto Freije Carballo | Arquitectura - Ricoh | Generación reporte de alertas Owasp Zap\n"
    adocSource += "|=== \n"
    adocSource += "== ZAP Scanning Report \n"
    adocSource += "* Sitio: " + xmlContent.site["@name"] + "\n"
    adocSource += "* Generado: " + xmlContent["@generated"] + "\n"
    adocSource +="\n"
    adocSource += "=== Alertas \n"
    adocSource += """[width=\"100%\",cols=\"60,30,10\",options=\"header\"]\n"""
    adocSource += "|===\n"
    adocSource += "^.^| Nombre ^.^| Nivel del riesgo ^.^| Numero de instancias \n"
    xmlContent.site.alerts.alertitem.each{
        e -> tablaAlerts += "| " + e.alert+ " | " + e.riskdesc + " | " + e.count + "\n";
    }
    adocSource += tablaAlerts
    adocSource += "|=== \n"
    adocSource += "=== Detalle Alertas \n"
    adocSource += """[width=\"100%\",cols=\"30,60\",options=\"header\"]\n"""
    adocSource += "|===\n"
    xmlContent.site.alerts.alertitem.each{
        e -> tablaAlertsDetail += "| " + e.riskdesc + " | " + e.name + "\n"
        tablaAlertsDetail += "| " + "Descripcion" + " | " + e.desc + "\n"
        e.instances.instance.each{
            a -> tablaAlertsDetail += "| " + "URL" + " | " + a.uri + "\n"
            tablaAlertsDetail += "| " + "Method" + " | " + a.method + "\n"
            tablaAlertsDetail += "| " + "Parameter" + " | " + a.param + "\n"
            tablaAlertsDetail += "| " + "Attack" + " | " + a.attack + "\n"
            tablaAlertsDetail += "| " + "Evidence" + " | " + a.evidence + "\n"
            
        }
        tablaAlertsDetail += "| " + "Solucion" + " | " + e.solution + "\n"
        tablaAlertsDetail += "| " + "Reference" + " | " + e.reference + "\n"
        
    }
    adocSource += tablaAlertsDetail
    adocSource += "|=== \n"
    adocSource += " \n"
    adocSource += " \n"
    adocSource += " \n"
    return adocSource
    
}

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
        }
        stage('Build') {
           stash name: 'prueba', includes: '**/owasp-quick-scan-report.xml'
        }
    }
    node("jmeter"){
       cleanWs()
       unstash 'prueba'
       sh("ls -la")
       sh("pwd") 
      //  def inputFile = input message: 'Upload file', parameters: [file(name: nombreXML)]
      //  writeFile(file: nombreXML, text: inputFile.readToString())
       println("aqui1")
       sh("ls -la")
       sh("pwd")
       def xmlContent = readFile( file: "${WORKSPACE}/" + nombreXML)
       def adocSource = sluper(xmlContent)
       writeFile(file: "informeAlertas.adoc", text: "${adocSource}")
       sh("wget https://github.com/AlbertoFreije/templates/archive/main.zip")
       sh("unzip main.zip")
       sh("ls -la")
       sh("pwd")
       sh("asciidoctor-pdf informeAlertas.adoc -o informeAlertas.pdf")
       emailext (
         attachmentsPattern: '**/informeAlertas.pdf',
         subject: mailSubject,
         body: mailBody,
         from: mailFrom,
         to: mailTo
       )
    }
   
