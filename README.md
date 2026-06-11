# Smart Asset Management and Resource Allocation Platform

## Project Overview
The Smart Asset Management and Resource Allocation Platform is a full-stack web application designed for the Cultural Council of IIT Roorkee. It provides a centralized dashboard for tracking, managing, reserving, and maintaining assets such as cameras, lighting systems, audio equipment, stage props, costumes, and event infrastructure. The platform enables administrators to manage inventories, approve or reject booking requests, monitor item checkouts and check-ins using QR codes, and trace system-wide operations, while consumer users can browse assets, check availability, and request bookings.

---

## Technology Stack
- **Framework**: Next.js 15 (App Router) with TypeScript
- **Styling**: Tailwind CSS v4 with PostCSS
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: NextAuth.js supporting Credentials (email and password) and OAuth providers (Google and GitHub)
- **Utilities**: Recharts (for analytics dashboards), html5-qrcode (for scanner integration), and qrcode (for label rendering)

---

## Setup Instructions

### 1. Configure Environment Variables
Copy the configuration template file `.env.example` to create your `.env` file:
```bash
cp .env.example .env
```
Open the `.env` file and set the required environment variables:
- `DATABASE_URL`: Connection string for your PostgreSQL database.
- `NEXTAUTH_SECRET`: Secret string used to sign session cookies.
- `NEXTAUTH_URL`: Canonical URL of the application (e.g., http://localhost:3000).
- `AUTH_GOOGLE_ID` / `AUTH_GOOGLE_SECRET`: Google OAuth app credentials.
- `AUTH_GITHUB_ID` / `AUTH_GITHUB_SECRET`: GitHub OAuth app credentials.
- `CLOUDINARY_CLOUD_NAME` / `CLOUDINARY_API_KEY` / `CLOUDINARY_API_SECRET`: Cloudinary API parameters for hosting profile image uploads.

### 2. Start PostgreSQL Database
If you do not have a running database server, you can use the provided Docker Compose file to launch a local PostgreSQL container:
```bash
docker-compose up -d
```

### 3. Generate Database Client and Sync Schema
Run the following commands to generate the database client and push the schema structure to your PostgreSQL database:
```bash
npx prisma generate
npx prisma db push
```

### 4. Seed Database
Initialize the database with mock administrators, members, assets, bookings, and audit records:
```bash
npx prisma db seed
```
Default accounts seeded:
- **Admin**: admin@cultural.iitr.ac.in (Password: AdminPassword123)
- **Member**: rohan@cultural.iitr.ac.in (Password: MemberPassword123)

---

## Running the Application

### Development Server
To launch the local development server:
```bash
npm run dev
```
Open your browser and navigate to http://localhost:3000 to access the platform.

### Production Build
To create an optimized production build:
```bash
npm run build
npm run start
```

---

## Feature List

### 1. Authentication and Onboarding
- Registration and login using credentials (email and password).
- Onboarding redirect that forces first-time Google or GitHub users to complete their profile setup before accessing the dashboard.
- Automatic routing based on user role (Admin or Consumer).

### 2. Asset Discovery and Reservation
- Category-based catalog filter for browsing council items.
- Live inventory counts preventing overbooking.
- Booking date constraint checks that prevent booking items in the past or picking a return date earlier than checkout.

### 3. Checkout and Return Circulation
- Camera-based QR code scanner built to check out (issue) and check in (return) assets.
- Integrated maintenance log reporting triggered when items are returned damaged.

### 4. Admin Management Console
- Role management panel allowing admins to promote consumers to admin status or demote secondary admins safely.
- Prevention controls that restrict admins from demoting themselves to avoid accidental lockout.

### 5. Activity Audit Logging
- Interactive system log console with KPI stats cards showing total records, asset alterations, booking occurrences, and profile events.
- Full-text search queries matching users, emails, actions, and details.
- Structured display tables displaying diff comparisons for updated assets, colored flags for returned condition states, and rejection reasons.
- Collapsible JSON inspection views with copy-to-clipboard actions.
- Client-side data exporters for CSV and JSON logs.
- Paginated results table limiting views to 10 lines per page.
