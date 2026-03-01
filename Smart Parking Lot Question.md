# 🚗 Smart Parking Lot – Low Level Design

---

## 📄 Brief

### 🎯 Objective

Design the low-level architecture for a backend system of a **Smart Parking Lot**.  
This system should handle:

- Vehicle entry and exit management
- Parking space allocation
- Parking fee calculation

---

## 🧩 Problem Statement

Imagine a parking lot in an urban area with multiple floors and numerous parking spots.

Your task is to create a **low-level design** for a system that efficiently manages the parking process.

The system should:

- Automatically assign parking spots based on vehicle size and availability
- Track the time each vehicle spends in the parking lot
- Calculate parking fees upon exit

---

## ✅ Functional Requirements

### 1️⃣ Parking Spot Allocation
Automatically assign an available parking spot to a vehicle when it enters, based on the vehicle’s size:

- Motorcycle
- Car
- Bus

---

### 2️⃣ Check-In and Check-Out
- Record the entry time of vehicles
- Record the exit time of vehicles

---

### 3️⃣ Parking Fee Calculation
- Calculate fees based on:
    - Duration of stay
    - Vehicle type

---

### 4️⃣ Real-Time Availability Update
- Update parking spot availability in real-time
- Reflect changes immediately when vehicles enter or leave

---

## 🏗 Design Aspects to Consider

### 📦 Data Model
Design a database schema to manage:
- Parking spots
- Vehicles
- Parking transactions

---

### ⚙ Algorithm for Spot Allocation
Develop an efficient algorithm to:
- Assign available spots
- Minimize lookup time
- Scale with multiple floors

---

### 💰 Fee Calculation Logic
Implement logic to:
- Calculate parking duration
- Apply vehicle-type-based pricing

---

### 🔐 Concurrency Handling
Ensure the system can handle:
- Multiple vehicles entering simultaneously
- Multiple vehicles exiting simultaneously
- No duplicate spot allocation

---