# Lost & Found Management System

A comprehensive web-based platform that connects people who have lost items with those who have found them. The system streamlines the process of reporting lost/found items, searching databases, managing claims, and uses intelligent matching algorithms to automatically detect potential matches.

## Overview

Lost & Found is a community-driven platform designed to help reunite people with their belongings. Users can:
- **Report Lost Items** — Post details about items they've lost with descriptions
- **Report Found Items** — Help reunite found items with their owners
- **Smart Matching** — Automatic detection of potential matches between lost and found items
- **Search Database** — Browse and filter through lost and found reports
- **Manage Claims** — Submit, track, and manage claims on found items
- **Contact Owners** — Direct contact information for lost item reporters
- **Admin Dashboard** — Administrative staff can approve/reject claims and manage reports

## Features

### User Features
- **User Authentication** — Secure registration and login with encrypted phone storage
- **Report Items** — Report lost or found items with detailed descriptions
- **Intelligent Matching** — System automatically finds potential matches (5-10 point scoring based on category, location, date, and name similarity)
- **Real-time Notifications** — Bell icon notifications for potential matches and claim updates
- **Search & Filter** — Find items by category, status, date, location, and type
- **Claims Management** — Submit and track claims on found items with approval workflow
  - View button for approved claims to access owner contact info
  - Track claim status (pending/approved/rejected) with visual indicators
- **Contact Information Sharing** — Smart contact info visibility:
  - Lost items: Contact info always visible to help return items
  - Found items: Contact info revealed only after claim approval
  - Approved claimers can view owner contact details
  - Found item owners can view claimer contact details after approval
- **Profile Management** — View and update user profile information with encrypted phone numbers
- **Enhanced Dashboard** — Personal dashboard with stats, notifications, and quick actions
- **Step-by-Step Guidance** — Context-aware guidance for claiming found items or contacting lost item reporters
- **Status Management** — Mark lost items as found or found items as returned
- **Responsive Design** — Modern gradient-based UI that works seamlessly on all devices

### Admin Features
- **Claim Approval/Rejection** — Review and approve/reject user claims with admin notes
  - Full item details displayed in claim review interface
  - Contact information sharing automatically triggered upon approval
- **Report Management** — View and manage all lost and found reports with complete item details
- **User Management** — View and manage user accounts
- **Staff Management** — Create and manage staff accounts
- **Dashboard Analytics** — View system-wide statistics and activity

### Staff Features
- **Report Verification** — Verify and manage item reports with full item details
- **Claim Processing** — Assist with claim verification and approval
  - Complete item information displayed during claim review
- **User Support** — Help users with claims and inquiries

## Technology Stack

- **Backend:** PHP 7+/8+ with PDO
- **Database:** MySQL 5.7+
- **Frontend:** HTML5, CSS3, Bootstrap 5.3.3
- **Icons:** Bootstrap Icons 1.11.3
- **Additional:** JavaScript (Bootstrap Bundle)
- **Encryption:** AES-256-CBC for phone numbers

## Project Structure

```
Lost-Found/
├── index.php                          # Root entry point (redirects to login)
├── pages/
│   ├── login.php                      # User login page
│   ├── register.php                   # User registration
│   ├── logout.php                     # Logout handler
│   ├── admin_login.php                # Admin login
│   ├── staff_login.php                # Staff login
│   ├── user/
│   │   ├── userdash.php               # User dashboard with notifications
│   │   ├── userprofile.php            # User profile management
│   │   ├── lost.php                   # Report lost item
│   │   ├── found.php                  # Report found item
│   │   ├── search.php                 # Search items
│   │   ├── claim.php                  # Manage claims & reports
│   │   ├── match_results.php          # View potential matches
│   │   ├── item_detail.php            # Item detail view with guidance
│   │   ├── edit_profile.php           # Edit user profile
│   │   ├── changeuserpassword.php     # Change password
│   │   └── includes/
│   │       └── header.php             # User header component
│   ├── admin/
│   │   ├── admin_dashboard.php        # Admin dashboard
│   │   ├── admin_staff.php            # Manage staff
│   │   ├── admin_users.php            # Manage users
│   │   ├── admin_claim.php            # Manage claims
│   │   ├── admin_report.php           # Manage reports
│   │   └── includes/
│   │       └── header.php             # Admin header component
│   └── staff/
│       ├── staff_dashboard.php        # Staff dashboard
│       ├── staff_claim.php            # Manage claims
│       ├── staff_report.php           # Manage reports
│       └── includes/
│           └── header.php             # Staff header component
├── config/
│   ├── userconfig.php                 # User database config
│   ├── adminconfig.php                # Admin configuration
│   └── staffconfig.php                # Staff configuration
├── includes/
│   └── functions.php                  # Helper functions (encrypt/decrypt phone, sanitization)
├── assets/
│   ├── style.css                      # Global styles
│   └── admin-style.css                # Admin styles
├── sql/
│   ├── schema.sql                     # Database schema
│   ├── config.sql                     # Database configuration
│   ├── scripts.sql                    # Stored procedures & triggers
│   ├── privileges.sql                 # Database user privileges
│   ├── syntheticrecords.sql           # Sample data for testing
│   └── add_matching_feature.sql       # Matching algorithm setup
└── README.md                          # This file
```

## Database Schema

### Main Tables
- **User** — User accounts (username, email, encrypted phone, password, etc.)
- **LostItem** — Lost item reports (item_name, description, status, etc.)
- **FoundItem** — Found item reports (item_name, description, status, etc.)
- **ClaimRequest** — Claims linking users to found items (status: pending/approved/rejected) with separate nullable approver foreign keys: `admin_approver_id` and `staff_approver_id` (replaces prior polymorphic `approver_id` + `approver_type` design)
- **Admin** — Admin accounts
- **Staff** — Staff accounts

### Status Values
- **LostItem.status:** `pending`, `claimed`
- **FoundItem.status:** `available`, `returned`
- **ClaimRequest.status:** `pending`, `approved`, `rejected`

### Stored Procedures
- `GetUserById()` — Retrieve user information
- `ReportFoundItem()` — Insert found item report
- `ReportLostItem()` — Insert lost item report
- `SubmitClaim()` — Submit a claim request
- `ViewPendingClaimsWithFoundDetails()` — Get pending claims with associated found item
- `ViewProcessedClaimsWithDetails()` — Approved / rejected claims incl. approver (admin/staff)
- `ViewStaffProcessedClaims()` — Processed claims approved by a specific staff member
- `ApproveClaim()` — Branching logic writes to `admin_approver_id` or `staff_approver_id`
- `RejectClaim()` — Branching logic writes to `admin_approver_id` or `staff_approver_id`
- `GetUserLostItems()` — Get user's lost items
- `GetUserFoundItems()` — Get user's found items
- `GetUserLostItemsCount()` — Count user's lost items
- `GetUserFoundItemsCount()` — Count user's found items
- `GetUserClaims()` — Get user's claim requests with item details
- `GetUserClaimsCount()` — Count user's total claims
- `FindPotentialMatches()` — Intelligent matching algorithm for lost/found items
- `GetUserMatchCount()` — Count potential matches for user
- `GetUserClaimNotifications()` — Recent approved/rejected claim decisions for user
- `GetFoundItemClaimNotifications()` — Notifications for owners whose found items were claimed
- `SearchItems()` — Search items with filters
- `UpdateUserProfile()` — Update user profile information

## Installation

### Prerequisites
- PHP 7.4+ or PHP 8+
- MySQL 5.7+
- A web server (Apache, Nginx, etc.)
- MAMP/LAMP/LEMP stack or similar

### Setup Steps

1. **Clone/Extract** the project to your web server root:
   ```bash
   git clone https://github.com/SupaOhm/Lost-and-Found-Management-System.git
   cd Lost-Found
   ```

2. **Create Database:**
   ```bash
   mysql -u root -p < sql/schema.sql
   mysql -u root -p lost_found_db < sql/config.sql
   mysql -u root -p lost_found_db < sql/scripts.sql
   mysql -u root -p lost_found_db < sql/privileges.sql
   mysql -u root -p lost_found_db < sql/syntheticrecords.sql  # Optional: sample data
   ```

3. **Configure Database Connections:**
   Edit `config/userconfig.php`, `config/adminconfig.php`, and `config/staffconfig.php`:
   ```php
   $host = 'localhost';
   $port = '8889';  // Adjust for your setup (default MySQL: 3306)
   $dbname = 'lost_found_db';
   $user = 'user_user';  // or admin_user, staff_user
   $password = 'user_password';  // Set in privileges.sql
   ```

4. **Set Encryption Key (Optional):**
   Phone numbers are encrypted using AES-256-CBC. Set the environment variable:
   ```bash
   export LF_ENCRYPT_KEY="your_32_character_encryption_key"
   ```
   If not set, a default key is used (not recommended for production).

5. **Access the Application:**
   - User Login: `http://localhost/Lost-Found/`
   - Admin Login: `http://localhost/Lost-Found/pages/admin_login.php`
   - Staff Login: `http://localhost/Lost-Found/pages/staff_login.php`

### Database Users & Privileges
The system uses three separate database users with different privilege levels:
- **user_user** — Limited privileges for regular users (SELECT, INSERT, DELETE on items/claims, UPDATE on User/LostItem/FoundItem)
- **staff_user** — Extended privileges for staff (full access to items and claims)
- **admin_user** — Full privileges for administrators

Passwords are set in `sql/privileges.sql`.

### Default Credentials (Development)
Check `sql/syntheticrecords.sql` for test users created during database initialization.

## Key Features in Detail

### User Dashboard
- View stats on lost items, found items, potential matches, and active claims
- Real-time notification bell icon with count badge
- Quick access to report new items, search database, and view matches
- Recent activity feed

### Claims & Reports Management
- **Left Panel:** View all your reported items (lost/found) with filters and pagination
  - Filter by Type (Lost/Found) and Status (Open/Closed)
  - Delete reports
  - Mark items as found/returned
  - View item details with one-click access
- **Right Panel:** View your claims on other users' found items with pagination
  - Track claim status (pending/approved/rejected)
  - Delete claims
  - View admin notes
  - **View Button:** For approved claims, directly access item detail page to see owner contact info
- **Profile Stats:** View counts of your lost items, found items, and claims

### Search & Discovery
- **Intelligent Matching** — Automatic potential match detection based on:
  - Category match (2 points)
  - Location match (2 points)
  - Date proximity within 30 days (2 points)
  - Similar item names (4 points)
  - Minimum 2 points required for match notification
- Search through all lost and found items in the database
- Filter by category, location, date range, type, and status
- View item details with step-by-step guidance:
  - **For Lost Items:** Contact info visible, guidance on reaching out to owner
  - **For Found Items:** Submit claim with proof of ownership
- Submit claims directly from item detail page

### Item Detail Page
- **Context-Aware Guidance:**
  - **Lost Items:** "How to Contact" guidance with 3 steps (verify info → contact owner → share/return)
  - **Found Items:** "How to Claim" guidance with 3 steps (verify details → submit claim → prove ownership)
- **Smart Contact Information Display:**
  - **Lost items:** Reporter's email and phone always visible (decrypted for display) to facilitate returns
  - **Found items (non-owner view):** Contact info protected until claim approval
  - **Found items (approved claimer):** Owner contact info becomes visible with approval notification
  - **Found items (owner view):** Claimer contact info displayed after approving their claim
- **Seamless Claim Workflow:**
  - Claim submission integrated into item detail page
  - Automatic notification when viewing own items
  - Direct access to approved claim details via view button
- **Modern UI:** Gradient backgrounds, rounded cards, icon badges, status-aware displays

### Admin Panel
- **Notification System** — Real-time bell icon notifications for:
  - New potential matches detected
  - Claim status updates (approved/rejected within 7 days)
  - Notification count badge
- Dashboard with system-wide statistics
- View all reports and claims in the system
- Approve or reject claims with admin notes
- Manage user accounts and staff members

## Usage

### For Users

1. **Register** — Create an account on the registration page (provide name, email, phone, password)
2. **Report Lost Item** — Navigate to "Report Lost Item" and fill in details (name, category, location, date, description)
3. **Report Found Item** — Navigate to "Report Found Item" and describe the item
4. **View Matches** — Check notification bell for potential matches between your lost items and found reports
5. **Search** — Use the search feature to find items matching your needs
6. **Claim** — Submit a claim on a found item if you believe it's yours
7. **Track Claim Status** — Monitor your claim from the "Claims & Reports" page
8. **Access Owner Info** — Once your claim is approved, click the view button to see owner contact details
9. **Contact** — For lost items, directly contact the reporter using provided email/phone; for approved found item claims, contact info becomes visible
10. **Manage** — Track your claims and reports from the dashboard, mark items as found/returned, and view claimer contact info after approving claims

### For Admins

1. **Login** — Use admin credentials to access the admin panel
2. **Review Claims** — Review pending claims and approve/reject them with notes
3. **Manage Reports** — Monitor and manage all system reports
4. **Manage Users** — View and manage user accounts
5. **Manage Staff** — Create and manage staff accounts

## Security Features

- **Password Hashing** — All passwords hashed using PHP's password_hash (bcrypt)
- **Phone Number Encryption** — Phone numbers encrypted using AES-256-CBC with automatic decryption for display. Current implementation derives a static IV from the key; recommended enhancement: store a random 16‑byte IV prefixed to the ciphertext (e.g. `base64(iv + encrypted)`), then extract for decryption.
- **Prepared Statements** — PDO prepared statements prevent SQL injection
- **Session Management** — Secure session-based authentication
- **Input Validation** — Server-side validation and sanitization
- **Ownership Checks** — Users can only modify their own reports/claims
- **Privacy-First Contact Sharing** — Contact information only revealed when appropriate (claim approval)
- **Database Privilege Separation** — Different DB users with minimal required privileges

## UI/UX Features

- **Responsive Design** — Optimized for all screen sizes
- **Modern Gradient UI** — Gradient backgrounds, rounded corners, shadow effects
- **Bootstrap 5.3.3** — Clean, professional interface with Bootstrap Icons
- **Intuitive Navigation** — Easy-to-use menu structure
- **Visual Feedback** — Color-coded status badges, hover effects, confirmation dialogs
- **Aesthetic Components** — Gradient buttons, circular badges, smooth transitions
- **Contextual Guidance** — Step-by-step instructions based on item type
- **Notification System** — Bell icon with badge counter for real-time updates
- **Pagination** — Clean pagination for large lists of items
- **Accessibility** — Semantic HTML, ARIA labels, keyboard navigation

## Known Limitations & Future Improvements

- Photo upload feature for items (currently text-only descriptions)
- Email notifications not yet implemented
- SMS notifications could be added using encrypted phone numbers
- Random IV adoption for phone encryption (improves cryptographic robustness)
- Two-factor authentication for enhanced security
- Advanced analytics dashboard with charts and trends
- Mobile app version under consideration
- Integration with campus security/police departments
- Geolocation-based proximity matching

## Contributing

This project is in active development. Contributions are welcome!

## License

This project is licensed under the MIT License.

## Support & Contact

For issues, feature requests, or support:
- Create an issue on the repository
- Contact: ohm.supakornth@gmail.com

## Project Status

🔄 **In Active Development** — Features and improvements are being added regularly.

## Recent Updates (Changelog Snapshot)
**Schema:** Introduced separate `admin_approver_id` and `staff_approver_id` columns in `ClaimRequest` replacing polymorphic approver pattern; added trigger `BeforeFoundItemDelete` to automatically remove related `ClaimRequest` rows before a `FoundItem` delete.

**Triggers:**
- `BeforeUserDelete` — Cleans up related claims, found items, lost items.
- `BeforeFoundItemDelete` — Cleans up claims for the specific found item.

**Procedures Added / Revised:**
- `ViewProcessedClaimsWithDetails`, `ViewStaffProcessedClaims` reflect new approver column structure.
- `GetUserClaimNotifications`, `GetFoundItemClaimNotifications` power dashboard bell notifications.
- `ApproveClaim` / `RejectClaim` now route approver IDs into the correct dedicated column.

**Report Filtering:** Admin and Staff report pages now support item type (lost/found) and status (pending/available vs claimed/returned) filters with active filter display and clear button.

**Deletion Logic:** Application layer simplified—claim cleanup for found item deletion now handled by trigger instead of manual PHP deletion cascade.

**Security Note:** Encryption section documents recommendation to migrate to per‑record random IV for phone number storage.

**Consistency:** All view item and claim operations use prepared statements, reducing SQL injection surface.

_Keep this section updated as further changes land._
