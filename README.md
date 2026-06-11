# Swap

A full-stack web application for exchanging skills and knowledge between users. Built with Flask, SQLAlchemy, Bootstrap, and Socket.IO.

### User Features

- **User Registration & Authentication**: Secure registration and login with password hashing

- **Profile Management**: Create and edit profiles with name, location, photo, availability, and privacy settings

- **Skill Listing**: Add skills you offer or want to learn with proficiency levels (beginner/intermediate/expert)

- **Search & Discovery**: Find users by skill name, skill type, user name, or location with paginated results and autocomplete

- **Swap Requests**: Send skill swap requests proposing a trade between your offered skill and another user's offered skill

- **Swap Lifecycle**: Track requests through pending, accepted, rejected, cancelled, and completed states

- **Real-time Chat**: WebSocket-based messaging for accepted swaps with system notifications

- **Feedback & Rating**: Rate swap partners 1-5 stars with optional comments after completed swaps

- **Real-time Notifications**: Instant Socket.IO push for new requests, acceptances, rejections, completions, and broadcast messages

- **Account Deletion**: Users can delete their own account

### Admin Features

- **Dashboard**: Overview with total/active/banned users, pending/completed swaps, approved/pending skills, recent activity

- **User Management**: View, search, filter, and ban/unban user accounts

- **Skill Moderation**: Approve or reject skill submissions with reason

- **Swap Monitoring**: View all swap requests filtered by status

- **Platform Messages**: Send real-time broadcast messages (info/warning/success/danger) to all connected users

- **Analytics**: Configurable time-period metrics including new users, swaps, completion rate, top skills, and growth trends

- **CSV Reports**: Downloadable reports for users, swaps, feedback, and activity

- **Admin Profile**: View admin details and quick links

## Tech Stack

- **Backend**: Python Flask 2.3.3
- **Database**: MySQL with SQLAlchemy ORM
- **Frontend**: HTML, CSS, JavaScript with Bootstrap 5.3
- **Real-time**: Flask-SocketIO with Socket.IO 4.7 client
- **Authentication**: Flask-Login with bcrypt password hashing
- **File Upload**: Secure file handling for profile photos (max 16MB)
- **Logo**: Custom SVG brand logo

## Prerequisites

- Python 3.8+
- MySQL 8.0+ (or MariaDB)
- pip (Python package manager)

## Installation

### 1. Clone the Repository

```
git clone <repository-url>
cd swap
```

### 2. Set Up Virtual Environment

```
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```
pip install -r requirements.txt
```

### 4. Database Setup

```
-- Create the database
CREATE DATABASE swap;
USE swap;

-- The application will automatically create all tables on first run
```

### 5. Environment Configuration

Copy the example environment file and configure it:

```
cp env.example .env
```

Edit `.env` with your database credentials:

```
FLASK_ENV=development
SECRET_KEY=your-secret-key-change-in-production
DATABASE_URL=mysql://username:password@localhost/swap
```

### 6. Run the Application

```
python run.py
```

The application will be available at `http://localhost:5005`

## Project Structure

```
swap/
├── .env                        # Environment configuration
├── .flaskenv                   # Flask CLI configuration
├── app/
│   ├── __init__.py             # Flask app factory, extensions, blueprints
│   ├── models/
│   │   ├── user.py             # User model (Flask-Login)
│   │   ├── skill.py            # Skill model
│   │   ├── user_skill.py       # User-Skill relationship
│   │   ├── swap_request.py     # Swap request model
│   │   ├── feedback.py         # Feedback/rating model
│   │   ├── availability.py     # Availability slots model
│   │   ├── admin.py            # Admin model + default admin creator
│   │   └── chat.py             # Chat message model
│   ├── routes/
│   │   ├── main.py             # Homepage, about, contact
│   │   ├── auth.py             # Registration, login, logout, API validators
│   │   ├── users.py            # Profiles, skills, search, account deletion
│   │   ├── swaps.py            # Swap lifecycle, chat API, SocketIO events
│   │   ├── admin.py            # Dashboard, management, analytics, CSV reports, messages
│   │   └── feedback.py         # Feedback CRUD, reviews, API endpoints
│   ├── templates/
│   │   ├── base.html           # Base layout (Bootstrap 5, Font Awesome, SocketIO)
│   │   ├── index.html          # Landing page
│   │   ├── auth/               # Login, registration, admin login
│   │   ├── users/              # Profile, edit profile, search, view user
│   │   ├── swaps/              # My swaps, view swap, chat
│   │   ├── admin/              # Dashboard, user/skill/swap management, messages, analytics
│   │   └── feedback/           # User reviews
│   ├── static/
│   │   ├── logo.svg             # Custom brand logo
│   │   ├── css/style.css       # Custom styles
│   │   ├── js/main.js          # SocketIO, form validation, autocomplete, chat, notifications
│   │   └── uploads/            # User profile photo uploads
│   └── utils/
│       └── validators.py       # Form validation and sanitization
├── requirements.txt            # Python dependencies
├── run.py                      # Application entry point (port 5005)
├── check_admin.py              # Check/create admin utility
├── create_admin.py             # Create admin utility
└── README.md                   # This file
```

## Configuration

### Database Configuration

The application uses MySQL. Update the `DATABASE_URL` in your `.env` file:

```
DATABASE_URL=mysql://username:password@localhost/swap
```

### File Upload Configuration

Profile photos are stored in `app/static/uploads/`. Ensure this directory is writable. Maximum file size is configured via `MAX_CONTENT_LENGTH` (default 16MB).

### Admin Access

Default admin credentials (auto-created on first run):

- Email: `admin@swap.com`
- Password: `admin123`

**Important**: Change these credentials in production!

## Usage

### For Users

1. **Register**: Create an account with your name, email, password, and location
2. **Complete Profile**: Add skills (offered/wanted), set availability, upload a profile photo
3. **Search**: Find users by skill name, name, or location
4. **Send Requests**: Propose skill swaps by selecting one of your offered skills and one of theirs
5. **Chat**: Communicate in real-time once a swap is accepted
6. **Complete & Rate**: Mark swaps as complete and leave feedback (1-5 stars)

### For Admins

1. **Login**: Use `/admin/login` with admin credentials
2. **Dashboard**: View platform statistics at a glance
3. **Manage Users**: Search, filter, and ban/unban accounts
4. **Moderate Skills**: Approve or reject skill submissions
5. **Monitor Swaps**: Track all swap requests by status
6. **Send Messages**: Broadcast real-time notifications to all users
7. **Analytics**: View time-period metrics and growth trends
8. **Export Reports**: Download CSV reports for users, swaps, feedback, and activity

## Database Schema

### Core Tables

- **users**: User accounts and profiles (name, email, password, location, photo, availability, privacy, ban status)
- **skills**: Available skills (name, description, category, approval status)
- **user_skills**: Many-to-many with skill type (offered/wanted) and proficiency level
- **swap_requests**: Skill swap requests with status lifecycle (pending, accepted, rejected, completed, cancelled)
- **feedback**: Ratings (1-5) and comments linked to completed swaps
- **availability**: Per-day time slots for user availability
- **admins**: Admin accounts with roles
- **chat_messages**: Real-time chat messages per swap (including system messages)

### Key Relationships

- Users have many skills (offered and wanted)
- Swap requests connect two users with specific offered skills
- Feedback links rater, rated user, and completed swap
- Availability slots belong to users
- Chat messages belong to a swap, sent by a user or system

## API Endpoints

The application exposes several JSON API endpoints for frontend interactivity:

- `GET /api/skills/search?q=<query>` — Skill autocomplete
- `GET /api/users/<id>/skills` — Get user's skills
- `GET /api/check-email?email=<email>` — Email availability check
- `GET /api/validate-password?password=<pass>` — Password strength validation
- `GET /api/swaps/pending-count` — Pending swap count for current user
- `GET /api/swaps/active-count` — Active swap count for current user
- `GET /api/swap/<id>/messages` — Chat messages for a swap
- `POST /api/swap/<id>/messages` — Send a chat message
- `GET /api/user/<id>/rating` — User's average rating
- `GET /api/swap/<id>/can-rate` — Check if user can rate a swap
- `GET /api/feedback/<id>` — Get feedback details
- `GET /api/admin/stats` — Admin dashboard statistics

## Deployment

### Local Development

```
python run.py
```

Runs on `http://localhost:5005` with Socket.IO support.

### Production Deployment

1. Set `FLASK_ENV=production` in environment
2. Use a production-grade server (Gunicorn + eventlet)
3. Set up a reverse proxy (Nginx) with WebSocket support
4. Configure SSL certificates
5. Set up database backups

## Version History

- **v1.0.0**: Initial release with core functionality
- **v1.1.0**: Added real-time notifications
- **v1.2.0**: Enhanced admin dashboard
- **v1.3.0**: Improved search functionality

## License

MIT
