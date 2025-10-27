 import java.io.*;
import java.net.*;
import java.util.*;

public class DictionaryServer {
    public static void main(String[] args) {
        try {
            // Use a port that is free (change if needed)
            int port = 6000;
            ServerSocket server = new ServerSocket(port);
            System.out.println("Server started on port " + port + ". Waiting for client...");

            Socket socket = server.accept();
            System.out.println("Client connected.");

            // Input & Output Streams
            DataInputStream dis = new DataInputStream(socket.getInputStream());
            DataOutputStream dos = new DataOutputStream(socket.getOutputStream());

            // Predefined dictionary
            HashMap<String, String> dictionary = new HashMap<>();
            dictionary.put("java", "A high-level, object-oriented programming language.");
            dictionary.put("tcp", "Transmission Control Protocol — ensures reliable data transfer.");
            dictionary.put("socket", "An endpoint for communication between two systems.");
            dictionary.put("server", "A system that provides services to other systems.");
            dictionary.put("client", "A system that requests services from a server.");

            // Loop to allow multiple queries
            while (true) {
                String word = dis.readUTF();
                if (word.equalsIgnoreCase("exit")) {
                    System.out.println("Client disconnected.");
                    break;
                }

                System.out.println("Word received from client: " + word);
                String meaning = dictionary.getOrDefault(word.toLowerCase(), "Word not found in dictionary.");
                dos.writeUTF(meaning);
            }

            // Close all connections
            dis.close();
            dos.close();
            socket.close();
            server.close();
            System.out.println("Server stopped.");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
[10/26, 5:10 PM] 🤍: import java.io.*;
import java.net.*;
import java.util.*;

public class DictionaryClient {
    public static void main(String[] args) {
        try {
            // Connect to the server (same port as server)
            Socket socket = new Socket("localhost", 6000);
            System.out.println("Connected to server.");

            DataInputStream dis = new DataInputStream(socket.getInputStream());
            DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
            Scanner sc = new Scanner(System.in);

            // Loop for multiple lookups
            while (true) {
                System.out.print("Enter a word to search (or type 'exit' to quit): ");
                String word = sc.nextLine();

                dos.writeUTF(word);

                if (word.equalsIgnoreCase("exit")) {
                    System.out.println("Exiting...");
                    break;
                }

                String meaning = dis.readUTF();
                System.out.println("Meaning: " + meaning);
                System.out.println();
            }

            // Close all
            dis.close();
            dos.close();
            socket.close();
            sc.close();
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
