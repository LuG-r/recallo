# Database Schema
**1. Users**
```sql
CREATE TABLE users(
    id SERIAL PRIMARY KEY,
    username TEXT NOT NULL UNIQUE,
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
id               INTEGER PRIMARY KEY
collection_id    INTEGER REFERENCES collections(id)
front            TEXT
back             TEXT
created_at       TIMESTAMP
```

**4. Reviews**
```sql
id                INTEGER PRIMARY KEY
user_id           INTEGER REFERENCES users(id)
flashcard_id      INTEGER REFERENCES flashcards(id)
reviewed_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP

rating            INTEGER    -- 0 = Again, 1 = Hard, 2 = Good, 3 = Easy

interval          INTEGER    -- in days
ease_factor       REAL
repetitions       INTEGER

elapsed_days      INTEGER    -- since last review
scheduled_days    INTEGER    -- scheduled interval
```