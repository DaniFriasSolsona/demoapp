import groovy.json.JsonSlurper

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
                    out = callAPI("GET", "https://apidatos.ree.es/es/datos/demanda/evolucion?start_date=2022-09-01\\&end_date=2022-09-02\\&time_trunc=day", "")
                    echo "${ out }"
                }
            }
        }
        stage("Clases"){
            steps{
                script{
                    //Llamada a la API
                    def jsonString = callAPI("GET", "https://api.github.com/repos/MartiMarch/formacion-jenkins-groovy", "")
                    //Transforamcion del json a un mapa
                    def jsonObj = new JsonSlurper()
                    def json = jsonObj.parseText(jsonString)

                    def id = json.id
                    def name = json.name
                    def url = json.html_url
                    def visibility = json.visibility
                    def description = String.valueOf(json.description)
                    def forks = json.forks
                    Repositorio repo = new Repositorio(id, name, url, visibility, description, forks)
                    repo.print()
                }
            }
        }
    }
}

String callAPI(String call, String parameters, String json){
    def command = "curl -X \'" + call + "\' " + parameters
    if(json.length() > 0){
        command += "-H \'Content-Type: application/json\' -d ${ json }"
    }
    def out = sh (returnStdout: true, script: "${ command }")
    return out
}

class Repositorio{
    private String id = ""
    private String name = ""
    private String url = ""
    private String visibility = ""
    private String description = ""
    private String forks = ""

    public Repositorio(String id, String name, String url, String visibility, String description, String forks){
        this.id = id;
        this.name = name;
        this.url = url;
        this.visibility = visibility;
        if(description.equals("public")){
            this.description = "publico";
        }else{
            this.description = "privado";
        }
        this.forks = forks;
    }

    public String print(){
        String out = "";
        out += "Datos del repositorio\n"
        out += "Nombre: " + this.name + "\n"
        out += "Id: " + this.id + "\n"
        out += "Description: " + this.description + "\n"
        out += "URL: " + this.url + "\n"
        out += "Visibilidad:" + this.visibility + "\n"
        out+= "Cantidad de forks: " + this.forks + "\n"
        return out;
    }
}