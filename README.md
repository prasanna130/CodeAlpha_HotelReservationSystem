import java.util.*;
import java.io.*;

public class HotelReservationSystem {
    static List<Room> rooms = new ArrayList<>();
    static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {
        initializeRooms();
        int choice;

        do {
            System.out.println("\n=== Hotel Reservation System ===");
            System.out.println("1. View All Rooms");
            System.out.println("2. Book a Room");
            System.out.println("3. Cancel Reservation");
            System.out.println("4. View Available Rooms");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();
            sc.nextLine(); // consume newline

            switch (choice) {
                case 1 -> viewAllRooms();
                case 2 -> bookRoom();
                case 3 -> cancelReservation();
                case 4 -> viewAvailableRooms();
                case 5 -> System.out.println("Thank you for using Hotel Reservation System.");
                default -> System.out.println("Invalid choice! Try again.");
            }
        } while (choice != 5);
    }

    static void initializeRooms() {
        rooms.add(new Room(101, "Standard"));
        rooms.add(new Room(102, "Standard"));
        rooms.add(new Room(201, "Deluxe"));
        rooms.add(new Room(202, "Deluxe"));
        rooms.add(new Room(301, "Suite"));
    }

    static void viewAllRooms() {
        System.out.println("\n--- All Rooms ---");
        for (Room room : rooms) {
            System.out.println(room);
        }
    }

    static void bookRoom() {
        System.out.print("\nEnter room number to book: ");
        int roomNumber = sc.nextInt();
        sc.nextLine();
        Room room = getRoomByNumber(roomNumber);

        if (room != null) {
            if (!room.isBooked) {
                System.out.print("Enter customer name: ");
                String name = sc.nextLine();
                room.book(name);
                System.out.println("Room booked successfully.");
            } else {
                System.out.println("Room is already booked.");
            }
        } else {
            System.out.println("Invalid room number.");
        }
    }

    static void cancelReservation() {
        System.out.print("\nEnter room number to cancel: ");
        int roomNumber = sc.nextInt();
        sc.nextLine();
        Room room = getRoomByNumber(roomNumber);

        if (room != null) {
            if (room.isBooked) {
                room.cancel();
                System.out.println("Reservation cancelled.");
            } else {
                System.out.println("Room is not currently booked.");
            }
        } else {
            System.out.println("Invalid room number.");
        }
    }

    static void viewAvailableRooms() {
        System.out.println("\n--- Available Rooms ---");
        for (Room room : rooms) {
            if (!room.isBooked) {
                System.out.println(room);
            }
        }
    }

    static Room getRoomByNumber(int number) {
        for (Room room : rooms) {
            if (room.roomNumber == number) {
                return room;
            }
        }
        return null;
    }
}

class Room {
    int roomNumber;
    String type; // Standard, Deluxe, Suite
    boolean isBooked;
    String customerName;

    public Room(int roomNumber, String type) {
        this.roomNumber = roomNumber;
        this.type = type;
        this.isBooked = false;
        this.customerName = "";
    }

    public void book(String customerName) {
        this.isBooked = true;
        this.customerName = customerName;
    }

    public void cancel() {
        this.isBooked = false;
        this.customerName = "";
    }

    @Override
    public String toString() {
        return "Room " + roomNumber + " (" + type + ") - " + (isBooked ? "Booked by " + customerName : "Available");
    }
}
