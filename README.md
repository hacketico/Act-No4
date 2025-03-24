// Clase Abstracta Producto
abstract class Producto {
    private String nombre;       // Nombre del producto (ej: "Tacos al pastor")
    private double precio;      // Precio en pesos mexicanos (MXN)
    private int cantidad;       // Cantidad en stock

    // Constructor
    public Producto(String nombre, double precio, int cantidad) {
        this.nombre = nombre;
        this.precio = precio;
        this.cantidad = cantidad;
    }

    // Métodos getters
    public String getNombre() {
        return nombre;
    }

    public double getPrecio() {
        return precio;
    }

    public int getCantidad() {
        return cantidad;
    }

    // Método setter para la cantidad
    public void setCantidad(int cantidad) {
        this.cantidad = cantidad;
    }

    // Método abstracto que debe ser implementado por las clases hijas
    public abstract void mostrarDetalles();
}

// Interfaz Inventariable
interface Inventariable {
    void agregarStock(int cantidad);   // Método para agregar stock
    void reducirStock(int cantidad);   // Método para reducir stock
}

// Clase Concreta Plato
class Plato extends Producto implements Inventariable {
    private String tipo;  // Tipo de plato (ej: "Entrada", "Plato fuerte", "Postre")

    // Constructor
    public Plato(String nombre, double precio, int cantidad, String tipo) {
        super(nombre, precio, cantidad);
        this.tipo = tipo;
    }

    // Implementación del método abstracto
    @Override
    public void mostrarDetalles() {
        System.out.println("Plato: " + getNombre() + ", Tipo: " + tipo + ", Precio: $" + getPrecio() + " MXN, Cantidad: " + getCantidad());
    }

    // Implementación de los métodos de la interfaz
    @Override
    public void agregarStock(int cantidad) {
        setCantidad(getCantidad() + cantidad);
        System.out.println("Stock agregado. Nueva cantidad de " + getNombre() + ": " + getCantidad());
    }

    @Override
    public void reducirStock(int cantidad) {
        if (getCantidad() >= cantidad) {
            setCantidad(getCantidad() - cantidad);
            System.out.println("Stock reducido. Nueva cantidad de " + getNombre() + ": " + getCantidad());
        } else {
            System.out.println("No hay suficiente stock de " + getNombre() + " para reducir.");
        }
    }
}

// Clase Concreta Bebida
class Bebida extends Producto implements Inventariable {
    private boolean alcoholica;  // Indica si la bebida es alcohólica

    // Constructor
    public Bebida(String nombre, double precio, int cantidad, boolean alcoholica) {
        super(nombre, precio, cantidad);
        this.alcoholica = alcoholica;
    }

    // Implementación del método abstracto
    @Override
    public void mostrarDetalles() {
        String tipo = alcoholica ? "Alcohólica" : "No alcohólica";
        System.out.println("Bebida: " + getNombre() + ", Tipo: " + tipo + ", Precio: $" + getPrecio() + " MXN, Cantidad: " + getCantidad());
    }

    // Implementación de los métodos de la interfaz
    @Override
    public void agregarStock(int cantidad) {
        setCantidad(getCantidad() + cantidad);
        System.out.println("Stock agregado. Nueva cantidad de " + getNombre() + ": " + getCantidad());
    }

    @Override
    public void reducirStock(int cantidad) {
        if (getCantidad() >= cantidad) {
            setCantidad(getCantidad() - cantidad);
            System.out.println("Stock reducido. Nueva cantidad de " + getNombre() + ": " + getCantidad());
        } else {
            System.out.println("No hay suficiente stock de " + getNombre() + " para reducir.");
        }
    }
}

// Clase Mesero
class Mesero {
    private String nombre;

    // Constructor
    public Mesero(String nombre) {
        this.nombre = nombre;
    }

    // Método para gestionar el stock de un producto
    public void gestionarStock(Inventariable producto, int cantidad, boolean esAgregar) {
        System.out.println("Mesero " + nombre + " está gestionando el stock...");
        if (esAgregar) {
            producto.agregarStock(cantidad);
        } else {
            producto.reducirStock(cantidad);
        }
    }
}

// Clase Principal para probar el sistema
public class RestauranteApp {
    public static void main(String[] args) {
        // Crear productos
        Plato tacos = new Plato("Tacos al pastor", 120.50, 10, "Plato fuerte");
        Bebida refresco = new Bebida("Refresco de cola", 25.00, 50, false);

        // Mostrar detalles de los productos
        tacos.mostrarDetalles();
        refresco.mostrarDetalles();

        // Crear un mesero
        Mesero meseroJuan = new Mesero("Juan");

        // Gestionar el stock
        meseroJuan.gestionarStock(tacos, 5, true);   // Agregar 5 unidades de tacos
        meseroJuan.gestionarStock(refresco, 10, false); // Reducir 10 unidades de refresco

        // Mostrar detalles actualizados
        tacos.mostrarDetalles();
        refresco.mostrarDetalles();
    }
}
