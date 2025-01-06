# saloon_table


Saloon management website ke liye, aapko database mein kuch essential tables ki zarurat hogi jo saloon ke operations ko manage kar sakein. Yahan par kuch important tables diye gaye hain jo aapko apne database mein create karni chahiye:

### 1. **Users Table (Users)**

Is table mein customers aur staff (e.g., admin, employees) ka information store hoga.

```sql
CREATE TABLE Users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(255) UNIQUE,
    phone VARCHAR(20),
    password VARCHAR(255),
    role ENUM('customer', 'staff', 'admin') DEFAULT 'customer',
    status ENUM('active', 'inactive') DEFAULT 'active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 2. **Services Table (Services)**

Saloon ke services ko store karne ke liye.

```sql
CREATE TABLE Services (
    service_id INT AUTO_INCREMENT PRIMARY KEY,
    service_name VARCHAR(100),
    description TEXT,
    price DECIMAL(10, 2),
    duration INT,  -- in minutes
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 3. **Appointments Table (Appointments)**

Customer ke bookings (appointments) ke liye table.

```sql
CREATE TABLE Appointments (
    appointment_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,  -- Customer (Foreign Key)
    service_id INT,  -- Service booked (Foreign Key)
    staff_id INT,  -- Staff assigned to appointment (Foreign Key)
    appointment_date DATETIME,
    status ENUM('pending', 'completed', 'canceled') DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (service_id) REFERENCES Services(service_id),
    FOREIGN KEY (staff_id) REFERENCES Users(user_id)
);
```

### 4. **Payments Table (Payments)**

Saloon ki services ka payment record.

```sql
CREATE TABLE Payments (
    payment_id INT AUTO_INCREMENT PRIMARY KEY,
    appointment_id INT,  -- Related appointment (Foreign Key)
    payment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    amount DECIMAL(10, 2),
    payment_method ENUM('cash', 'credit_card', 'debit_card', 'online') DEFAULT 'cash',
    payment_status ENUM('completed', 'pending', 'failed') DEFAULT 'completed',
    FOREIGN KEY (appointment_id) REFERENCES Appointments(appointment_id)
);
```

### 5. **Staff Schedule Table (Staff_Schedule)**

Staff ke schedule aur availability ko track karna.

```sql
CREATE TABLE Staff_Schedule (
    schedule_id INT AUTO_INCREMENT PRIMARY KEY,
    staff_id INT,  -- Staff (Foreign Key)
    work_date DATE,
    work_start_time TIME,
    work_end_time TIME,
    status ENUM('available', 'unavailable') DEFAULT 'available',
    FOREIGN KEY (staff_id) REFERENCES Users(user_id)
);
```

### 6. **Saloon Inventory Table (Inventory)**

Saloon ke products aur materials ko manage karne ke liye.

```sql
CREATE TABLE Inventory (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    product_name VARCHAR(100),
    quantity INT,
    price DECIMAL(10, 2),
    supplier VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 7. **Customer Reviews Table (Reviews)**

Customer feedback aur reviews store karna.

```sql
CREATE TABLE Reviews (
    review_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,  -- Customer (Foreign Key)
    service_id INT,  -- Service being reviewed (Foreign Key)
    rating INT,  -- Rating out of 5
    review_text TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (service_id) REFERENCES Services(service_id)
);
```

### 8. **Notifications Table (Notifications)**

Saloon ke notifications ko track karne ke liye (appointments, reminders, etc.).

```sql
CREATE TABLE Notifications (
    notification_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,  -- User receiving the notification (Foreign Key)
    message TEXT,
    notification_type ENUM('appointment', 'reminder', 'general') DEFAULT 'general',
    status ENUM('read', 'unread') DEFAULT 'unread',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);
```

### 9. **Salon Settings Table (Settings)**

Saloon ki settings (e.g., working hours, discount settings) ko store karne ke liye.

```sql
CREATE TABLE Settings (
    setting_id INT AUTO_INCREMENT PRIMARY KEY,
    setting_name VARCHAR(100),
    setting_value VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 10. **Discounts Table (Discounts)**

Saloon mein koi discount schemes hain to unka record.

```sql
CREATE TABLE Discounts (
    discount_id INT AUTO_INCREMENT PRIMARY KEY,
    service_id INT,  -- Related service (Foreign Key)
    discount_percentage DECIMAL(5, 2),
    start_date DATE,
    end_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (service_id) REFERENCES Services(service_id)
);
```

---

### Summary:
1. **Users** table: Customer aur staff ka information.
2. **Services** table: Saloon ki services ka record.
3. **Appointments** table: Customer ke appointments ka record.
4. **Payments** table: Saloon ke payments ka record.
5. **Staff_Schedule** table: Staff ke working hours.
6. **Inventory** table: Saloon ke products/materials.
7. **Reviews** table: Customer feedback aur ratings.
8. **Notifications** table: Customer ko appointment reminders ya other notifications.
9. **Settings** table: Saloon ki basic settings.
10. **Discounts** table: Services par available discounts.

In tables ke through aap apne saloon management system ko efficiently manage kar sakte hain.
