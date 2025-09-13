# Tienda-de-Ana

string[] productos = new string[4] { "galletas", "chicles", "barriletes", "alfajores" };
double[] precios = new double[4] { 2000, 500, 800, 3000 };
int[] stock = new int[4] { 10, 10, 10, 10 };

Console.WriteLine("-----------------------------------");
Console.WriteLine("---Bienvenido a la tienda de Ana---");
Console.WriteLine("-----------------------------------");
Console.WriteLine("Este es nuestro inventario disponible:");
Console.WriteLine("-----------------------------------");

for (int i = 0; i < 4; i++)
{
    Console.WriteLine($"{productos[i]} | Precio: ${precios[i]} | Stock: {stock[i]}");
}

bool seguirComprando = true;
double totalCompra = 0;

// arrays para el historial
string[] productosComprados = new string[10];
int[] cantidadesCompradas = new int[10];
double[] subtotalesComprados = new double[10];
int contadorCompras = 0; //contador para saber cuántas compras se han hecho

do
{
    Console.WriteLine("\n¿Que producto deseas comprar? (escribe 'no' para terminar)");
    string productoElegido = Console.ReadLine().ToLower().Trim();

    if (productoElegido == "no")
    {
        seguirComprando = false;
        continue; // Continúa al final del ciclo para terminar
    }

    bool productoEncontrado = false;
    for (int i = 0; i < productos.Length; i++)
    {
        if (productoElegido == productos[i].ToLower())
        {
            productoEncontrado = true;
            Console.WriteLine($"¿Cuantas unidades de {productos[i]} deseas llevar?");
            
            int cantidadDeseada = int.Parse(Console.ReadLine());
            
            if (cantidadDeseada > 0 && cantidadDeseada <= stock[i])
            {
                // Actualizar el stock
                stock[i] -= cantidadDeseada;

                // Calcular el subtotal y sumarlo al total general
                double subtotal = precios[i] * cantidadDeseada;
                totalCompra += subtotal;

                // Guardar la compra en los arrays de historial
                productosComprados[contadorCompras] = productos[i];
                cantidadesCompradas[contadorCompras] = cantidadDeseada;
                subtotalesComprados[contadorCompras] = subtotal;
                contadorCompras++;

                Console.WriteLine($"{cantidadDeseada} {productos[i]} añadidos al carrito");
            }
            else
            {
                Console.WriteLine($"La cantidad solicitada ({cantidadDeseada}) es inválida o excede el stock disponible.");
            }
            break; // Salimos del for una vez que el producto es procesado
        }
    }

    if (!productoEncontrado && productoElegido != "no")
    {
        Console.WriteLine($"Lo siento, el producto '{productoElegido}' no existe en nuestro inventario.");
    }
} while (seguirComprando);

// --- Aplicar descuentos y mostrar el ticket ---
double descuento = 0;
string mensajeDescuento = "No se aplicó descuento.";
double totalFinal = totalCompra;

if (totalCompra > 20000)
{
    descuento = 0.20; // 20% de descuento
    totalFinal = totalCompra * (1 - descuento);
    mensajeDescuento = "Se aplicó un 20% de descuento.";
}
else if (totalCompra > 10000)
{
    descuento = 0.10; // 10% de descuento
    totalFinal = totalCompra * (1 - descuento);
    mensajeDescuento = "Se aplicó un 10% de descuento.";
}

Console.WriteLine("\n-----------------------------------");
Console.WriteLine("---      TICKET DE COMPRA       ---");
Console.WriteLine("-----------------------------------");

// Usamos el contador para saber cuantas compras debemos mostrar
if (contadorCompras > 0)
{
    for (int i = 0; i < contadorCompras; i++)
    {
        Console.WriteLine($"{cantidadesCompradas[i]} x {productosComprados[i]} -> ${subtotalesComprados[i]}");
    }
}
else
{
    Console.WriteLine("No se realizaron compras.");
}

Console.WriteLine($"\nSubtotal: ${totalCompra}");
Console.WriteLine($"{mensajeDescuento}");
Console.WriteLine($"Total a pagar: ${totalFinal}");
Console.WriteLine("\n¡Gracias por comprar en la tienda de Ana!");
Console.WriteLine("-----------------------------------");
