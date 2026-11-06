# PostgreSQL Lab (Docker)

A reproducible local PostgreSQL environment for practicing SQL, PL/pgSQL, roles, and privileges — no local PostgreSQL install required.

- **Image:** `postgres:17`
- **Container:** `postgres-lab`
- **Database:** `practice`
- **Superuser:** `postgres`
- **Volume:** `postgres-data` (persistent)
- **Port binding:** `127.0.0.1:5432:5432` (host localhost only — not exposed publicly)

---

## 1. Requirements

- Docker installed and running:

```bash
docker --version
```

---

## 2. Create the container

Run from your **host shell** (VM or local machine):
Dont forget to create the volume for the container

```bash
docker run -d \
  --name postgres-lab \
  -e POSTGRES_PASSWORD='YOUR_STRONG_PASSWORD' \
  -e POSTGRES_DB=practice \
  -p 127.0.0.1:5432:5432 \
  -v postgres-data:/var/lib/postgresql/data \
  postgres:17
```

Verify it started:

```bash
docker ps
docker logs postgres-lab
```

### What each flag does

| Flag                                        | Purpose                                             |
|---------------------------------------------|-----------------------------------------------------|
| `-d`                                        | Run in background                                   |
| `--name postgres-lab`                       | choose a name for  container                        |
| `-e POSTGRES_PASSWORD=...`                  | Superuser password **on first initialization only** |
| `-e POSTGRES_DB=practice`                   | Creates the `practice` database on first init       |
| `-p 127.0.0.1:5432:5432`                    | Publish port 5432 on **host localhost only**        |
| `-v postgres-data:/var/lib/postgresql/data` | Named volume for data persistence                   |
| `postgres:17`                               | Install version 17  instead of `latest`             |


---

## 3. Access options

### 3.1 Directly into `psql` (from host shell)

```bash
docker exec -it postgres-lab psql -U postgres -d practice
```

You should see the prompt:

```
practice=#
```

Try:

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    age INTEGER
);

INSERT INTO students (name, age) VALUES ('Mohamed', 23);

SELECT * FROM students;

\q
```

### 3.2 Into the container's Linux shell

```bash
docker exec -it postgres-lab bash
```

You'll get:

```
root@<container-id>:/#
```

From there you can:

```bash
psql -U postgres -d practice    # launch psql inside
```

### 3.3 From a GUI (optional)

| Field    | Value               |
|----------|---------------------|
| Host     | `127.0.0.1`         |
| Port     | `5432`              |
| Database | `practice`          |
| User     | `postgres`          |
| Password | *(the one you set)* |


---

## 4. Daily commands

| Task                    | Command                                                     |
|-------------------------|-------------------------------------------------------------|
| List running containers | `docker ps`                                                 |
| Stop the database       | `docker stop postgres-lab`                                  |
| Start it again          | `docker start postgres-lab`                                 |
| Open `psql`             | `docker exec -it postgres-lab psql -U postgres -d practice` |
| Open container shell    | `docker exec -it postgres-lab bash`                         |
| View logs               | `docker logs postgres-lab`                                  |
| Follow logs             | `docker logs -f postgres-lab`                               |
| Inspect the volume      | `docker volume inspect postgres-data`                       |

---

## 5. Persistence behavior

Because of the named volume `postgres-data`:

| Action                           | Data preserved?                       |
|----------------------------------|---------------------------------------|
| `docker stop postgres-lab`       | ✅ Yes                                |
| `docker start postgres-lab`      | ✅ Yes                                |
| `docker rm postgres-lab`         | ✅ Yes (volume survives)              |
| `docker volume rm postgres-data` | ❌ No — **this deletes the database** |

> 🔥 Only run `docker volume rm postgres-data` when you intentionally want a clean slate.

---

## 6. Recreating the container (with new credentials)

If you want to change `POSTGRES_PASSWORD` / `POSTGRES_DB` / `POSTGRES_USER`, you must also drop the volume:

```bash
docker rm -f postgres-lab
docker volume rm postgres-data   # WARNING: destroys all data

docker run -d \
  --name postgres-lab \
  -e POSTGRES_PASSWORD='NEW_PASSWORD' \
  -e POSTGRES_DB=practice \
  -p 127.0.0.1:5432:5432 \
  -v postgres-data:/var/lib/postgresql/data \
  postgres:17
```

If you only want to **change the password without wiping data**, use:

```sql
ALTER USER postgres WITH PASSWORD 'new_password';
```

---

## 7. Cleanup

```bash
# stop and remove the container (data stays in the volume)
docker stop postgres-lab
docker rm postgres-lab

# remove the data volume too (DESTROYS ALL DATA)
docker volume rm postgres-data
```

Or in one shot:

```bash
docker rm -f postgres-lab && docker volume rm postgres-data
```

