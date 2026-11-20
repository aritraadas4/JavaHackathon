# SkyHop — Airline Ticket Booking & Management System

> A fully functional, professional airline booking management system built with **Spring Boot** (Java 17) and **Vanilla JavaScript** for a Java Hackathon.

---

## Project Structure

```
JAVA HACKATHON/
├── backend/                          ← Spring Boot Java Backend
│   ├── src/main/java/com/skyhop/
│   │   ├── SkyhopApplication.java    ← Spring Boot entry point
│   │   ├── model/
│   │   │   ├── Person.java           ← Base class (UNIT IV)
│   │   │   ├── Passenger.java        ← Abstract class (UNIT IV)
│   │   │   ├── Regular.java          ← Extends Passenger, 0% discount
│   │   │   ├── FrequentFlyer.java    ← Extends Passenger, 7% discount
│   │   │   ├── Billable.java         ← Interface, final GST = 0.05
│   │   │   ├── Flight.java           ← Flight entity
│   │   │   ├── BookingItem.java      ← Flight + quantity line item
│   │   │   ├── ItemStack.java        ← Custom array-based stack (undo)
│   │   │   └── Booking.java         ← Booking entity, implements Billable
│   │   ├── exception/
│   │   │   ├── FlightNotFoundException.java
│   │   │   ├── SeatsNotAvailableException.java
│   │   │   └── InvalidBookingException.java
│   │   ├── service/
│   │   │   └── BookingService.java  ← Core business logic
│   │   ├── controller/
│   │   │   ├── FlightController.java
│   │   │   ├── BookingController.java
│   │   │   └── DashboardController.java
│   │   └── dto/
│   │       ├── BookingRequestDTO.java
│   │       ├── BookingItemDTO.java
│   │       └── BookingResponseDTO.java
│   ├── pom.xml                       ← Maven build file (Spring Boot 3.2.5)
│   └── mvnw / mvnw.cmd              ← Maven wrapper (no install needed)
│
├── frontend/                         ← Vanilla HTML/CSS/JS Frontend
│   ├── index.html                    ← Main SPA entry point
│   ├── style.css                     ← Professional airline UI styles
│   └── app.js                        ← Full application logic + voice commands
│
├── README.md                         ← This file
└── AI_LOG.txt                        ← AI development log
```

---

## Quick Start

### Prerequisites

| Tool | Required Version | Check Command |
|------|-----------------|---------------|
| Java JDK | 17 or later (JDK 26 tested) | `java -version` |
| Maven | Included via `mvnw` | `.\mvnw.cmd -version` |
| Browser | Chrome or Edge | — (for voice commands) |

No separate installation of Maven or Node.js required.

---

### Step 1 — Start the Backend

Open a terminal in VS Code and run:

```bash
# Windows
cd backend
.\mvnw.cmd spring-boot:run
```

Wait for this message:
```
SKYHOP Airline Booking Backend Running on 8080
```

The backend starts at: `http://localhost:8080`

---

### Step 2 — Open the Frontend

Open **another terminal** (or just use File Explorer) and open:

```
frontend/index.html
```

in your Chrome or Edge browser. The frontend talks to the backend at `http://localhost:8080/api`.

**OR** open it directly:

```bash
# Windows — open in default browser
start frontend/index.html
```

> **Note:** Voice commands require Chrome. All other features work in any modern browser.

---

## REST API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/flights` | Get all flights |
| GET | `/api/flights/{id}` | Get single flight by ID |
| POST | `/api/bookings` | Create a new booking |
| GET | `/api/bookings` | Get all bookings |
| GET | `/api/bookings/{id}` | Search booking by ID (e.g., FLT1) |
| GET | `/api/dashboard` | Dashboard statistics |
| GET | `/api/report` | Daily booking report |

### Sample POST /api/bookings

```json
{
  "passengerName": "Rahul Sharma",
  "passengerType": "Regular",
  "items": [
    { "flightId": 101, "quantity": 2 },
    { "flightId": 105, "quantity": 1 }
  ]
}
```

---

## Test Cases — All Verified ✅

| Test | Input | Expected | Actual |
|------|-------|----------|--------|
| **TC1** | Regular, 2×Flight101 + 1×Flight105 | ₹9,345.00 | ✅ ₹9,345.0 |
| **TC2** | Frequent Flyer, 3×Flight102 + 2×Flight106 | ₹30,076.20 | ✅ ₹30,076.2 |
| **TC3** | Regular, 36×Flight102 (only 35 seats) | SeatsNotAvailableException | ✅ Correct error, seats unchanged |
| **TC4** | Flight ID 999 | FlightNotFoundException | ✅ Correct error, app continues |

### Billing Formula (Test 1 Walkthrough)

```
2 × ₹3,200 (Flight 101) = ₹6,400
1 × ₹2,500 (Flight 105) = ₹2,500
Subtotal                 = ₹8,900
Discount (Regular = 0%)  = ₹0.00
Discounted Subtotal      = ₹8,900
GST (5%)                 = ₹445.00
FINAL TOTAL              = ₹9,345.00 ✅
```

### Billing Formula (Test 2 Walkthrough)

```
3 × ₹2,800 (Flight 102) = ₹8,400
2 × ₹11,200 (Flight 106) = ₹22,400
Subtotal                 = ₹30,800
Discount (FF = 7%)       = ₹2,156.00
Discounted Subtotal      = ₹28,644.00
GST (5%)                 = ₹1,432.20
FINAL TOTAL              = ₹30,076.20 ✅
```

---

## Java Syllabus Implementation

| Unit | Concept | Implementation |
|------|---------|---------------|
| **UNIT I** | `Flight[]` and `Booking[]` arrays | `BookingService.java` — no ArrayList |
| **UNIT I** | `int`, `double`, `char`, `boolean`, `final GST = 0.05` | Throughout all model classes |
| **UNIT II** | `do-while` loop | `BookingService.initializeFlights()` |
| **UNIT II** | `switch` statement | Passenger type selection in `createBooking()` |
| **UNIT II** | Ternary for LOW/OK | `Flight.getSeatStatus()` |
| **UNIT II** | `token & 1` odd/even | `Booking` constructor |
| **UNIT III** | Classes, constructors, `this` | All model classes |
| **UNIT III** | Method overloading | `calculateSubtotalRecursive()` overloads |
| **UNIT III** | Static booking counter | `Booking.bookingCounter` |
| **UNIT III** | Recursive subtotal | `Booking.calculateSubtotalRecursive()` |
| **UNIT III** | Custom `ItemStack` | `ItemStack.java` — array-based stack for undo |
| **UNIT IV** | `Person → Passenger → Regular/FrequentFlyer` | Full class hierarchy |
| **UNIT IV** | `Billable` interface | `Booking implements Billable` |
| **UNIT IV** | `final` method | `Booking.getFormattedBookingId()`, `Passenger.hasValidName()` |
| **UNIT IV** | `toString()` override | All model classes |
| **UNIT V** | `FlightNotFoundException` | Checked exception, thrown and caught |
| **UNIT V** | `SeatsNotAvailableException` | Checked exception, thrown and caught |
| **UNIT V** | `throw`/`throws` | `BookingService.createBooking()` |
| **UNIT V** | Multiple `catch` blocks | `BookingController` |
| **UNIT V** | Nested `try` | Inner flight validation loop |
| **UNIT V** | `finally` | Validation outer try in `createBooking()` |
| **UNIT V** | `try-with-resources` | `findBookingById()` using `ByteArrayOutputStream` |

---

## Features

- **Dashboard** — Live stats: flights, seats, bookings, revenue, passenger types
- **Flight Management** — All 6 flights with seat status (LOW/OK)
- **New Booking** — Step-by-step booking with undo (ItemStack)
- **Invoice Generation** — Full bill with discount, GST, booking ID, token, counter
- **Print & PDF** — Print invoice or download as PDF
- **Booking History** — Search by booking ID or browse all bookings
- **Daily Report** — Flight popularity chart, passenger split, revenue
- **Voice Commands** — Full speech recognition (Chrome/Edge)

---

## Voice Commands

| Category | Command |
|----------|---------|
| Navigation | "Open dashboard", "Show flights", "New booking", "Open booking history", "Show daily report" |
| Flights | "Show flight 101", "Find Bengaluru to Goa", "How many seats on flight 104" |
| Booking | "Book flight 101", "Select two seats", "Select frequent flyer", "Undo last flight" |
| Search | "Search booking FLT1" |
| Other | "Show total revenue", "Show most popular flight", "Go back" |

---

## Technology Stack

- **Backend:** Java 17+ with Spring Boot 3.2.5
- **Build:** Maven (via bundled `mvnw.cmd`)
- **Frontend:** HTML5, CSS3, Vanilla JavaScript (ES2020)
- **Charts:** Chart.js v4.4.0 (CDN)
- **Icons:** Font Awesome 6.5.0 (CDN)
- **Voice:** Web Speech API (SpeechRecognition)
#   J a v a H a c k a t h o n  
 