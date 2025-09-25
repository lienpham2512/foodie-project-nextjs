# Foodie — Next.js Meals Demo

A small Next.js app that showcases a meals directory with images, summaries, and full instructions. It uses a local SQLite database (`meals.db`) and includes a simple seeding script to populate example meals.

## What this project contains

- Next.js 14 app using the App Router convention (files under `app/`).
- A small SQLite database accessed with `better-sqlite3` (`meals.db`).
- Utility library code in `lib/` for reading and writing meals.
- Seed script `initdb.js` to create the `meals` table and insert example meals.
- Public images in `public/images/` and example assets in `assets/`.

## Highlights / features

- List and view individual meal pages (by slug).
- Create/save new meals from a form (server-side image file handling in `lib/meals.js`).
- Input sanitization for instructions using the `xss` package and slug generation via `slugify`.

## Prerequisites

- Node.js (recommend Node 18+). 
- npm (or yarn/pnpm) for installing dependencies.
- On Windows, installing `better-sqlite3` may require build tools (Visual Studio Build Tools). If you hit install errors for native modules, see the package's docs or install the platform build tools.

## Install

Open a terminal in the project folder and run:

```bash
npm install
```

This will install the dependencies listed in `package.json` (Next.js, React, better-sqlite3, slugify, xss, etc.).

## Initialize the database (seed sample data)

The project ships with an `initdb.js` script that will create the `meals` table (if missing) and insert sample meals into `meals.db`.

Run it once before starting the app (or when you want to reset the seed data):

```bash
node initdb.js
```

After running that, a file named `meals.db` will be created/updated in the project root and populated with sample rows.

## Run the app (development)

Start the Next.js dev server:

```bash
npm run dev
```

Open http://localhost:3000 in your browser to view the app.

Available npm scripts (from `package.json`):

- `npm run dev` — start dev server
- `npm run build` — build for production
- `npm run start` — start built app
- `npm run lint` — run ESLint

## How the data layer works

- `lib/meals.js` contains helper functions used by the app:
  - `getMeals()` — returns all meals (note: the function includes an artificial 2s delay to simulate loading).
  - `getMealbySlug(slug)` — returns a single meal row by slug.
  - `saveMeal(meal)` — saves a new meal to the DB. It:
    - Generates a URL-friendly slug using `slugify`.
    - Sanitizes `instructions` using `xss`.
    - Stores an uploaded image to `public/images/` and updates the `image` path saved in the DB.

If you need to change where images are stored or how filenames are generated, update `saveMeal` in `lib/meals.js`.

## Project structure (important files)

- `app/` — Next.js app pages and layout. Look here for routing and UI components.
- `components/` — React components used across the app (header, meal grid, form helpers, etc.).
- `lib/` — server-side helper functions that interact with `meals.db`.
- `initdb.js` — seed script for the DB.
- `meals.db` — the SQLite database file (created by `initdb.js`).
- `public/images/` — directory where meal images are served from.

## Adding or updating seed data

Edit `initdb.js` to add, remove, or modify the `dummyMeals` array. Re-run `node initdb.js` to recreate/append entries. Note: the `slug` field is UNIQUE in the DB schema.

## Troubleshooting

- If `better-sqlite3` fails to install on Windows, ensure you have Visual Studio Build Tools and a compatible Python runtime installed, or consult the `better-sqlite3` GitHub page for prebuilt binaries and platform-specific steps.
- If the app cannot read `meals.db`, confirm `initdb.js` was run and that the file exists in the project root and the process has permission to read it.
- If uploaded images are not showing, check that `public/images/` contains the files and that the URL paths saved in the DB (e.g. `/images/<slug>.<ext>`) match those files.

## Contributing

Small, focused changes are welcome. Typical workflow:

1. Fork/branch.
2. Run `npm install` and `node initdb.js` to get the DB seeded.
3. Make changes and ensure the dev server runs: `npm run dev`.
4. Add tests or manual verification steps for UI changes.

## License

This project template doesn't include a license file. Add one if you plan to publish or share the repository.


