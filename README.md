========================================
COMMENTS SERVICE API
========================================

A comprehensive RESTful API service for managing comments with advanced authentication and role-based access control.
Built with Flask and Microsoft SQL Server, this service provides a complete comment management system with external authentication integration via Plymouth University's API.

----------------------------------------
PROJECT DESCRIPTION
----------------------------------------
This project implements a secure and scalable API for comment management. It includes external authentication, role-based access, CRUD operations, Swagger documentation, and database integration with stored procedures and triggers.

----------------------------------------
KEY FEATURES
----------------------------------------
- External Authentication: Integration with Plymouth University's authentication API
- Role-Based Access Control: Admin and user permissions
- Comment Management: Create, read, update, delete (soft delete)
- Pagination: Efficient data retrieval
- Swagger Documentation: Interactive and comprehensive
- Security: Supports Bearer token and Basic authentication
- Database: SQL Server with SQLAlchemy ORM
- Data Validation: Input validation and structured error handling
- Stored Procedures and Triggers: For advanced database operations and logging

----------------------------------------
LIVE DEMO
----------------------------------------
Hosted Application:
https://web.socem.plymouth.ac.uk

----------------------------------------
SETUP INSTRUCTIONS
----------------------------------------

PREREQUISITES
- Python 3.8+
- Microsoft SQL Server
- ODBC Driver 17 for SQL Server
- Git

INSTALLATION STEPS

1. Clone the Repository:
   git clone <repository-url>
   cd comments-service

2. Create Virtual Environment:
   python -m venv venv
   (Windows) venv\Scripts\activate
   (Linux/Mac) source venv/bin/activate

3. Install Dependencies:
   pip install -r requirements.txt

4. Create a .env file in the project root and include:

   DB_HOST=dist-6-505.uopnet.plymouth.ac.uk
   DB_NAME=comments_service
   DB_USER=your_username
   DB_PASSWORD=your_password
   SECRET_KEY=your-secret-key-here
   EXTERNAL_AUTH_API=https://web.socem.plymouth.ac.uk/COMP2001/auth/api/users

----------------------------------------
DATABASE SETUP
----------------------------------------

1. Connect to SQL Server:
   sqlcmd -S dist-6-505.uopnet.plymouth.ac.uk -U your_username -P your_password

2. Execute Initialization Script:
   :r db/init_db.sql

   This will create:
   - CW2 schema
   - users and comments tables
   - comment_logs audit table
   - Stored procedures: AddComment, GetComments, UpdateComment, ArchiveComment
   - Triggers for audit logging
   - Sample test data

3. Verify Setup:
   SELECT * FROM CW2.users;
   SELECT * FROM CW2.comments;
   EXEC CW2.GetComments @include_archived = 0, @page_number = 1, @page_size = 10;

----------------------------------------
RUNNING THE APPLICATION
----------------------------------------

OPTION 1: DOCKER (Recommended)
1. Build and Run:
   docker-compose up --build

2. Access:
   - Local: http://localhost:5000
   - Swagger UI: http://localhost:5000/swagger/
   - Health Check: http://localhost:5000/health

3. Benefits:
   - Consistent environment
   - Auto reload on changes
   - No need for local SQL Server setup
   - Easy cleanup and rebuild

OPTION 2: LOCAL PYTHON ENVIRONMENT
1. Start the Server:
   python app.py

2. Access:
   - Local: http://localhost:5000
   - Swagger: http://localhost:5000/swagger/
   - API Spec: http://localhost:5000/apispec.json

----------------------------------------
SWAGGER DOCUMENTATION
----------------------------------------

ACCESS:
Local: http://localhost:5000/swagger/
Production: https://web.socem.plymouth.ac.uk/swagger/

USAGE:
1. Open Swagger UI
2. Click "Authorize"
3. Choose Authentication Method:
   - Basic Auth (email + password)
   - Bearer Token (JWT)
4. Test endpoints using "Try it out"

FEATURES:
- Interactive testing
- Example requests and responses
- Authentication testing
- Schema documentation and validation

----------------------------------------
API ENDPOINTS
----------------------------------------

COMMENTS:
GET /api/comments        -> Get all comments (paginated)
POST /api/comments       -> Create new comment (auth required)
GET /api/comments/{id}   -> Retrieve specific comment
PUT /api/comments/{id}   -> Update comment (auth required)
DELETE /api/comments/{id}-> Soft delete comment (auth required)

USERS:
POST /api/users          -> Create new user (admin only)

----------------------------------------
AUTHENTICATION
----------------------------------------

The API uses Plymouth University's authentication system. Users authenticate via Basic Auth or Bearer Token.

METHODS:
1. Basic Authentication:
   curl -u "your-email@plymouth.ac.uk:your-password" http://localhost:5000/api/comments

2. Bearer Token:
   curl -H "Authorization: Bearer your-token" http://localhost:5000/api/comments

AUTHENTICATION FLOW:
1. Client sends credentials.
2. Server validates with Plymouth's API.
3. User role checked in CW2.users.
4. user_id and user_role added to request context.

ROLES:
- Admin: Manage all comments and users.
- User: Manage own comments only.

PROTECTED ENDPOINTS:
POST /api/comments
PUT /api/comments/<id>
DELETE /api/comments/<id>
POST /api/users (admin only)
GET /api/users (admin only)
PUT /api/admin/comments/restore/<id> (admin only)

----------------------------------------
ERROR HANDLING
----------------------------------------
The API returns JSON responses with clear error messages and appropriate HTTP status codes.

----------------------------------------
GITHUB COLLABORATION
----------------------------------------

SHARED WITH:
- @mjread (Supervisor)
- @haoyiwang25 (Collaborator)

GUIDELINES:
- Submit all changes via Pull Requests
- All PRs require review before merging
- Use feature branches
- Report issues through GitHub Issues

----------------------------------------
LICENSE
----------------------------------------
All rights reserved.

----------------------------------------
END OF FILE
----------------------------------------
