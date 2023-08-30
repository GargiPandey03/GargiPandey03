import java.util.*;

// Enum representing the direction of the elevator
enum Direction {
    UP, DOWN, IDLE
}

// Class representing a request for the elevator
class Request {
    private int floor;
    private Direction direction;

    public Request(int floor, Direction direction) {
        this.floor = floor;
        this.direction = direction;
    }

    public int getFloor() {
        return floor;
    }

    public Direction getDirection() {
        return direction;
    }
}

// Class representing an elevator
class Elevator {
    private int currentFloor;
    private Direction direction;
    private Set<Request> requests;

    public Elevator() {
        currentFloor = 1; // Start at the first floor
        direction = Direction.IDLE;
        requests = new TreeSet<>(Comparator.comparing(Request::getFloor)); // Using TreeSet to sort requests by floor
    }

    public int getCurrentFloor() {
        return currentFloor;
    }

    public Direction getDirection() {
        return direction;
    }

    public void addRequest(Request request) {
        requests.add(request);
    }

    public void move() {
        if (!requests.isEmpty()) {
            Request nextRequest = requests.iterator().next();
            int requestedFloor = nextRequest.getFloor();
            if (requestedFloor > currentFloor) {
                direction = Direction.UP;
                currentFloor++;
            } else if (requestedFloor < currentFloor) {
                direction = Direction.DOWN;
                currentFloor--;
            } else {
                direction = Direction.IDLE;
                requests.remove(nextRequest);
            }
            System.out.println("Elevator is on floor " + currentFloor + " moving " + direction);
        } else {
            direction = Direction.IDLE;
            System.out.println("Elevator is idle");
        }
    }
}

public class ElevatorSystem {
    public static void main(String[] args) {
        Elevator elevator = new Elevator();

        // Simulate elevator requests
        elevator.addRequest(new Request(5, Direction.UP));
        elevator.addRequest(new Request(3, Direction.DOWN));
        elevator.addRequest(new Request(7, Direction.UP));

        // Move the elevator
        while (elevator.getDirection() != Direction.IDLE) {
            elevator.move();
        }
    }
}
