# Echoes

> **Anonymous messages that drift through time.**

A public anonymous message board reimagined as a digital time capsule. Leave a thought for anyone or anything, then watch it become part of a shared archive of memories, confessions, hopes, and gratitude.

**Status:** Completed
**Platform:** Web
**Built with:** React · Vite · Supabase · Framer Motion

**[🌌 Live Site](https://echoes-sandy.vercel.app)**

---

## The Idea

What if an anonymous message board wasn't a feed?

No follower counts.
No likes.
No notifications.
No algorithm deciding what deserves attention.

Just people leaving small pieces of themselves behind.

**Echoes** turns anonymous messages into a shared digital time capsule. Messages are grouped by the month they were written and represented as constellations floating through a starfield.

Instead of scrolling through an endless feed, you explore moments in time.

---

## The Experience

```text id="9y5i5g"
                  Leave a Message
                         │
                         ▼
                ┌─────────────────┐
                │      Echo       │
                │                 │
                │  From → To      │
                │  Your message   │
                └────────┬────────┘
                         │
                         ▼
                  Added to Archive
                         │
                         ▼
                 Grouped by Month
                         │
                         ▼
                  ✦ Constellation ✦
                         │
                         ▼
                  Explore the Past
```

Each month becomes a constellation that can be explored independently.

Select one and the interface transitions into that point in the archive, turning browsing into something closer to exploration than scrolling.

---

## Core Features

### 🌌 Constellation Navigation

Messages are organised into monthly constellations with different visual forms:

* Orion
* Dipper
* Cross
* Triangle
* Ring
* Arrow
* Scatter

Each constellation becomes a visual representation of a particular month.

### 📅 Time-Based Archive

The archive can be explored year by year as it grows.

Mobile users can navigate between years with swipe interactions.

### ✨ Ambient Interaction

The interface uses motion and particles as part of the experience rather than as decoration.

Desktop users get a custom ember-like cursor with a trailing stardust effect, while mobile devices receive touch-based equivalents of the interactive effects.

### 📱 Mobile-First Interaction

Echoes isn't simply a desktop interface compressed onto a phone.

Touch-specific interactions were designed to preserve the atmosphere and interaction model across different devices.

### 🔒 Anonymous by Design

There are no:

* Accounts
* Logins
* Follower systems
* Likes
* Public identity profiles
* Engagement rankings

The focus stays on the message rather than the person who posted it.

---

## Safety & Moderation

An anonymous public platform needs safeguards.

Echoes uses several layers of abuse prevention:

```text id="5jcx0e"
                 User Submission
                        │
                        ▼
              ┌──────────────────┐
              │ Rate Limiting     │
              └────────┬─────────┘
                       ▼
              ┌──────────────────┐
              │ Content Filters  │
              └────────┬─────────┘
                       ▼
              ┌──────────────────┐
              │ Database Rules   │
              └────────┬─────────┘
                       ▼
                  Stored Echo
                       │
                       ▼
                 Community Report
                       │
                       ▼
                Admin Moderation
```

The system includes:

* Per-device rate limiting
* Global rate limiting
* Submission pause controls
* Bulk-managed banned phrases
* Community reporting
* Visible report counts
* Administrative moderation tools

An important design decision was moving critical validation and protection into the database layer rather than relying entirely on client-side checks.

---

## Admin System

Although the public experience is intentionally minimal, the application includes a full administrative interface behind the scenes.

The dashboard supports:

* Message moderation
* Bulk deletion
* CSV export
* Activity logging
* Monthly analytics
* Submission controls
* Emergency data wipe

This creates a deliberate contrast:

> **Simple public experience.
> Serious operational system underneath.**

---

## Data & Reliability

Echoes uses **Supabase PostgreSQL** as its persistent data layer.

The core database contains:

```text id="zkmv79"
messages
├── sender
├── recipient
├── content
├── timestamp
└── report count

site_settings
├── submission state
└── banned phrases
```

Row Level Security policies and database-level validation help enforce important application rules independently of the client.

The project also includes automated weekly database backups through GitHub Actions.

---

## Architecture

```text id="7y2jsk"
                     Echoes
                        │
          ┌─────────────┴─────────────┐
          │                           │
       React UI                   Admin UI
          │                           │
          └─────────────┬─────────────┘
                        │
                        ▼
                     Supabase
                        │
                  PostgreSQL
                        │
          ┌─────────────┴─────────────┐
          │                           │
      Messages                  Site Settings
          │
          ▼
   Reports / Moderation
```

Vercel handles the web deployment, while GitHub Actions handles scheduled database maintenance and backups.

---

## Tech Stack

| Technology         | Purpose                          |
| ------------------ | -------------------------------- |
| **React**          | Frontend application             |
| **Vite**           | Build tooling                    |
| **Tailwind CSS**   | Styling                          |
| **Framer Motion**  | Animation and interaction        |
| **Supabase**       | Backend and PostgreSQL database  |
| **Vercel**         | Hosting                          |
| **GitHub Actions** | Scheduled automation and backups |

---

## Engineering Highlights

### Designing around anonymity

Anonymity isn't treated as merely removing the login screen.

The product deliberately avoids identity-driven mechanics such as profiles, followers, likes, and engagement rankings.

The message becomes the primary unit of the experience.

### Database-level protection

Critical submission controls are enforced at the database level through Row Level Security policies and validation logic.

This provides an additional protection layer beyond client-side checks.

### Custom interaction model

The constellation system required more than displaying messages in a conventional list.

The project combines:

* Spatial positioning
* Particle effects
* Motion
* Transitions
* Touch interactions
* Custom cursor behaviour

to create a distinct browsing experience.

### Responsive interaction design

Desktop and mobile environments don't interact with the application in exactly the same way.

Echoes therefore adapts its interaction model rather than simply shrinking the desktop UI.

---

## Design Philosophy

Echoes was built around one question:

> **What would social media feel like if we removed the mechanics designed to keep us scrolling?**

The answer became a quieter kind of social space.

There is no feed demanding attention.

There is no number telling you how much attention your message received.

There is simply a message, a moment in time, and the possibility that someone will eventually discover it.

---

## What This Project Demonstrates

Echoes provided experience with:

* React application architecture
* Vite
* Supabase and PostgreSQL
* Row Level Security
* Database-level validation
* Anonymous public systems
* Abuse prevention
* Moderation workflows
* Framer Motion
* Particle-based interfaces
* Custom cursor interactions
* Touch interaction design
* Responsive web development
* GitHub Actions automation
* Automated database backups
* Designing software around a strong product concept

---

## Project Status

**Completed**

The public message board, time-based archive, constellation navigation, moderation system, administrative dashboard, responsive interactions, and automated backup workflow are implemented.

---

## Source Code

🔒 **Private repository**

The source code is intentionally kept private. This page documents the product, architecture, design philosophy, and engineering decisions without exposing the implementation.

---

## Author

**Arunan Kavirajan**

IT undergraduate at SRM Institute of Science and Technology, Chennai.

Building software, experimenting with AI, and turning ideas into working products.

[GitHub](https://github.com/Arunan-Kavirajan) · [LinkedIn](https://www.linkedin.com/in/arunan-kavirajan)

---

*Echoes — a place for thoughts to drift through time.*
