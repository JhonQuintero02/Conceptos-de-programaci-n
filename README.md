import java.io.FileWriter;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;
import java.util.Random;

public class GenerateInfoFiles {
    private static final String DIRECTORY = "data";
    private static final String SELLERS_FILE = DIRECTORY + "/sellers.txt";
    private static final String PRODUCTS_FILE = DIRECTORY + "/products.txt";
    private static final int MAX_PRODUCTS = 10;
    private static final int MAX_SALES = 5;
    private static final Random RANDOM = new Random();

    public static void main(String[] args) {
        try {
            Files.createDirectories(Paths.get(DIRECTORY));
            createSalesManInfoFile(5);
            createProductsFile(10);
            createSalesFiles(5);
            System.out.println("Archivos de prueba generados exitosamente.");
        } catch (IOException e) {
            System.err.println("Error al generar archivos: " + e.getMessage());
        }
    }

    public static void createSalesManInfoFile(int salesmanCount) throws IOException {
        try (FileWriter writer = new FileWriter(SELLERS_FILE)) {
            for (int i = 1; i <= salesmanCount; i++) {
                String line = "CC;" + (1000 + i) + ";Vendedor" + i + ";Apellido" + i;
                writer.write(line + "\n");
            }
        }
    }

    public static void createProductsFile(int productsCount) throws IOException {
        try (FileWriter writer = new FileWriter(PRODUCTS_FILE)) {
            for (int i = 1; i <= productsCount; i++) {
                String line = i + ";Producto" + i + ";" + (10 + RANDOM.nextInt(90));
                writer.write(line + "\n");
            }
        }
    }

    public static void createSalesFiles(int salesmanCount) throws IOException {
        List<String> products = Files.readAllLines(Paths.get(PRODUCTS_FILE));
        for (int i = 1; i <= salesmanCount; i++) {
            String salesFile = DIRECTORY + "/salesman_" + i + ".txt";
            try (FileWriter writer = new FileWriter(salesFile)) {
                writer.write("CC;" + (1000 + i) + "\n");
                for (int j = 0; j < RANDOM.nextInt(MAX_SALES) + 1; j++) {
                    String product = products.get(RANDOM.nextInt(products.size()));
                    String[] parts = product.split(";");
                    writer.write(parts[0] + ";" + (1 + RANDOM.nextInt(5)) + "\n");
                }
            }
        }
    }
}
