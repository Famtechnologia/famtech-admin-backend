# FamTech Admin - SuperAdmin User Management System

A comprehensive Node.js/Express.js application for managing users in the FamTech agricultural platform. This system provides SuperAdmin functionality for user approval, role management, and comprehensive user analytics.

## Features

### 🎯 Core SuperAdmin Features

1. **User Management Dashboard**

   - Total number of users
   - Active/inactive user counts
   - User distribution by roles
   - Real-time statistics

2. **User Roles System**

   - `farmer` - Regular farm users
   - `admin` - Administrative users
   - `viewer` - Read-only access users
   - `superadmin` - Full system access
   - `advisor` - Agricultural advisors

3. **User Registration & Approval Process**

   - Pending user registration queue
   - Approval/rejection workflow with reasons
   - Bulk approval operations
   - Audit trail for all actions

4. **Advanced User Search & Filtering**

   - Search by name, email
   - Filter by role, status
   - Pagination support
   - Sortable results

5. **User Status Management**
   - `active` - Approved and active users
   - `inactive` - Rejected or deactivated users
   - `pending` - Awaiting approval
   - `suspended` - Temporarily suspended users

## Technology Stack

- **Backend**: Node.js with Express.js
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JWT-based (ready for implementation)
- **Security**: bcryptjs for password hashing
- **Validation**: Custom middleware with comprehensive checks

## Project Structure

```
famtech-admin/
├── app/
│   └── app.js                 # Express app configuration
├── config/
│   └── databaseConfig.js      # MongoDB connection setup
├── controller/
│   └── SuperAdminContoller.js # All admin functionality
├── middleware/
│   └── validation.js          # Request validation middleware
├── model/
│   └── User.js               # Enhanced User model with admin features
├── router/
│   └── superAdminRoute.js    # Admin API routes
├── server.js                 # Server entry point
├── package.json
├── config.env.example        # Environment configuration template
└── API_DOCUMENTATION.md      # Comprehensive API documentation
```

## Installation & Setup

### Prerequisites

- Node.js (v16 or higher)
- MongoDB (local or MongoDB Atlas)

### Installation Steps

1. **Clone/Download the project**

   ```bash
   cd "c:\Users\LENOVO\Desktop\famtech admin"
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Environment Configuration**

   ```bash
   copy config.env.example config.env
   ```

   Update `config.env` with your actual values:

   - Database connection string
   - JWT secrets
   - Email configuration (optional)
   - Other environment-specific settings

4. **Start the server**
   ```bash
   npm start
   ```

The server will start on `http://localhost:4000` (or your configured PORT).

## API Endpoints

### Base URL: `/api/admin`

#### User Statistics

- `GET /stats` - Get comprehensive user statistics

#### User Listing & Search

- `GET /users` - Search users with filters and pagination
- `GET /users/all` - Get all users (backward compatibility)
- `GET /users/pending` - Get pending registrations
- `GET /users/role/:role` - Get users by role
- `GET /users/:userId` - Get specific user details

#### User Management

- `POST /users/:userId/approve` - Approve user registration
- `POST /users/:userId/reject` - Reject user registration
- `POST /users/:userId/suspend` - Suspend user account
- `POST /users/:userId/reactivate` - Reactivate user account
- `PUT /users/:userId/role` - Update user role
- `DELETE /users/:userId` - Delete user (soft delete)

#### Bulk Operations

- `POST /users/bulk/approve` - Bulk approve multiple users

## User Model Schema

The enhanced User model includes:

```javascript
{
  // Basic Information
  email: String (required, unique),
  passwordHash: String (required),
  firstName: String,
  lastName: String,
  phoneNumber: String,
  profilePicture: String,

  // System Fields
  role: Enum ['farmer', 'admin', 'viewer', 'superadmin', 'advisor'],
  status: Enum ['active', 'inactive', 'pending', 'suspended'],
  region: String (required),
  language: String (default: 'en'),

  // Verification & Security
  isVerified: Boolean (default: false),
  verificationToken: String,
  refreshTokens: [String],
  passwordResetToken: String,
  passwordResetExpires: Date,

  // Admin Tracking
  lastLogin: Date,
  approvedBy: ObjectId (ref: User),
  approvedAt: Date,
  rejectedBy: ObjectId (ref: User),
  rejectedAt: Date,
  rejectionReason: String,

  // Relations
  WeatherInfo: ObjectId (ref: WeatherForecast),
  farmAssets: [ObjectId] (ref: FarmAsset),

  // Timestamps
  createdAt: Date,
  updatedAt: Date
}
```

## Security Features

- **Role-based Access Control**: Different permission levels for different user roles
- **Input Validation**: Comprehensive validation middleware for all inputs
- **Rate Limiting**: Protection against bulk operation abuse
- **Audit Trail**: Complete tracking of approval/rejection actions
- **Soft Delete**: Users are deactivated rather than permanently deleted
- **Password Security**: bcryptjs hashing with configurable salt rounds

## Performance Optimizations

- **Database Indexes**: Optimized queries for filtering and searching
- **Pagination**: Efficient handling of large user datasets
- **Text Search Index**: Fast full-text search across user fields
- **Aggregation Pipelines**: Efficient statistics calculation

## Example Usage

### Get User Statistics

```javascript
const response = await fetch("/api/admin/stats");
const { statistics } = await response.json();
console.log(statistics.totalUsers); // 150
console.log(statistics.usersByRole); // { farmer: 100, admin: 5, ... }
```

### Search Users

```javascript
const response = await fetch(
  "/api/admin/users?search=john&role=farmer&page=1&limit=10"
);
const { data } = await response.json();
console.log(data.users); // Array of users
console.log(data.pagination); // Pagination info
```

### Approve User

```javascript
const response = await fetch(
  "/api/admin/users/64a1b2c3d4e5f6789012345/approve",
  {
    method: "POST",
    headers: { Authorization: "Bearer jwt-token" },
  }
);
const result = await response.json();
```

## Development Notes

### Authentication Middleware

The routes are prepared for authentication middleware. You'll need to implement:

- JWT token verification
- User session management
- `req.user.id` population

### Future Enhancements

- Real-time notifications for admin actions
- User activity logging
- Advanced reporting and analytics
- Email notifications for approval/rejection
- File upload for profile pictures and documents

## Testing

To test the API endpoints, you can use tools like:

- Postman
- Insomnia
- curl commands
- Frontend applications

## Database Setup

Ensure your MongoDB database is running and accessible. The application will automatically create the necessary collections and indexes on first run.

## Contributing

When extending this system:

1. Follow the existing code structure
2. Add appropriate validation middleware
3. Update API documentation
4. Ensure backward compatibility
5. Add proper error handling

## Support

For questions or issues:

1. Check the API_DOCUMENTATION.md for detailed endpoint information
2. Review the validation middleware for request requirements
3. Examine the User model for available fields and methods

---

**Note**: This is a backend API system. You'll need to create a frontend interface to interact with these endpoints for a complete admin dashboard experience.
"# famtech-admin-backend" 
