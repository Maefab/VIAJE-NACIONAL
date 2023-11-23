# VIAJE-NACIONAL

 val viajeMonterrey=National("Monterrey")
    viajeMonterrey.quotePrice(2)
    viajeMonterrey.reserve(2)*/
    //lambda
    val saverGrade: (Double,Double) -> String = {expected: Double, saved: Double->
        val percentage = saved / expected
        when{
            percentage > 1 -> "Ahorrador pro"
            percentage == 1.0 -> "Buen ahorrador"
            percentage < 1.0 && percentage >= 0.8 -> "Almost"
            else -> "aprendiz saver"
        }
    }
    println(saverGrade(120.00,130.00))
    //anonima
    val saverGrade2 =fun(expected: Double,saved: Double): String {
        val percentage = saved / expected

        return when {
            percentage > 1 -> "Ahorrador pro"
            percentage == 1.0 -> "Buen ahorrador"
            percentage < 1.0 && percentage >= 0.8 -> "Almost"
            else -> "aprendiz saver"
        }
    }
    println(saverGrade(12.0,13.0))

    println(saverGrade(18.0,19.0))
}

    fun calculadora (a: Int, b: Int, operacion :(Int, Int) -> Int): Int{
        return operacion (a,b)
    }
    fun suma (a:Int, b:Int)= a+b
    fun resta(a:Int, b:Int)= a-b
    fun multiplicar(a:Int, b:Int) = a*b
    fun dividir(a:Int, b:Int) =a/b
    println(calculadora(5,4, ::suma))
    println(calculadora(5,4, ::resta))
    println(calculadora(5,4, ::multiplicar))
    println(calculadora(5,4, ::dividir))
}
    fun main() {
        val salvePrecio: (Double, String) -> Double = { precio, cupon ->
            val precioConIVA = (precio * .16)+precio
            when (cupon) {
                "noiva" -> precio
                "halfiva" -> (precio * .08)+precio
                "minus100" -> precio - 100.0
                "promo2020" -> {
                    val impuestos = when {
                        precio in 0.0..1000.0 -> (precio * 0.12)+precio
                        precio in 1000.0..2000.0 -> (precio * 0.04)+precio
                        precio in 2000.0..4000.0 -> (precio * .16)+precio
                        precio > 4000.0 -> precio * 2.0 / 3.0
                        else -> 0.0
                    }
                   impuestos
                }
                else -> precioConIVA
            }
        }

        val total = salvePrecio(20.0, "promo2020")
        println("Total a pagar: $$total")
    }

    class National(override val city: String): Travel() {
    override val country = "Mexico"
            protected val fees = mapOf(
                "Monterrey" to 400,
                "Guadalajara" to 350,
                "CDMX" to 360,
                "San Cristobal de las casas" to 240,
                "San Miguel de Allende" to 320
            )
    override fun getPrice(numDays:Int):Int{
        val fee=fees[city]
        return if (fee==null)0 else fee*numDays
    }

    override fun quotePrice(numDays: Int) {
    val price = getPrice(numDays)
    if (price==0) {
        println("Lo sentimos, no hacemos viajes a esta ciudad")
    }else{
        println("Tu viaje a $city cuesta \$$price")
    }
    }
}

abstract class Travel {

    abstract val country: String
    abstract val city: String
    protected val serviceType = "Viaje"
    protected var reserved = false
    protected var paidAmount = 0

    abstract  fun quotePrice(numDays: Int)
    protected abstract fun getPrice(numDays: Int):Int
    fun reserve(numDays: Int){
        if(reserved){
            println("""¡Ya reservaste tu viaje! 
                       País: $country
                       Ciudad: $city
                       Precio: $paidAmount""".trimMargin())
            return
        }
        val amount = getPrice(numDays)
        if(amount==0){
            return
        }
        reserved = true
        paidAmount = amount
        println("""¡Viaje reservado exitosamente! 
                       País: $country
                       Ciudad: $city
                       Precio: $paidAmount""".trimMargin())
    }
}
