# HiringKit: screening questionnaire MVP (Django + SQLite)

Candidates fill a public form; you review, score, filter and export in a staff dashboard.
Questions live in the database, so every company and job can have its own question set.

## Run it
    pip install -r requirements.txt
    python manage.py migrate
    python manage.py createsuperuser
    python manage.py seed_legal        # loads the Legal Associate questionnaire (optional)
    python manage.py runserver

- Candidate form: http://127.0.0.1:8000/apply/<job-slug>/   (slug is printed by seed_legal and shown in /admin/ > Jobs)
- Dashboard (staff login): http://127.0.0.1:8000/dashboard/
- Setup (companies, jobs, questions): http://127.0.0.1:8000/admin/

## How scoring works
- Each question has a **weight** (max points; 0 = not scored).
- Choice questions: write options one per line, with points after a pipe, e.g. `Yes|10`. The score is scaled to the weight.
- Written answers with a weight are marked by you (0 to weight) on the candidate page.
- Candidate score = points earned / total weighted points, as a percent.

## Use it for another company
In /admin/: add a Company, add a Job, add Questions inline. Share the job's /apply/ link.

## Before going live
- Set `DEBUG = False`, a new `SECRET_KEY`, and `ALLOWED_HOSTS` in hiring/settings.py; serve behind HTTPS (nginx/gunicorn).
- Resumes are saved in `media/` and only downloadable by logged-in staff. Back up `db.sqlite3` and `media/`.
- Add a privacy notice and a retention policy for candidate data.
- Move to PostgreSQL when several companies use one install.

## Not in the MVP (good next steps)
Per-company logins (right now every staff user sees every company), email notifications, interview scheduling, trial-assignment tracking, rate/availability comparison view.
