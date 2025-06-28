import com.tienda.excepciones.InventarioExcepcion; // Excepción personalizada
import com.tienda.modelo.Producto;
import java.util.HashMap;
import java.util.Map;

public class InventarioService {

    // Simulación de un almacén de productos en memoria
    private Map<String, Producto> productosEnInventario = new HashMap<>();

    public InventarioService() {
        // Inicializar con algunos productos de ejemplo
        productosEnInventario.put("PS001", new Producto("PS001", "Laptop Gamer", "Laptop de alta gama", 1200.0, 10));
        productosEnInventario.put("TV002", new Producto("TV002", "Smart TV 55\"", "Televisor 4K", 800.0, 15));
    }

    /**
     * Actualiza el stock de un producto dado su código y la cantidad a añadir/quitar.
     * @param codigoProducto El código único del producto.
     * @param cantidad La cantidad a ajustar (positiva para añadir, negativa para quitar).
     * @throws InventarioExcepcion Si el producto no existe o la cantidad es inválida.
     */
    public void actualizarStock(String codigoProducto, int cantidad) throws InventarioExcepcion {
        // 1. Validación de entrada
        if (codigoProducto == null || codigoProducto.trim().isEmpty()) {
            throw new IllegalArgumentException("El código del producto no puede ser nulo o vacío.");
        }

        // 2. Manejo de errores con try-catch y excepción personalizada
        try {
            Producto producto = productosEnInventario.get(codigoProducto);

            if (producto == null) {
                throw new InventarioExcepcion("Producto con código " + codigoProducto + " no encontrado.");
            }

            // Validar que la cantidad no sea negativa o que el stock no se vuelva negativo
            if (cantidad < 0 && (producto.getStock() + cantidad) < 0) {
                throw new InventarioExcepcion("No hay suficiente stock para la operación. Stock actual: " + producto.getStock());
            }

            // Actualizar stock
            producto.setStock(producto.getStock() + cantidad);
            System.out.println("Stock actualizado para " + producto.getNombre() + ". Nuevo stock: " + producto.getStock());

        } catch (InventarioExcepcion e) {
            // Captura nuestra excepción personalizada
            System.err.println("Error en el inventario: " + e.getMessage());
            throw e; // Relanza la excepción para que sea manejada a un nivel superior si es necesario
        } catch (Exception e) {
            // Captura cualquier otra excepción inesperada
            System.err.println("Ocurrió un error inesperado al actualizar el stock: " + e.getMessage());
            throw new InventarioExcepcion("Error desconocido al actualizar el stock.", e); // Envuelve en nuestra excepción
        }
    }
}
