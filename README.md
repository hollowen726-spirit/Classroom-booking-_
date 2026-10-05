# Spirit University Classroom Booking System

## Features
- Spirit University image-based login template
- University Email / Student ID / PRN login
- Password with show/hide toggle
- Optional Student / Faculty / Admin role selection
- Automatic role detection when role is left blank
- Remember Me
- Forgot Password flow placeholder
- bcrypt password hashing
- Generic invalid-login error
- Student classroom booking
- Faculty access to see the classroom booking schedule
- Admin access to users and all bookings
- Automatic overlapping-booking conflict detection
- SQLite database
- No OTP and no Twilio

## Run on Windows
1. Open this folder in VS Code.
2. Open Terminal.
3. Run:

```powershell
python -m pip install -r requirements.txt
python app.py
```

4. Open http://127.0.0.1:5000

You can also double-click `run.bat`.

## Demo accounts
Student: `2024BTCY001` / `Student@123`
Faculty: `hello@spirit.edu` / `Hello@726`
Admin: `hi@spirit.edu` / `Hi@726`

For a real university deployment, replace the demo users, secret key and development reset flow, and use HTTPS.


## Multiple users
- The app supports multiple independent user accounts using SQLite.
- New students can register from the **Create a student account** link on the login page.
- Each account has a unique Email, Student ID, and PRN.
- Each logged-in user has their own session and booking history.
- Existing demo accounts remain available.


## Pre-registered student accounts

All five students use the same password: `Spirit@123`

- Jai — 2024BTCY005
- Anil — 2024BTCY007
- Bharath — 2024BTEC009
- lithin — 2024BTCS001
- hari — 2024BTDS175


### Login fix
The five pre-registered students can log in using their **name or PRN** with password `Spirit@123`. The accounts are seeded automatically when the app starts.

## Faculty and Admin accounts

Faculty:
- Email: `hello@spirit.edu`
- Password: `Hello@726`
- Can view classroom booking schedules, see the account/PRN that made reservations, view classrooms, and make classroom bookings.

Admin:
- Email: `hi@spirit.edu`
- Password: `Hi@726`
- Can view users, view all classroom bookings, see who made reservations, and manage the overall booking system.


## Capacity & concurrency management

Classroom bookings now include the number of students/enrollment requested.

- Each classroom has a maximum capacity.
- Overlapping reservations can use the same classroom only while their combined enrollment stays within capacity.
- SQLite `BEGIN IMMEDIATE` transactions serialize competing booking writes.
- The capacity check and booking insert happen inside the same transaction.
- If two users try to claim the last available seats at the same time, only the request that fits first is committed; the other receives a capacity-unavailable message.
- Existing databases are migrated automatically with `enrollment_count = 1` for older bookings.

- Each individual reservation is limited to a maximum of **5 seats**.

## Website entry point

The Flask website has a proper root entry point at `/`.
Opening the site displays `templates/index.html`, with links to Login and student registration.
Run with `python app.py`.
