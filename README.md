# Movies App Assignment

A full‑stack movies management app with authentication, CRUD, image upload, and a responsive UI. The backend is Express.js with simple JSON storage; the frontend is a Vite + React app styled for mobile and desktop.

## Features
- **Auth**: Register, login, JWT-protected routes (`/api/auth/*`).
- **Movies CRUD**: Create, list, update, delete.
- **Image Uploads**: Poster uploads via `multer` saved in `public/uploads/`.
- **Pagination**: Server-side pagination for lists.
- **Responsive UI**: Mobile-first create/edit flows with drag-and-drop upload.

## Tech Stack
- **Backend**: Node.js, Express, `multer`, `express-validator`, `jsonwebtoken`.
- **Frontend**: React, Vite, React Router, React Dropzone.
- **Storage**: JSON files in `data/` for users and movies.

## Project Structure
```
windsurf-project/
├─ server.js                  # Express app entry
├─ src/
│  ├─ middleware/requireAuth.js
│  ├─ routes/
│  │  ├─ auth.js              # Register/Login
│  │  └─ movies.js            # Movies CRUD + uploads
│  └─ utils/fsStore.js        # JSON file store helpers
├─ public/
│  ├─ uploads/                # Uploaded posters
│  ├─ index.html              # Landing (legacy)
│  ├─ movies.html             # Legacy movies page
│  └─ styles.css              # Legacy styles
├─ client/                    # React app (Vite)
│  ├─ index.html
│  └─ src/
│     ├─ main.jsx
│     ├─ App.jsx
│     ├─ styles.css
│     ├─ components/
│     │  └─ MovieForm.jsx
│     └─ pages/
│        ├─ MoviesList.jsx
│        ├─ Movies.jsx        # Combined screen (create/edit + list)
│        ├─ MovieCreate.jsx   # Optional dedicated create page
│        └─ MovieEdit.jsx     # Optional dedicated edit page
├─ data/
│  ├─ users.json              # Seeded users
│  └─ movies.json             # Seeded movies
├─ scripts/
│  └─ seed.js                 # Seed helper (if used)
├─ package.json               # Root scripts (server + proxy)
└─ client/package.json        # Frontend scripts
```

## Getting Started

### Prerequisites
- Node.js 18+

### Step-by-step: Install and run (development)
1. Install server dependencies
   ```bash
   # from project root
   npm install
   ```
2. Install client dependencies
   ```bash
   cd client && npm install
   ```
3. Start both server and client concurrently
   ```bash
   # run from project root
   npm run dev
   ```
   - Server: http://localhost:4000
   - Client (Vite): http://localhost:5173
   - API base: requests from the client to `/api/*` are proxied to the server

Alternative: run separately in two terminals
```bash
# terminal 1 (server at :4000)
npm run server

# terminal 2 (client at :5173)
cd client && npm run dev
```

### Environment Variables
Create a `.env` in project root (optional):
```
PORT=4000
JWT_SECRET=change-me
NODE_ENV=development
```

## API Overview
Base URL: `/api`

- `POST /api/auth/register` → `{ email, password }` → `201 { id, email }`
- `POST /api/auth/login` → `{ email, password }` → `200 { token, user }`

- `GET /api/movies?page=1&pageSize=10` → List with pagination
- `GET /api/movies/:id` → Get a movie
- `POST /api/movies` (auth, multipart) → Create
  - fields: `title` (string), `publishingYear` (int), `poster` (file optional)
- `PUT /api/movies/:id` (auth, multipart) → Update (replaces poster if provided)
- `DELETE /api/movies/:id` (auth) → Delete (removes poster file if exists)

Auth: Send `Authorization: Bearer <token>` for protected routes.

## Frontend Routes
- `/` or `/movies` → List movies
- `/movies/new` → Create movie (if using dedicated page)
- `/movies/:id/edit` → Edit movie (if using dedicated page)
- `/login` → Login

A combined page exists at `client/src/pages/Movies.jsx` that includes create/edit and list in one screen; `MovieForm.jsx` contains a reusable form with drag-and-drop.

## UI Notes
- Mobile-first layout with large heading, outlined cancel button, and centered upload area.
- Dropzone supports drag-and-drop; on mobile, tap “Upload”.
- Image previews appear after selecting a file; you can Change/Remove before submitting.

## Data Storage
- Users and movies are persisted in JSON files under `data/`.
- Uploaded posters are stored in `public/uploads/` and referenced by relative URL.

## Seeding (optional)
If a seed script exists, run:
```bash
node scripts/seed.js
```

## Build & Production
```bash
# build frontend
cd client && npm run build
# serve built frontend from Express in production (NODE_ENV=production)
NODE_ENV=production node ../server.js
```
When `NODE_ENV=production`, Express serves `client/dist` and keeps `/api/*` and `/uploads/*` working.
You can also use the root script which sets NODE_ENV=production automatically:
```bash
npm start
```

## Clean before sharing
- **Remove dependencies** to shrink the archive size (they can be reinstalled):
  ```bash
  # from project root
  rm -rf node_modules
  rm -rf client/node_modules
  ```
- **Ensure Git ignores dependencies** by adding to `.gitignore`:
  ```
  node_modules/
  client/node_modules/
  ```

## Sharing
- Push this repo to a public Git provider and share the link.
- Or zip the repository excluding `node_modules/` and share the archive.

## License
MIT
