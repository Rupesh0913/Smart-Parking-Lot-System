# 🚗 Smart Parking Lot System

---

## 1. 📌 Objective

Design the low-level architecture for a backend system of a **Smart Parking Lot** that:

- Allocates parking spots automatically based on vehicle size
- Tracks entry and exit times
- Calculates parking fees
- Maintains real-time spot availability
- Handles concurrent vehicle operations

---

## 2. 📋 Assumptions

To keep the design focused and realistic:

- Parking lot has multiple floors.
- Each floor contains predefined parking spots.
- Spot types:
   - `MOTORCYCLE`
   - `CAR`
   - `BUS`
- One vehicle occupies one spot.
- No reservations (real-time allocation only).
- Payment happens during checkout.
- System runs in centralized backend server.

---

## 3. 🧠 Core Entities (Domain Modeling)

We identify real-world objects and convert them into system classes.

### Main Entities:

- ParkingLot
- Floor
- ParkingSpot
- Vehicle
- Ticket
- Payment
- FeeStrategy

---

## 4. 🏗 Class Design (Low-Level Architecture)

### 4.1 Vehicle

```java
class Vehicle {
    String licenseNumber;
    VehicleType vehicleType;
}


 enum VehicleType {
    MOTORCYCLE,
    CAR,
    BUS
}


 class ParkingSpot {
    Long spotId;
    VehicleType spotType;
    boolean isAvailable;
    Vehicle parkedVehicle;
}


class Floor {
    int floorNumber;
    List<ParkingSpot> spots;
    Map<VehicleType, Queue<ParkingSpot>> availableSpots;
}
```

## Why Queue? 

- Ensures O(1) allocation
- Always assigns first available spot


```java
class Ticket {
   String ticketId;
   Vehicle vehicle;
   ParkingSpot parkingSpot;
   LocalDateTime entryTime;
   LocalDateTime exitTime;
   double totalFee;
  }


class ParkingLot {
   List<Floor> floors;
   Map<String, Ticket> activeTickets;
}

```
# 5.  Database Schema Design
  
### Parking_spot

| Column | Type |
| ------ | ------ |
| id | BIGINT |
| floor_number | INT |
| spot_type | VARCHAR |
| is_available | BOOLEAN |


### Vehicle


| Column | Type |
| ------ | ------ |
| license_number | VARCHAR |
| vehicle_type | VARCHAR |

   	
   	
### Ticket

| Column | Type |
| ------ | ------ |
| ticket_id | VARCHAR |
| license_number | VARCHAR |
| spot_id | BIGINT |
| entry_time | TIMESTAMP |
| exit_time | TIMESTAMP |
| total_fee | DECIMAL |

---

## 6. ⚙ Parking Spot Allocation Algorithm

### 🎯 Goal

Efficient and scalable allocation.

---

### 📦 Data Structure Used

```java
Map<VehicleType, Queue<ParkingSpot>> availableSpots;
```
---

### 🚗 Check-In Flow

1. Vehicle enters.
2. Identify vehicle type.
3. Iterate through floors.
4. Check queue for matching vehicle type.
5. `poll()` available spot.
6. Mark spot unavailable.
7. Create ticket.
8. Store in `activeTickets` map.
9. Return ticket.

---

### ⏱ Time Complexity

- **O(F)** → F = number of floors
- Spot lookup → **O(1)**

---

## 7. 💰 Fee Calculation Design (Strategy Pattern)

To keep the system extensible, we use the **Strategy Pattern**.

---

### FeeStrategy Interface

```java
interface FeeStrategy {
    double calculateFee(Ticket ticket);
}
```
---

## Implementations

- `CarFeeStrategy`
- `MotorcycleFeeStrategy`
- `BusFeeStrategy`

---

### Example Fee Logic

```java
fee = hourlyRate * totalHours;


class RateCard {
  VehicleType vehicleType;
  double hourlyRate;
}
```
---

## 8. 🚗 Check-Out Flow

1. Vehicle provides ticket ID.
2. Retrieve ticket from `activeTickets`.
3. Set `exitTime`.
4. Calculate duration.
5. Use `FeeStrategy` to compute total fee.
6. Mark parking spot available.
7. Add spot back to queue.
8. Remove ticket from `activeTickets`.
9. Return total fee.

---

## 9. 🔐 Concurrency Handling

Since multiple vehicles may enter/exit simultaneously:

### 🚨 Problem

Two vehicles may get the same spot.

---

### ✅ Solutions

#### 1️⃣ Synchronized Allocation
Make spot allocation method `synchronized`.

#### 2️⃣ Database Locking

```sql
SELECT * FROM parking_spot WHERE id = ? FOR UPDATE;
```
---

### 3️⃣ Thread-Safe Collections

Use:

- `ConcurrentHashMap`
- `BlockingQueue`

---

### 🏆 Recommended Approach

Use atomic spot allocation logic + DB locking for production systems.

---

## 10. 🧱 Layered Architecture
    Controller Layer
    ↓
    ParkingService
    ↓
    Domain Models
    ↓
    Repository Layer
    ↓
    Database
---

## 11. 🔄 Real-Time Availability Update

- Spot marked unavailable during check-in.
- Spot added back to queue during check-out.
- Frontend dashboard can fetch availability count per floor.

---

## 12. 🚀 Future Enhancements

- Online pre-booking
- Dynamic pricing (peak hours)
- EV charging spots
- Handicapped priority spots
- Analytics dashboard
- QR-based entry system
- Payment gateway integration

---

## 13. ⭐ Design Strengths

- O(1) spot allocation
- Strategy pattern for flexible pricing
- Clean separation of responsibilities
- Concurrency-safe design
- Easily extendable

---

## 14. ✅ Conclusion

This design ensures:

- Efficient parking allocation
- Accurate fee calculation
- Real-time availability tracking
- Safe concurrent operations
- Scalable and extensible architecture

--- 
This concludes the low-level design for the Smart Parking Lot System.