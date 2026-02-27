# Bus Ticket System - Complete Django & React Implementation

A production-ready bus ticket booking system with Django REST API backend and React frontend featuring role-based access control, user self-registration, and comprehensive admin management.

## ✨ Key Features

### 👤 User Features:
- **Self-Registration**: Users can create accounts without admin help
- **Secure Authentication**: Token-based login with role detection
- **Search & Book Tickets**:
  - View all available schedules with complete bus details
  - See bus number, bus name, price, departure time
  - Select specific seat from interactive seat grid
  - Confirm booking with detailed review
- **Manage Bookings**: View, cancel your reservations
- **Secure Logout**: Complete session cleanup

### 👨‍💼 Administrator Features:
- **Full CRUD Operations**: Manage routes, buses, and schedules
- **Booking Management**:
  - View all user bookings with details
  - **Edit bookings**: Change seat numbers if user made mistake
  - **Delete bookings**: Remove bookings if needed
  - Complete audit trail
- **Dashboard**: Role-based UI automatic on login

## Project Structure

```
ticketsystemproject/
├── manage.py
├── db.sqlite3
├── myapp/                # Django app with models, views, serializers
│   ├── models.py
│   ├── views.py
│   ├── Serializers.py
│   ├── urls.py
│   ├── permissions.py   # Custom permission classes
│   └── migrations/
├── ticketsystemproject/  # Django project settings
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── frontend/            # React frontend
    ├── package.json
    ├── public/
    │   └── index.html
    └── src/
        ├── App.js
        ├── Login.js
        ├── Register.js
        ├── UserDashboard.js
        ├── AdminDashboard.js
        ├── api.js         # Axios API client
        └── *.css          # Styles
```

## 🎯 User Workflows

### Registration & Login Flow (New Users)
1. Visit http://localhost:3000 → Login page
2. Click **"Register here"** link
3. Fill in: Username, Email, Password, Confirm Password
4. Click **Register** → Automatic redirect to Login on success
5. Login with credentials → System detects user type
6. Regular user → **User Dashboard** | Admin user → **Admin Dashboard**

### User Booking Process
1. Go to **"Search & Book"** tab
2. View all available schedules (bus details shown per card)
3. Click **"Select & Book"** on desired schedule
4. Booking modal opens showing:
   - Route details (From → To)
   - Bus number & name
   - Departure date/time
   - Price
   - Current seat selection
5. Click on available seats (green) to select
6. Selected seat highlights in dark green
7. Click **"Confirm Booking"** to finalize
8. Success message shows booking confirmed
9. View booking in **"My Bookings"** tab

### Admin Management Process
1. Login as admin (created via `createsuperuser`)
2. See **5 management tabs**:

   **Routes** - Add/Delete routes
   ```
   Form: From Location, To Location, Distance
   List: All routes with delete buttons
   ```

   **Buses** - Add/Delete buses  
   ```
   Form: Bus Number, Bus Name, Total Seats
   List: All buses with capacity info
   ```

   **Schedules** - Add/Delete schedules
   ```
   Form: Select Route, Select Bus, Departure Time, Price, Available Seats
   List: All schedules with details
   ```

   **Bookings** - View/Edit/Delete bookings
   ```
   List: All user bookings with details
   Edit: Change seat number if user made error
   Delete: Remove booking if needed
   ```

## Prerequisites

- Python 3.8+
- Node.js & npm
- pip (Python package manager)

## Backend Setup (Django)

### 1. Install Python Dependencies

```bash
pip install django djangorestframework django-cors-headers
```

### 2. Apply Database Migrations

```bash
python manage.py migrate
```

### 3. Create Admin User

```bash
python manage.py createsuperuser
```

Follow the prompts to create an admin account. Use this account to log in with `is_staff=true`.

### 4. Run Django Server

```bash
python manage.py runserver
```

The API will be available at: `http://localhost:8000/api`

### 5. Add Sample Data (Optional)

You can add routes, buses, and schedules through:
- Django Admin: `http://localhost:8000/admin`
- Or directly through the React admin dashboard once you log in

## Frontend Setup (React)

### 1. Navigate to Frontend Directory

```bash
cd frontend
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Start Development Server

```bash
npm start
```

The frontend will open at: `http://localhost:3000`

## API Endpoints

### Authentication
- `POST /api/register/` - Register new user
- `POST /api/login/` - Login user (returns token and is_staff flag)
- `POST /api/logout/` - Logout user (requires authentication)

### Routes Management
- `GET /api/routes/` - List all routes
- `POST /api/routes/` - Create route (admin only)
- `PUT /api/routes/{id}/` - Update route (admin only)
- `DELETE /api/routes/{id}/` - Delete route (admin only)

### Buses Management
- `GET /api/buses/` - List all buses
- `POST /api/buses/` - Create bus (admin only)
- `PUT /api/buses/{id}/` - Update bus (admin only)
- `DELETE /api/buses/{id}/` - Delete bus (admin only)

### Schedules Management
- `GET /api/schedules/` - List all schedules
- `POST /api/schedules/` - Create schedule (admin only)
- `PUT /api/schedules/{id}/` - Update schedule (admin only)
- `DELETE /api/schedules/{id}/` - Delete schedule (admin only)
- `GET /api/schedules/{id}/available_seats/` - Get available seats for schedule

### Bookings
- `GET /api/bookings/` - Get user's bookings (or all if admin)
- `POST /api/bookings/` - Create booking (authenticated users)
- `DELETE /api/bookings/{id}/` - Cancel booking

## Login Flow

When a user logs in, the API returns:

```json
{
  "token": "token_string",
  "user_id": 1,
  "username": "john_doe",
  "is_staff": false
}
```

The frontend checks the `is_staff` flag to determine if the user is an admin:
- **is_staff = false**: Show User Dashboard (search, book, manage bookings)
- **is_staff = true**: Show Admin Dashboard (manage routes, buses, schedules, view all bookings)

## Testing the System

### Complete User Journey (Regular User):
1. Go to `http://localhost:3000`
2. **Click "Register here"** to create new account
3. Fill in username, email, password (2x)
4. Click **Register** → Redirects to login after success
5. **Login** with new credentials
6. You see **User Dashboard** (admin sees Admin Dashboard instead)
7. Go to **"Search & Book"** tab
8. See all available schedules with:
   - Route (From → To)
   - Bus Number & Name
   - Departure Date/Time
   - Price
   - Available seats count
9. Click **"Select & Book"** on any schedule
10. **Booking modal** opens showing:
    - Complete route details
    - Bus number and name
    - Departure date/time
    - Price in TZS
    - Available seats: You can see which seats are available (green) vs booked (red)
11. **Click on a seat** → It highlights in dark green to show selection
12. Click **"Confirm Booking"** → Success message
13. Go to **"My Bookings"** tab
14. **See your booking** with seat number and booking date
15. Can **cancel** if needed

### Admin Booking Management:
1. Login as admin account (created via `python manage.py createsuperuser`)
2. See **Admin Dashboard** with tabs: Routes, Buses, Schedules, **Bookings**
3. Go to **Bookings tab** → See ALL user bookings
4. Each booking shows: User, Seat Number, Booking Date
5. **Edit button**: Click to change seat (if user made mistake)
   - Modal opens with current booking info
   - Change seat number
   - Click **"Update Booking"**
6. **Delete button**: Click to cancel booking completely
   - Confirmation prompt appears
   - Booking removed from system

### Complete Admin Setup Test:
1. Login as admin
2. **Routes Tab**: Add route "Dar es Salaam" → "Morogoro"
3. **Buses Tab**: Add bus "TZ-001", "Express Bus", capacity 50
4. **Schedules Tab**:
   - Select the route you created
   - Select the bus you created
   - Set departure time (e.g., tomorrow at 9 AM)
   - Set price (e.g., 35,000 TZS)
   - Set available seats to 50
5. Click **Add Schedule**
6. Logout and login as regular user
7. See schedule appears in Search & Book
8. Book a seat → Logout
9. Login as admin again
10. Go to Bookings → See user's booking
11. Click Edit → Change seat number → Update
12. Verify booking updated

## CORS Configuration

The Django backend is configured to allow requests from:
- `http://localhost:3000`
- `http://127.0.0.1:3000`

If you're running on a different port, update `CORS_ALLOWED_ORIGINS` in `ticketsystemproject/settings.py`.

## Troubleshooting

### CORS Errors
- Ensure Django server is running on port 8000
- Check that `CORS_ALLOWED_ORIGINS` includes your React port
- Clear browser cookies and cache

### Token Authentication Errors
- Make sure the token is properly stored in localStorage
- Check that the API returns the token after login
- Verify token is sent with each request header

### Database Errors
- Run `python manage.py migrate` to apply pending migrations
- Clear `db.sqlite3` and re-run migrations if needed

## Future Enhancements

- Payment integration
- Email notifications
- Advanced search filters
- Ticket cancellation policies
- Review and rating system
- Mobile app version
"# TICKET" 

## Deployment (Render Backend + Vercel Frontend)

### 1. Backend on Render

This repository now includes:
- `requirements.txt`
- `build.sh`
- `render.yaml`

Steps:
1. Push this project to GitHub.
2. In Render, create Blueprint deployment from your repo (it will use `render.yaml`).
3. After first deploy, update `CORS_ALLOWED_ORIGINS` env var in Render with your real Vercel URL.
4. Confirm backend is live at: `https://ticketsystem-backend.onrender.com/api/`

### 2. Frontend on Vercel

This repository now includes:
- `frontend/vercel.json`

Steps:
1. Import the same GitHub repo into Vercel.
2. Set project root to `frontend`.
3. Add Environment Variable in Vercel:
   - `REACT_APP_API_BASE_URL=https://ticketsystem-backend.onrender.com/api`
4. Deploy.

### 3. Required post-deploy update

After Vercel deploy, copy your actual Vercel domain and set Render env:
- `CORS_ALLOWED_ORIGINS=https://your-real-app.vercel.app`
