### Feature: User Authentication

**Description:**  
Handles user registration, login, and authentication for accessing the platform.

**API Endpoints:**  
- POST /api/register – Registers a new user
- POST /api/login – Logs in a user and returns a JWT token

**Input Specifications:**  
- `POST /api/register`  
  - `username` (string, required, min 3 characters)  
  - `email` (string, required, valid email format)  
  - `password` (string, required, min 8 characters)  
- `POST /api/login`  
  - `email` (string, required, valid email)  
  - `password` (string, required)  

**Output Specifications:**  
- Success:  
```json
{ "message": "User registered successfully", "userId": 1 }
- Error: { "error": "Email already exists" }

**Validation Rules:**  
- Password must be at least 8 characters
- Email must be unique
- Username cannot be empty

**Performance Criteria:**  
- Response time < 2 seconds
- Password stored using hashing (bcrypt)



## Feature 1: Property Management

**Description:**  
Allows hosts to create, update, view, or delete property listings.

**API Endpoints:**  
- **POST /api/properties** – Add new property  
- **PUT /api/properties/:id** – Update property  
- **DELETE /api/properties/:id** – Delete property  
- **GET /api/properties** – List all properties  

**Input Specifications:**  
- `POST /api/properties`  
  - `title` (string, required) – Name of the property  
  - `description` (string, optional) – Details about the property  
  - `price` (number, required, positive) – Price per night  
  - `location` (string, required) – Property location  
  - `images` (array of strings, optional) – URLs of property images  

**Output Specifications:**  
- Success:  
```json
{
  "message": "Property added successfully",
  "propertyId": 101
}

- Error:
{
  "error": "Price must be positive"
}

**Validation Rules:**  
- Price must be a positive number
- Title and location are mandatory
- Maximum image upload size: 5MB

**Performance Criteria:**  
- Response time < 2 seconds
- Database updates must be atomic to prevent partial data saves



### Feature 3: Booking System

**Description:**  
Enables users to book available properties and manage their bookings efficiently.

**API Endpoints:**  
- **POST /api/bookings** – Create a new booking  
- **GET /api/bookings/:userId** – View bookings of a specific user  
- **DELETE /api/bookings/:bookingId** – Cancel an existing booking  

**Input Specifications:**  
- `POST /api/bookings`  
  - `userId` (integer, required) – ID of the user making the booking  
  - `propertyId` (integer, required) – ID of the property being booked  
  - `startDate` (date, required) – Booking start date  
  - `endDate` (date, required) – Booking end date  

**Output Specifications:**  
- Success:  
```json
{
  "message": "Booking successful",
  "bookingId": 5001
}

- Error:

{
  "error": "Property not available for selected dates"
}

**Validation Rules:**

- startDate must be before endDate
- User cannot book overlapping dates for the same property
- All fields are required and must be valid

**Performance Criteria:**
- Response time < 3 seconds
- Ensure no double-booking for the same property
- All bookings should be recorded securely in the database