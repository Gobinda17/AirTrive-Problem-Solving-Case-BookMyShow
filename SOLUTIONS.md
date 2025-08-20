# BookMyShow Backend Assignment
<br/>

## Step 1: Entities and Attributes


### 1. Movie
- id
- name
- duration
- language

### 2. Theatre
- id
- name
- location

### 3. Show
- id
- movie_id
- theatre_id
- show_date
- show_time

### 4. Seat
- id
- theatre_id
- seat_number
- seat_type

### 5. Booking
- id
- show_id
- user_id
- seat_id
- booking_time
- status

### 6. User
- id
- name
- email

---
<br/>

## Step 2: Attribute Lists & Table Design

### Table Normalization (1NF, 2NF, 3NF, BCNF)
- All tables use atomic fields with a single value per cell (**1NF**)
- No partial dependencies—no composite keys used, all fields depend on the complete primary key (**2NF**)
- No transitive dependencies—all non-key attributes depend only on the primary key (**3NF**)
- All functional dependencies have the left side as a superkey (**BCNF**)

### 1. Movie

- **movie_id** (Primary Key, INT, AUTO_INCREMENT)

- **name** (VARCHAR)

- **duration** (INT, in minutes)

- **language** (VARCHAR)

### 2. Theatre

- **theatre_id** (Primary Key, INT, AUTO_INCREMENT)

- **name** (VARCHAR)

- **location** (VARCHAR)

### 3. Show

- **show_id** (Primary Key, INT, AUTO_INCREMENT)

- **movie_id** (Foreign Key → Movie)

- **theatre_id** (Foreign Key → Theatre)

- **show_date** (DATE)

- **show_time** (TIME)

### 4. Seat

- **seat_id** (Primary Key, INT, AUTO_INCREMENT)

- **theatre_id** (Foreign Key → Theatre)

- **seat_number** (VARCHAR)

- **seat_type** (VARCHAR)

## 5. User

- **user_id** (Primary Key, INT, AUTO_INCREMENT)

- **name** (VARCHAR)

- **email** (VARCHAR)

## 6. Booking

- **booking_id** (Primary Key, INT, AUTO_INCREMENT)

- **show_id** (Foreign Key → Show)

- **user_id** (Foreign Key → User)

- **seat_id** (Foreign Key → Seat)

- **booking_time** (DATETIME)

- **status** (VARCHAR, e.g., 'confirmed', 'cancelled')

---

## Step 3: SQL Table Creation

#### `CREATE TABLE -> Movie`
```sql
CREATE TABLE Movie (
movie_id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(100) NOT NULL,
duration INT NOT NULL,
language VARCHAR(50) NOT NULL
);
```

#### `CREATE TABLE -> Theatre`
```sql
CREATE TABLE Theatre (
theatre_id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(100) NOT NULL,
location VARCHAR(100) NOT NULL
);
```

#### `CREATE TABLE -> Show`
```sql
CREATE TABLE Show (
show_id INT PRIMARY KEY AUTO_INCREMENT,
movie_id INT NOT NULL,
theatre_id INT NOT NULL,
show_date DATE NOT NULL,
show_time TIME NOT NULL,
FOREIGN KEY (movie_id) REFERENCES Movie(movie_id),
FOREIGN KEY (theatre_id) REFERENCES Theatre(theatre_id)
);
```

#### `CREATE TABLE -> Seat`
```sql
CREATE TABLE Seat (
seat_id INT PRIMARY KEY AUTO_INCREMENT,
theatre_id INT NOT NULL,
seat_number VARCHAR(10) NOT NULL,
seat_type VARCHAR(20) NOT NULL,
FOREIGN KEY (theatre_id) REFERENCES Theatre(theatre_id)
);
```

#### `CREATE TABLE -> User`
```sql
CREATE TABLE User (
user_id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(100) NOT NULL,
email VARCHAR(100) NOT NULL
);
```

#### `CREATE TABLE -> Booking`
```sql
CREATE TABLE Booking (
booking_id INT PRIMARY KEY AUTO_INCREMENT,
show_id INT NOT NULL,
user_id INT NOT NULL,
seat_id INT NOT NULL,
booking_time DATETIME NOT NULL,
status VARCHAR(20) NOT NULL,
FOREIGN KEY (show_id) REFERENCES Show(show_id),
FOREIGN KEY (user_id) REFERENCES User(user_id),
FOREIGN KEY (seat_id) REFERENCES Seat(seat_id)
);
```
---

## Step 4: Sample Data Insertion

#### `INSERT INTO -> Movie`
```sql
INSERT INTO Movie (name, duration, language) VALUES ('Inception', 148, 'English');
INSERT INTO Movie (name, duration, language) VALUES ('Dangal', 161, 'Hindi');
```

#### `INSERT INTO -> Theatre`
```sql
INSERT INTO Theatre (name, location) VALUES ('Fun Cinemas', 'Guwahati');
INSERT INTO Theatre (name, location) VALUES ('PVR Cinemas', 'Guwahati');
```

#### `INSERT INTO -> Show`
```sql
INSERT INTO Show (movie_id, theatre_id, show_date, show_time) VALUES (1, 1, '2025-08-21', '15:00:00');
INSERT INTO Show (movie_id, theatre_id, show_date, show_time) VALUES (2, 1, '2025-08-21', '18:00:00');
INSERT INTO Show (movie_id, theatre_id, show_date, show_time) VALUES (1, 2, '2025-08-21', '17:00:00');
```

#### `INSERT INTO -> Seat`
```sql
INSERT INTO Seat (theatre_id, seat_number, seat_type) VALUES (1, 'A1', 'Regular');
INSERT INTO Seat (theatre_id, seat_number, seat_type) VALUES (1, 'A2', 'Regular');
INSERT INTO Seat (theatre_id, seat_number, seat_type) VALUES (2, 'B1', 'VIP');
```

#### `INSERT INTO -> User`
```sql
INSERT INTO User (name, email) VALUES ('Rahul Sharma', 'rahul@example.com');
INSERT INTO User (name, email) VALUES ('Anjali Singh', 'anjali@example.com');
```

#### `INSERT INTO -> Booking`
```sql
INSERT INTO Booking (show_id, user_id, seat_id, booking_time, status)
VALUES (1, 1, 1, '2025-08-20 11:00:00', 'confirmed');
INSERT INTO Booking (show_id, user_id, seat_id, booking_time, status)
VALUES (2, 2, 2, '2025-08-20 11:10:00', 'confirmed');
```

---

## Step 5: Show Query

> Find all shows for a given theatre and date, along with movie name and show time:

#### `SHOW QUERY`
```sql
SELECT
Show.show_id,
Movie.name AS movie_name,
Show.show_date,
Show.show_time
FROM Show
JOIN Movie ON Show.movie_id = Movie.movie_id
WHERE Show.theatre_id = 1
AND Show.show_date = '2025-08-21';
```

**Sample Output:**

| show_id | movie_name | show_date   | show_time |
|---------|------------|-------------|-----------|
| 1       | Inception  | 2025-08-21  | 15:00:00  |
| 2       | Dangal     | 2025-08-21  | 18:00:00  |

---

