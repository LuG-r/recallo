# Database Schema
**1. Users**
```sql
CREATE TABLE users(
    id SERIAL PRIMARY KEY,
    username TEXT NOT NULL UNIQUE,
    email TEXT UNIQUE,
    password_hash TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**2. Collections**
A collection is a group of flashcards. Each user can create many collections
```sql
CREATE TABLE collections(
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    name TEXT NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**3. Flashcards**
Each flashcard belongs to a collection so there must be a default collection. (This should be configurable by the user)
```sql
CREATE TABLE flashcards(
    id SERIAL PRIMARY KEY,
    collection_id INTEGER REFERENCES collections(id) ON DELETE CASCADE,
    front TEXT NOT NULL,
    back TEXT NOT NULL,

    -- SM-2 Scheduling fields:
    ease_factor REAL DEFAULT 2.5,     -- Initial ease factor
    interval INTEGER DEFAULT 0,       -- Days until next review
    repetitions INTEGER DEFAULT 0,    -- Consecutive successful reviews
    due_date DATE,                    -- Next scheduled review date

    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**4. Reviews**
```sql
CREATE TABLE reviews(
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
    flashcard_id INTEGER REFERENCES flashcards(id) ON DELETE CASCADE,
    reviewed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    rating INTEGER NOT NULL CHECK (rating BETWEEN 0 AND 5),
    previous_intervale INTEGER,
    new_interval INTEGER,
    previous_ease_factor REAL,
    new_ease_factor REAL,
    repetitions       INTEGER
);
```