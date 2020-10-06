node {
    def tasks = tasks(readFile('bigtop.bom'))
    echo "TASKS $tasks"
}

@NonCPS
def tasks(content) {
    return new ConfigSlurper().parse(content).bigtop.components.collectEntries { name, component ->
        ["$name" : [ completed: false, requires: component.requires as ArrayList ]]
    }
}
