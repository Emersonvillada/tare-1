import java.util.Scanner;

public class HilosEjemplo {

    // Primer hilo: Conteo regresivo
    static class ConteoRegresivo implements Runnable {
        private int numero;

        public ConteoRegresivo(int numero) {
            this.numero = numero;
        }

        @Override
        public void run() {
            try {
                for (int i = numero; i >= 0; i--) {
                    System.out.println("Conteo regresivo: " + i);
                    Thread.sleep(500); // Temporalización de 500 ms
                }
                System.out.println("Trabajo del hilo 'Conteo regresivo' terminado.");
            } catch (InterruptedException e) {
                System.err.println("Error en el hilo de conteo regresivo");
            }
        }
    }

    // Segundo hilo: Mostrar letras del alfabeto
    static class MostrarLetras implements Runnable {
        private char letraFinal;

        public MostrarLetras(char letraFinal) {
            this.letraFinal = letraFinal;
        }

        @Override
        public void run() {
            try {
                for (char letra = 'A'; letra <= letraFinal; letra++) {
                    System.out.println("Letra: " + letra);
                    Thread.sleep(600); // Temporalización de 600 ms
                }
                System.out.println("Trabajo del hilo 'Mostrar letras' terminado.");
            } catch (InterruptedException e) {
                System.err.println("Error en el hilo de mostrar letras");
            }
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Validación de entrada para el conteo regresivo
        int numero;
        while (true) {
            System.out.print("Ingrese un número para el conteo regresivo: ");
            if (scanner.hasNextInt()) {
                numero = scanner.nextInt();
                if (numero >= 0) {
                    break;
                } else {
                    System.out.println("Por favor, ingrese un número entero positivo.");
                }
            } else {
                System.out.println("Entrada no válida. Por favor ingrese un número.");
                scanner.next(); // Limpiar el buffer del scanner
            }
        }

        // Validación de entrada para la letra final
        char letraFinal;
        while (true) {
            System.out.print("Ingrese una letra para el final del alfabeto: ");
            String input = scanner.next();
            if (input.length() == 1 && Character.isLetter(input.charAt(0))) {
                letraFinal = Character.toUpperCase(input.charAt(0));
                if (letraFinal >= 'A' && letraFinal <= 'Z') {
                    break;
                } else {
                    System.out.println("Por favor, ingrese una letra válida entre A y Z.");
                }
            } else {
                System.out.println("Entrada no válida. Por favor ingrese una letra.");
            }
        }

        // Crear los hilos
        Thread hiloConteo = new Thread(new ConteoRegresivo(numero));
        Thread hiloLetras = new Thread(new MostrarLetras(letraFinal));

        // Iniciar los hilos
        hiloConteo.start();
        hiloLetras.start();

        // Esperar a que los hilos terminen
        try {
            hiloConteo.join();
            hiloLetras.join();
        } catch (InterruptedException e) {
            System.err.println("Error esperando a que los hilos terminen.");
        }

        System.out.println("Ambos hilos han finalizado.");
        scanner.close();
    }
}
