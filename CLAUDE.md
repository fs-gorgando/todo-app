# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a simple todo list web application that started as a single-page HTML application with localStorage persistence and is currently being migrated to use Supabase for backend storage and authentication.

## Technology Stack

- **Frontend**: Vanilla JavaScript (no frameworks or build tools)
- **Styling**: Embedded CSS3 with flexbox, gradients, and transitions
- **Backend**: Supabase (PostgreSQL with Row Level Security)
- **Authentication**: Supabase Auth
- **Deployment**: Vercel (connected to GitHub)
- **Repository**: https://github.com/fs-gorgando/todo-app.git

## Current State

The application is in transition:
- Original version used localStorage for client-side persistence
- Database schema created in `supabase-schema.sql` but not yet integrated into `index.html`
- Authentication UI and Supabase integration are pending implementation

## Supabase Configuration

**Project Details:**
- Project ID: `thpzjlxlhvynbnmyactc`
- Project URL: `https://thpzjlxlhvynbnmyactc.supabase.co`
- Database table: `todos`
- Authentication: Email-based auth with Row Level Security

**Database Schema:**
The `todos` table includes:
- `id` (UUID, primary key)
- `user_id` (UUID, references auth.users)
- `text` (TEXT, the todo content)
- `completed` (BOOLEAN)
- `created_at` (TIMESTAMP)

RLS policies ensure users can only access their own todos.

## Development Workflow

**Making Changes:**
1. Edit `index.html` directly (all code is in one file)
2. Test locally by opening in browser or serving via Python: `python -m http.server 8000`
3. Commit changes: `git add . && git commit -m "Description"`
4. Push to GitHub: `git push origin main`
5. Vercel auto-deploys from GitHub

**Deploying to Vercel:**
- Use Vercel CLI: `vercel --name todo-app --yes`
- Or connect via Vercel dashboard at https://vercel.com
- Must be authenticated: `vercel login` if needed

## Architecture Notes

**Single File Application:**
All code (HTML, CSS, JavaScript) is in `index.html`. This design choice prioritizes simplicity and easy deployment over code organization.

**Security:**
- XSS protection via DOM-based HTML escaping in `escapeHtml()` function
- Supabase RLS policies enforce user-level data isolation
- Anon key is safe for client-side use (public API key)

**Data Flow (Planned):**
1. User signs in via Supabase Auth
2. Authenticated user ID links to their todos via `user_id` column
3. All CRUD operations go through Supabase client
4. RLS policies enforce data access rules at database level

## Important Considerations

- The application stores Supabase credentials directly in the HTML (this is acceptable for the anon key)
- No build process required - deploy the single HTML file as-is
- Authentication state should persist across page refreshes
- Consider adding loading states when implementing Supabase integration
