node {
    def tasks = tasks(readFile('bigtop.bom'))
    echo "TASKS $tasks"
    parallel tasks.collectEntries { name, task ->
        [(name) : { -> return {
                echo "PREPARE $name"
                task.requires.each { req ->
                    waitUntil { tasks[req].completed }
                }
                echo "BEGIN $name"
                sh script: "sleep 1"
                task.completed = true
                echo "END $name"
            }
        }(name)]
    }
}

@NonCPS
def tasks(content) {
    return new ConfigSlurper().parse(content).bigtop.components.collectEntries { name, component ->
        [(name) : [ completed: false, requires: component.requires != null ? component.requires as ArrayList : []]]
    }
}
