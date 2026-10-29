
## 1. Problem summary

Create a ticket-booking system with 5 available tickets. Multiple user threads attempt to book tickets. The system must prevent the available ticket count from becoming negative, use a synchronized booking method, and report invalid requests or requests that cannot be fulfilled using exception handling.

## 2. Pseudocode

```text
START

CLASS TicketsUnavailableException
    EXTENDS Exception
    CONSTRUCTOR(message)
        CALL parent Exception constructor with message
    END CONSTRUCTOR
END CLASS

CLASS TicketBookingSystem
    DECLARE availableTickets AS INTEGER

    CONSTRUCTOR(initialTickets)
        availableTickets = initialTickets
    END CONSTRUCTOR

    SYNCHRONIZED METHOD bookTicket(userName, numberOfTickets)
        IF userName is null OR userName is blank THEN
            THROW IllegalArgumentException("Username cannot be empty")
        END IF

        IF numberOfTickets <= 0 THEN
            THROW IllegalArgumentException("Ticket request must be positive")
        END IF

        IF numberOfTickets > availableTickets THEN
            THROW TicketsUnavailableException("Not enough tickets available")
        END IF

        availableTickets = availableTickets - numberOfTickets
        DISPLAY booking success and remaining ticket count
    END METHOD

    METHOD getAvailableTickets()
        RETURN availableTickets
    END METHOD
END CLASS

CLASS BookingThread EXTENDS Thread
    DECLARE bookingSystem, userName, numberOfTickets

    CONSTRUCTOR(bookingSystem, userName, numberOfTickets)
        STORE the provided values
    END CONSTRUCTOR

    OVERRIDE METHOD run()
        TRY
            CALL bookingSystem.bookTicket(userName, numberOfTickets)
        CATCH TicketsUnavailableException
            DISPLAY ticket-unavailable message for this user
        CATCH IllegalArgumentException
            DISPLAY invalid-request message for this user
        END TRY
    END METHOD
END CLASS

MAIN
    CREATE TicketBookingSystem with 5 tickets

    CREATE at least 5 BookingThread objects using different usernames
    and ticket requests

    START every thread

    WAIT for every thread to finish using join()

    DISPLAY remaining ticket count
END MAIN

STOP
```

## 3. Actual Java code

Save the following code in a file named `Assignment10.java`.

```java
class TicketsUnavailableException extends Exception {
    public TicketsUnavailableException(String message) {
        super(message);
    }
}

class TicketBookingSystem {
    private int availableTickets;

    public TicketBookingSystem(int initialTickets) {
        if (initialTickets < 0) {
            throw new IllegalArgumentException("Initial tickets cannot be negative");
        }
        this.availableTickets = initialTickets;
    }

    public synchronized void bookTicket(String userName, int numberOfTickets)
            throws TicketsUnavailableException {
        if (userName == null || userName.isBlank()) {
            throw new IllegalArgumentException("Username cannot be empty");
        }

        if (numberOfTickets <= 0) {
            throw new IllegalArgumentException("Ticket request must be positive");
        }

        if (numberOfTickets > availableTickets) {
            throw new TicketsUnavailableException(
                    "Requested " + numberOfTickets + ", but only "
                    + availableTickets + " ticket(s) remain.");
        }

        availableTickets -= numberOfTickets;
        System.out.println(userName + " booked " + numberOfTickets
                + " ticket(s). Remaining tickets: " + availableTickets);
    }

    public synchronized int getAvailableTickets() {
        return availableTickets;
    }
}

class BookingThread extends Thread {
    private final TicketBookingSystem bookingSystem;
    private final String userName;
    private final int numberOfTickets;

    public BookingThread(TicketBookingSystem bookingSystem, String userName,
                         int numberOfTickets) {
        this.bookingSystem = bookingSystem;
        this.userName = userName;
        this.numberOfTickets = numberOfTickets;
    }

    @Override
    public void run() {
        try {
            bookingSystem.bookTicket(userName, numberOfTickets);
        } catch (TicketsUnavailableException e) {
            System.out.println(userName + "'s booking failed: " + e.getMessage());
        } catch (IllegalArgumentException e) {
            System.out.println("Invalid request from " + userName + ": "
                    + e.getMessage());
        }
    }
}

public class Assignment10 {
    public static void main(String[] args) {
        TicketBookingSystem system = new TicketBookingSystem(5);

        BookingThread[] threads = {
            new BookingThread(system, "Asha", 2),
            new BookingThread(system, "Rahul", 1),
            new BookingThread(system, "Mina", 2),
            new BookingThread(system, "Arjun", 1),
            new BookingThread(system, "Priya", 1)
        };

        for (BookingThread thread : threads) {
            thread.start();
        }

        for (BookingThread thread : threads) {
            try {
                thread.join();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                System.out.println("Main thread was interrupted while waiting.");
                return;
            }
        }

        System.out.println("Total tickets remaining: "
                + system.getAvailableTickets());
    }
}
```

## 4. Explanation

- **Custom checked exception:** `TicketsUnavailableException` extends `Exception`, so callers must handle or declare it.
- **Synchronization:** `bookTicket()` is `synchronized`, so only one thread at a time can check and update the ticket count. This prevents two threads from booking the same remaining tickets.
- **Validation:** Empty usernames and requests of zero or fewer tickets are rejected with `IllegalArgumentException`.
- **Threading:** Each `BookingThread` represents a user. Calling `start()` runs its `run()` method on a separate thread.
- **Waiting for completion:** `join()` makes the main thread wait until each booking thread finishes before displaying the final count.
- **Nondeterministic order:** Booking messages may appear in different orders because thread scheduling is not fixed.


Note: The exact order of output can vary. With the sample requests, successful bookings total no more than 5 tickets, and the remaining count cannot become negative.
