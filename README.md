public class Main {
    private static BookCatalog catalog = new BookCatalog();
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        while (true) {
            System.out.println("1. Buscar libros por título");
            System.out.println("2. Listar detalles de un libro específico");
            System.out.println("3. Mostrar todos los libros guardados en el catálogo");
            System.out.println("4. Agregar un libro al catálogo");
            System.out.println("5. Eliminar un libro del catálogo");
            System.out.println("6. Salir");
            System.out.print("Elija una opción: ");
            int option = scanner.nextInt();
            scanner.nextLine();  // Consumir nueva línea

            switch (option) {
                case 1:
                    searchBooksByTitle();
                    break;
                case 2:
                    listBookDetails();
                    break;
                case 3:
                    showAllBooks();
                    break;
                case 4:
                    addBookToCatalog();
                    break;
                case 5:
                    removeBookFromCatalog();
                    break;
                case 6:
                    System.out.println("Saliendo...");
                    return;
                default:
                    System.out.println("Opción no válida.");
            }
        }
    }

    private static void searchBooksByTitle() {
        System.out.print("Ingrese el título del libro: ");
        String title = scanner.nextLine();
        try {
            String url = "https://openlibrary.org/search.json?title=" + title.replace(" ", "%20");
            HttpURLConnection conn = (HttpURLConnection) new URL(url).openConnection();
            conn.setRequestMethod("GET");
            BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            String inputLine;
            StringBuilder content = new StringBuilder();
            while ((inputLine = in.readLine()) != null) {
                content.append(inputLine);
            }
            in.close();
            conn.disconnect();
            System.out.println("Resultados: " + content.toString());
            // Aquí deberíamos parsear el JSON y mostrar los resultados de forma amigable
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static void listBookDetails() {
        System.out.print("Ingrese el ID del libro: ");
        String id = scanner.nextLine();
        Book book = catalog.getBookById(id);
        if (book != null) {
            System.out.println(book);
        } else {
            System.out.println("Libro no encontrado.");
        }
    }

    private static void showAllBooks() {
        List<Book> books = catalog.getAllBooks();
        for (Book book : books) {
            System.out.println(book);
        }
    }

    private static void addBookToCatalog() {
        System.out.print("Ingrese el título del libro: ");
        String title = scanner.nextLine();
        System.out.print("Ingrese el autor del libro: ");
        String author = scanner.nextLine();
        System.out.print("Ingrese el ID del libro: ");
        String id = scanner.nextLine();
        Book book = new Book(title, author, id);
        catalog.addBook(book);
        System.out.println("Libro agregado al catálogo.");
    }

    private static void removeBookFromCatalog() {
        System.out.print("Ingrese el ID del libro a eliminar: ");
        String id = scanner.nextLine();
        catalog.removeBook(id);
        System.out.println("Libro eliminado del catálogo.");
    }
}
