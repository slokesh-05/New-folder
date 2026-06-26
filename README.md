# NIFTY Options Signal Platform

Production-oriented FastAPI trading signal platform with:

- JWT authentication with bcrypt hashing
- User registration, login, logout, forgot/reset password
- Protected dashboard and admin routes
- Telegram subscriber notifications with delivery logging and retries
- Browser notifications with WebSocket real-time signal delivery
- Dashboard analytics and admin controls
- SQLAlchemy models targeting PostgreSQL

## Project Structure

```text
app/
├── admin/
├── auth/
├── dashboard/
├── notifications/
├── websocket/
├── signals/
├── templates/
├── static/
├── database/
├── models/
├── services/
├── data/
├── config.py
└── main.py
```

## Environment Variables

Create a `.env` file:

```env
SECRET_KEY=replace-with-a-long-random-secret
DATABASE_URL=postgresql+psycopg://postgres:postgres@localhost:5432/nifty_signals
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
FRONTEND_BASE_URL=http://localhost:8000
DEBUG=true
ACCESS_TOKEN_EXPIRE_MINUTES=30
REMEMBER_ME_EXPIRE_DAYS=30
SIGNAL_POLL_SECONDS=300
```

## Database Tables

- `users`
- `password_reset_tokens`
- `signals`
- `telegram_subscribers`
- `notification_logs`

## Local Setup

1. Create and activate a virtual environment.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Start PostgreSQL and create database:

```sql
CREATE DATABASE nifty_signals;
```

4. Run the app:

```bash
uvicorn app.main:app --reload
```

5. Open:

- `http://localhost:8000/register`
- `http://localhost:8000/login`
- `http://localhost:8000/dashboard`
- `http://localhost:8000/admin`

## Notification Workflow

When a new signal is generated:

1. The signal engine polls market data.
2. A new signal is saved in `signals`.
3. Telegram alerts are sent to active subscribers.
4. WebSocket notifications are broadcast to connected browsers.
5. Notification logs are stored in `notification_logs`.
6. Dashboard metrics refresh from database state.

## Deployment Notes

- Use Gunicorn/Uvicorn workers behind Nginx for production.
- Set `DEBUG=false`.
- Replace the default `SECRET_KEY`.
- Use PostgreSQL managed backups and SSL connections.
- Add SMTP integration or transactional email provider for production password reset emails.
- Serve `/static` behind a CDN or reverse proxy cache.
- Run the signal polling loop in a dedicated worker if you scale beyond one web instance.

## Suggested Production Commands

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

With systemd or container orchestration, run a separate dedicated signal worker if needed.

