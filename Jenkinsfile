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
    }
}
