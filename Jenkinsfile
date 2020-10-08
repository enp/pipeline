node {
    stage ('load') {
        tasks = tasks(readFile('bigtop.bom'))
    }
    stage ('exec') {
        parallel tasks.collectEntries { name, task ->
            [(name) : { -> return {
                    task.requires.each { req ->
                        stage ("wait[$req]") {
                            waitUntil { tasks[req].completed }
                        }
                    }
                    stage ('exec') {
                        sh "echo $name && sleep 1"
                        task.completed = true
                    }
                }
            }(name)]
        }
    }
}

@NonCPS
def tasks(content) {
    return new ConfigSlurper().parse(content).bigtop.components.collectEntries { name, component ->
        [(name) : [ completed: false, requires: component.requires != null ? component.requires as ArrayList : []]]
    }
}
