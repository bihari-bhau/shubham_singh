# Bhaucastic Online Gaming Website: Specification Package

This repository holds the specification package for building the **Bhaucastic Online Gaming Website**, a multi category online gaming hub where players browse a library of games, play several of them in the browser, earn virtual coins, climb a leaderboard, and enter tournaments. The platform uses virtual coins only and never handles real money.

This package is the source set a developer builds against. The application source (the `client/` and `server/` folders) is added during implementation against the requirements below.

## What is in here

```
shubham_singh/
├── instruction.md        Source of truth requirements (read first)
├── prd.pdf               Product requirements document (PDF)
├── prd.docx              Product requirements document (Word, same content)
├── overview.txt          Client, audience, brand, and delivery scope
├── scope_of_work.txt     In scope, out of scope, and assumptions
├── reference_image.png   Landing page wireframe mockup
├── README.md             This file
├── tests/
│   ├── rubric.yaml       Acceptance checks, machine readable
│   └── rubric.docx       Acceptance checks, numbered pass or fail list
└── assets/
    ├── brand/            Wordmark and square mark
    ├── icons/            Five category icons
    ├── covers/           Twenty game cover placeholders
    ├── schema_diagram.png   Data model with foreign key arrows
    └── brand_intro.mp4   Eight second brand animation
```

## File guide

| File | Purpose | Read order |
|------|---------|------------|
| `instruction.md` | The technical source of truth. Every requirement, value, and constraint lives here. | 1 |
| `prd.pdf` / `prd.docx` | The same requirement set expanded into a readable document, with the brand reference and asset specs. | 2 |
| `overview.txt` | Client context, target audience, brand direction, and delivery scope. | 3 |
| `scope_of_work.txt` | What is in scope, what is out of scope, and the assumptions. | 4 |
| `tests/rubric.yaml`, `tests/rubric.docx` | The acceptance rubric. The delivered build is graded against these items. | 5 |
| `reference_image.png` | A visual reference for the landing page layout. | as needed |
| `assets/` | Brand assets: logo, mark, category icons, game covers, schema diagram, and brand animation. | as needed |

## The product in brief

- Stack: React with Vite on the front end, Node with Express on the back end, MongoDB or SQLite selected through the `DB_DRIVER` environment variable.
- Authentication: JSON Web Tokens stored in an HTTP only cookie, with bcrypt password hashing.
- Categories: casino, arcade, casual, esports, and racing.
- Playable games: Spin Wheel, Brick Breaker, Memory Match, and Kart Sprint run in the browser.
- Economy: virtual coins only. New accounts start with 500 coins. Coins are earned from games and spent on tournament entry.
- Features: account sign up and sign in, a game library with category filters and search, a virtual coin wallet, a leaderboard, and tournaments.

The full data model and every numeric value are defined in `instruction.md` and mirrored in the PRD.

## Brand

The visual direction is aggressive esports. The accent color is crimson red at hex E10600 on a jet black base at hex 0A0A0A. The wide wordmark sits in the site header and the square mark serves the browser tab and avatars. See `prd.docx` section 23 for the color palette and `assets/brand/` for the source files.

## Running the site

The application source is produced during implementation. Once the `client/` and `server/` folders are in place, the build exposes these commands through npm scripts, as required in `instruction.md`:

```
npm install     # install dependencies
npm run seed    # load games, sample scores, and an open tournament
npm run build   # build the front end
npm start       # start the server
```

The seed script creates a demo user. Its login credentials are listed in this README once the seed script is implemented.

## Acceptance

The build is accepted when every item in `tests/rubric.yaml` can be checked as true against the running site or the repository. Each rubric item names its source section in the instruction file through the `note` field.
