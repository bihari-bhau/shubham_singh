# shubham_singh: Bhaucastic Online Gaming Website

**Product Requirements Document**

This document is the source of truth for the build. Every requirement here is meant to be checked against the running site and the repository. Numbers inside sections are stable so that test items can point to them.

## 1. Project Summary

- Bhaucastic is a multi category online gaming hub delivered as a full stack web application.
- Visitors browse a library of games, play four of them in the browser, earn virtual coins, climb a leaderboard, and join tournaments.
- The site handles virtual coins only and never processes real money.

## 2. Goals and Success Criteria

- A first time visitor can create an account and reach the game library in under one minute on a normal connection.
- A signed in player can open a playable game, finish a round, and see coins added to the wallet balance.
- The leaderboard updates after a score is submitted and shows the player in the correct rank order.
- A player can join an open tournament when the wallet balance is at least the entry cost.
- The build passes the test items listed in tests/rubric.yaml.

## 3. Target Audience

- The primary audience is players between the ages of 16 and 34 who follow esports and play casual titles.
- The secondary audience is casual visitors who try one game before deciding to register.
- The interface uses a dark theme to stay readable during long sessions.

## 4. Brand and Visual Direction

- The visual direction is aggressive esports.
- The primary accent color is crimson red at hex E10600.
- The base background is jet black at hex 0A0A0A.
- Surfaces and body text use supporting greys between hex 141414 and hex CFCFCF.
- Headings use a bold condensed sans family. Body copy uses a sans serif family sized at 16px or larger.
- The wide wordmark appears in the site header. The square mark appears in the browser tab and on avatars.

## 5. Technology Stack

- The front end uses React built with Vite.
- The back end uses Node with the Express framework.
- The database is either MongoDB or SQLite, selected through an environment variable named DB_DRIVER.
- Authentication uses JSON Web Tokens stored in an HTTP only cookie.
- Styling uses plain CSS or a single utility framework, kept consistent across pages.
- The repository uses npm scripts for install, seed, build, and start.

## 6. Repository and Folder Structure

- The repository root holds overview.txt, scope_of_work.txt, instruction.md, prd.pdf, prd.docx, and reference_image.png.
- The repository root holds an assets folder.
- The assets folder holds brand, icons, and covers subfolders plus schema_diagram.png and brand_intro.mp4.
- Source code lives under a client folder for the front end and a server folder for the back end.
- A README file at the root explains install, seed, and run steps.
- A tests folder holds rubric.yaml and rubric.docx with the test items.

## 7. Data Model and Schema

- The schema defines five tables named users, games, scores, wallet_transactions, and tournaments.
- The users table holds id, username, email, password_hash, coin_balance, and created_at.
- The games table holds id, title, category, slug, cover_path, and is_playable.
- The scores table holds id, user_id, game_id, points, and created_at.
- The wallet_transactions table holds id, user_id, amount, reason, and created_at.
- The tournaments table holds id, name, game_id, entry_cost, prize_pool, start_at, end_at, and status.
- The scores table user_id references users id, and game_id references games id.
- The wallet_transactions table user_id references users id.
- The tournaments table game_id references games id.
- The category field on games is one of casino, arcade, casual, esports, or racing.

## 8. Authentication and Sessions

- Passwords are hashed with bcrypt before storage and never stored as plain text.
- Sign in returns a JSON Web Token set in an HTTP only cookie.
- The cookie carries the Secure and SameSite attributes.
- Protected API routes reject requests that lack a valid token with status 401.
- Sign out clears the session cookie.

## 9. User Accounts and Profiles

- Sign up requires a username, an email, and a password.
- The username and email are each unique across the users table.
- A new account receives a starting balance of 500 virtual coins.
- A profile page shows the username, the coin balance, and the player best score.

## 10. Game Library and Categories

- The library lists at least 20 games across the five categories.
- Each category holds at least three games.
- The library shows category filter controls for all five categories.
- The library shows a search field that filters games by title text.
- Each game card shows the title, the category, and the cover image.
- Games marked playable show a play control that opens the game.

## 11. Playable Games

- Four games run inside the site: Spin Wheel, Brick Breaker, Memory Match, and Kart Sprint.
- Spin Wheel grants a coin reward based on the segment the wheel lands on.
- Brick Breaker awards points for each brick cleared and ends when the ball is lost.
- Memory Match awards points for matched pairs and ends when all pairs are found.
- Kart Sprint awards points based on laps completed before the timer ends.
- Each playable game submits a final score to the scores table for the signed in player.
- Each playable game converts its score into a coin reward credited to the wallet.

## 12. Wallet and Virtual Coins

- The wallet shows the current coin balance for the signed in player.
- Every balance change writes a row to wallet_transactions with a positive or negative amount and a reason.
- Coins are granted on sign up, earned from games, and spent on tournament entry.
- The wallet never exposes a way to add or withdraw real money.
- A balance can never drop below zero.

## 13. Leaderboard

- The leaderboard ranks players by best score within a selected game.
- The leaderboard offers a view that ranks players by total coin balance.
- Each leaderboard row shows rank, username, and the ranking value.
- The leaderboard reflects a new score within one refresh of the page.

## 14. Tournaments

- A tournament has a name, a linked game, an entry cost, a prize pool, a start time, an end time, and a status.
- The status is one of upcoming, open, or closed.
- A player joins an open tournament only when the balance is at least the entry cost.
- Joining a tournament subtracts the entry cost and writes a wallet transaction.
- When a tournament closes, players are ranked by their best score in the linked game.
- The seed data includes at least one tournament with status open.

## 15. Seed Data Requirements

- A seed script fills the games table with at least 20 games across the five categories.
- The seed script marks Spin Wheel, Brick Breaker, Memory Match, and Kart Sprint as playable.
- The seed script creates at least one demo user with a known username and password.
- The seed script writes sample scores so the leaderboard is not empty on first run.
- The seed script creates at least one open tournament.
- Each seeded game points to a cover image inside the assets covers folder.

## 16. API Endpoints

- The API exposes POST /api/auth/signup, POST /api/auth/signin, and POST /api/auth/signout.
- The API exposes GET /api/games with optional category and search query parameters.
- The API exposes GET /api/games/:slug for a single game.
- The API exposes GET /api/wallet and POST /api/wallet/earn for the signed in player.
- The API exposes GET /api/leaderboard with a query parameter for game or coins.
- The API exposes GET /api/tournaments and POST /api/tournaments/:id/join.
- The API returns JSON for every endpoint and uses standard status codes.

## 17. Frontend Pages and Routing

- The site has a landing page, a library page, a game play page, a wallet page, a leaderboard page, a tournaments page, and a profile page.
- The landing page shows the wordmark, a short pitch, and a call to action that leads to sign up.
- Routing uses lowercase path segments such as /library, /play/:slug, /wallet, /leaderboard, /tournaments, and /profile.
- The header shows the coin balance for a signed in player.
- Pages that need a session redirect a signed out visitor to the sign in page.

## 18. Responsive Layout and Accessibility

- The layout adapts to phone, tablet, and desktop widths without horizontal scrolling.
- Text holds a contrast ratio of at least 4.5 to 1 against its background.
- Interactive controls are reachable and operable with the keyboard.
- Images carry alt text that names the game or the brand element.

## 19. Security Requirements

- User input is validated on the server before it reaches the database.
- Database access uses parameterized queries or an object data mapper to block injection.
- Secrets such as the token signing key load from environment variables, not from source code.
- The server sets common security headers on responses.
- The site contains no flow that handles real money or stores payment details.

## 20. Testing and Acceptance

- Every test item in tests/rubric.yaml maps to a numbered line in this document.
- The build is accepted when each rubric item can be checked as true against the running site or the repository.
- The seed script runs without errors on a fresh database.
- The site starts with one documented command after install and seed.

## 21. Delivery and Handoff

- The full project ships inside a folder named shubham_singh.
- The folder is pushed to a public GitHub repository with all files included.
- The README lists the demo user credentials created by the seed script.
- The repository URL is the single item submitted for review.

## 22. Deliverables

- A client folder containing the React front end built with Vite, delivered as source.
- A server folder containing the Node and Express back end, delivered as source.
- One database schema covering the five tables in section 7, delivered as migration or model files.
- A seed script that loads the data described in section 15, runnable through one npm script.
- A README file in markdown that lists the install, seed, build, and start commands and the demo user credentials.
- Four playable browser games named in section 11, delivered inside the client folder.
- The brand assets in the assets folder at the dimensions stated in section 24.

## 23. Scope Boundaries

- Real money handling is out of scope, including deposits, withdrawals, and payment processors.
- Native mobile applications for iOS or Android are out of scope.
- Live multiplayer netcode and real time voice chat are out of scope.
- Video streaming and broadcast features are out of scope.
- Email delivery for password reset is out of scope in this build and is deferred to a later phase.
- Server hosting fees, domain registration, and maintenance after handoff are out of scope.
- Content moderation tooling beyond server side input validation is out of scope.

## 24. Brand Asset Specifications

- bhaucastic_logo.png is the wide wordmark sized at 1372 by 360 pixels.
- bhaucastic_mark.png is the square mark sized at 512 by 512 pixels.
- Each category icon is sized at 512 by 512 pixels.
- Each game cover is sized at 800 by 500 pixels.
- schema_diagram.png is sized at 1400 by 1000 pixels.
- brand_intro.mp4 is sized at 1280 by 720 pixels, runs for 8 seconds, and plays at 30 fps.
- reference_image.png is the landing page wireframe sized at 1440 by 2110 pixels.
- brand_intro.mp4 shows the mark, the wordmark, the five category names, and the closing line Virtual coins. Real competition.
