# Vehicle Rental System – SQL Queries Explained 

### 1. users

* user_id (Primary Key)
* name
* email
* role (Admin / Customer)

### 2. vehicles

* vehicle_id (Primary Key)
* name
* type (car / bike / truck)
* model
* registration_number
* price_per_day
* status (available / rented / maintenance)

### 3. bookings

* booking_id (Primary Key)
* user_id (Foreign Key → users)
* vehicle_id (Foreign Key → vehicles)
* start_date
* end_date
* booking_status

---

## Query 1: INNER JOIN

### Objective

To display complete booking details along with the **customer name** and **vehicle name**.

### Query

```sql
select
  b.booking_id,
  u.name as customer_name,
  v.name as vehicle_name,
  b.start_date,
  b.end_date,
  b.booking_status as status
from bookings b
inner join users u
  on b.user_id = u.user_id
inner join vehicles v
  on b.vehicle_id = v.vehicle_id;
```

### Explanation

1. `from bookings b` → bookings table is used as the base table
2. `inner join users u` → joins users using `user_id`
3. `inner join vehicles v` → joins vehicles using `vehicle_id`
4. `select` → selects required columns from all tables

👉 **INNER JOIN** returns only records that exist in **all joined tables**

---

## Query 2: EXISTS (Using NOT EXISTS)

### Objective

To find vehicles that have **never been booked**.

### Query

```sql
select
  v.vehicle_id,
  v.name,
  v.type,
  v.model,
  v.registration_number,
  v.price_per_day,
  v.status
from vehicles v
where not exists (
  select 1
  from bookings b
  where b.vehicle_id = v.vehicle_id
);
```

### Explanation

1. `from vehicles v` → selects all vehicles
2. `not exists` → checks whether a related booking exists
3. Subquery:

   * If a matching `vehicle_id` exists in bookings → EXISTS is true
4. `not exists` ensures only vehicles **without any booking** are returned

👉 **EXISTS** is used to check the existence of related records

---

## Query 3: WHERE Clause

### Objective

To retrieve only vehicles that are:

* Type: `car`
* Status: `available`

### Query

```sql
select
  vehicle_id,
  name,
  type,
  model,
  registration_number,
  price_per_day,
  status
from vehicles
where type = 'car'
  and status = 'available';
```

### Explanation

1. `from vehicles` → selects vehicle data
2. `where type = 'car'` → filters only cars
3. `and status = 'available'` → filters only available vehicles

👉 **WHERE** is used for row-level filtering

---

## Query 4: GROUP BY & HAVING

### Objective

To find vehicles that have been booked **more than 2 times**.

### Query

```sql
select
  v.name as vehicle_name,
  count(b.booking_id) as total_bookings
from vehicles v
inner join bookings b
  on v.vehicle_id = b.vehicle_id
group by v.vehicle_id, v.name
having count(b.booking_id) > 2;
```

### Explanation

1. Join `vehicles` and `bookings`
2. `group by v.vehicle_id` → groups bookings by vehicle
3. `count(b.booking_id)` → counts bookings per vehicle
4. `having count > 2` → filters grouped results

👉 **WHERE** filters rows
👉 **HAVING** filters grouped results

ERD Design Link : https://drawsql.app/teams/ph-company/diagrams/assignment
