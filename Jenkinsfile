pipeline{
    agent any
    environment {
        GLOBAL_1 = 3.141592653
    }
    stages{
        stage("Groovy"){
            steps{
                script{

                    //Groovy
                    def g_num1 = 3
                    def g_num2 = 7
                    echo "Suma usando groovy: ${ g_num1 + g_num2 }"

                    //Java
                    int j_num1 = 3
                    int j_num2 = 7
                    echo "Suma usando java: ${ j_num1 + j_num2 }"

                    //Groovy
                    def g_randomStrings = []
                    g_randomStrings.add("coche")
                    g_randomStrings.add("puerta")
                    g_randomStrings.add("teclado")
                    g_randomStrings.add("letra")
                    echo "Loop sobre el \"array\" usando Groovy:"
                    g_randomStrings.each { echo "${ it }" }

                    //Java
                    ArrayList<String> j_randomStrings = new ArrayList()
                    j_randomStrings.add("coche")
                    j_randomStrings.add("puerta")
                    j_randomStrings.add("teclado")
                    j_randomStrings.add("letra")
                    echo "Loop sobre el array usando Java:"
                    for(int i = 0; i < j_randomStrings.size(); ++i)
                    {
                        echo "${ j_randomStrings.get(i) }"
                    }

                }
            }
        }
        stage("Variables globales"){
            environment {
                GLOBAL2 = 12450
            }
            steps{
                script{
                    echo "Cualquier stage puede imprimir esta variable ${ GLOBAL_1 }"
                    echo "Solo el stage \"Variables globales\" puede imprimir la variable ${ GLOBAL2 } "
                }
            }
        }
        stage("Paralelización"){
            steps{
                parallel(
                    a: {
                        echo "Tarea a"
                    },
                    b: {
                        echo "Tarea b"
                    },
                    c: {
                        echo "Tarea c"
                    }
                )
            }
        }
        stage("SH"){
            steps{
                script{
                    //Imprimiendo cual es mi SO
                    sh "cat /etc/os-release"

                    //Descargando e imprimiendo el Jenknsfile
                    sh "wget -nc https://raw.githubusercontent.com/MartiMarch/formacion-jenkins-groovy/master/Jenkinsfile"
                    def output = sh (returnStdout: true, script: "cat Jenkinsfile")
                    echo "${ output }"
                }
            }
        }
        stage("WORKSPACE"){
            steps{
                script{
                    sh "pwd"
                    echo "${ WORKSPACE }"
                    sh "ls -ll ${ WORKSPACE }"
                }
            }
        }
        stage("Parametrización"){
            steps{
                script{
                    def MI_CADENA = 
                        input message: 'Introduce una cadena de texto:', 
                        parameters: [string(
                            defaultValue: 'change me',
                            name: 'MI_CADENA',
                            trim: false
                        )]
                    echo "${ MI_CADENA }"
                }
            }
        }
        stage("Funciones"){
            steps{
                script{
                    def out = callAPI("GET", "https://api.github.com/repos/MartiMarch/formacion-jenkins-groovy", "")
                    echo "${ out }"
                    out = callAPI("GET", "https://apidatos.ree.es/es/datos/demanda/evolucion?start_date=2022-09-01&end_date=2022-09-02&time_trunc=day", "")
                    echo "${ out }"
                }
            }
        }
    }
}

String callAPI(String call, String parameters, String json){
    def command = "curl -X \"" + call + "\" " + parameters
    if(json.length() > 0){
        command += "-H \'Content-Type: application/json\' -d ${ json }"
    }
    def out = sh (returnStdout: true, script: "${ command }")
    return out
}
