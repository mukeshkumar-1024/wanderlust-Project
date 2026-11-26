# Wanderlust

A full-stack accommodation listing and booking platform, in the spirit of Airbnb. Users can browse stays, list their own, leave reviews, book a stay and download a PDF ticket. Admins get a dashboard to manage everything.

**Live demo:** https://wanderlust-app-xh56.onrender.com/listings

> Hosted on Render's free tier, which sleeps when idle — the first load can take up to a minute. After that it is quick.

**Stack:** Node.js · Express 5 · MongoDB (Mongoose) · EJS · Passport.js · Cloudinary · Mapbox · PDFKit · Nodemailer

---

## Features

| Area | What it does |
| --- | --- |
| **Listings** | Full CRUD — create, browse, edit and delete stays, with image upload |
| **Images** | Uploaded through Multer and stored on Cloudinary, not in the repo |
| **Maps** | Addresses are geocoded with the Mapbox SDK and shown on an interactive map |
| **Reviews** | Logged-in users can review a listing; authors can delete their own |
| **Authentication** | Passport.js local strategy with `passport-local-mongoose` for hashing and salting |
| **Sessions** | Stored in MongoDB via `connect-mongo`, so logins survive a restart |
| **Bookings** | Book a listing, view the booking, and download a PDF ticket generated with PDFKit |
| **Email** | Booking confirmations sent through Nodemailer |
| **Admin panel** | Dashboard listing all bookings, listings, users and reviews, each deletable |
| **Validation** | Request bodies validated server-side with Joi before they reach the database |
| **Error handling** | Central `ExpressError` class and an async wrapper so no route swallows an error |

## Project structure

```
app.js              Express setup, session store, passport config, route mounting
routes/             listings, reviews, user, booking, admin
controllers/        route logic, kept out of the route files
models/             Mongoose schemas
views/              EJS templates (ejs-mate layouts)
utils/              PDF generation, email sending, error helpers
init/               seed data for local development
middleware.js       auth guards, ownership checks, Joi validation
cloudConfig.js      Cloudinary storage config
```

## Run it locally

**Prerequisites:** Node.js 18 or newer, and a MongoDB database (local or a free MongoDB Atlas cluster).

### 1. Install

```bash
git clone https://github.com/mukeshkumar-1024/wanderlust-Project.git
cd wanderlust-Project
npm install
```

### 2. Create a `.env` file

The app reads everything from environment variables — nothing is hardcoded. Copy `.env.example` to `.env` and fill it in:

```bash
cp .env.example .env
```

| Variable | Where to get it |
| --- | --- |
| `ATLASDB_URL` | MongoDB connection string (Atlas, or `mongodb://127.0.0.1:27017/wanderlust` locally) |
| `SECRET` | Any long random string — used to sign sessions |
| `CLOUD_NAME`, `CLOUD_API_KEY`, `CLOUD_API_SECRET` | [Cloudinary](https://cloudinary.com) dashboard (free tier) |
| `MAPBOX_TOKEN` | [Mapbox](https://account.mapbox.com) access token (free tier) |
| `EMAIL_USER`, `EMAIL_PASS` | Gmail address and an [app password](https://support.google.com/accounts/answer/185833) — not your account password |

### 3. Seed some data (optional)

```bash
node init/index.js
```

This wipes the listings collection and inserts the sample stays in `init/data.js`.

### 4. Start

```bash
npm run dev     # nodemon, restarts on change
# or
npm start
```

Open http://localhost:8080

### Troubleshooting

| Problem | Cause |
| --- | --- |
| `MongooseError: The uri parameter must be a string` | `ATLASDB_URL` is missing from `.env` |
| Connection hangs on Atlas | Your IP is not whitelisted in Atlas → Network Access |
| Images upload but do not appear | Cloudinary credentials wrong, or the free quota is used up |
| Map does not render | `MAPBOX_TOKEN` missing or expired |
| Email fails with "Invalid login" | Gmail needs an app password, not the normal password |

## Things I would still like to add

- Tests — there are none yet
- Search and filtering on listings
- Payment integration for bookings
- Rate limiting on auth routes

Issues and pull requests are welcome.

## License

MIT — see [LICENSE](LICENSE).

## Authors

Built as a BCA final year project (2025-26) by:

- **Mukesh Kumar Rar** — [GitHub](https://github.com/mukeshkumar-1024) · [LinkedIn](https://linkedin.com/in/mukesh-choudhary-64a9a4378)
- **Nasir Husain Gouri**
