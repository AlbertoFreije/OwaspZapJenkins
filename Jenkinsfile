node("jmeter"){
    cleanWs()
    def inputFile = input message: 'Upload file', parameters: [file(name: 'jmetertest.jmx')]
    writeFile(file: 'jmetertest.jmx', text: inputFile.readToString())
    //new hudson.FilePath(new File("$workspace/jmetertest.jmx")).copyFrom(inputFile)
    //inputFile.delete()
    echo fileExists('jmetertest.jmx').toString()
    println("${workspace}")
    sh("pwd")
    sh("ls -la")
    sh "jmeter -Dhttp.proxyHost=192.168.56.10 -Dhttp.proxyPort=8092 -Dhttps.proxyHost=192.168.56.10 -Dhttps.proxyPort=8092 -n -t /tmp/workspace/pruebaNodo/jmetertest.jmx -l result.jtl"
}
