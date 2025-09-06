***<----------Everything about logging in python ------------>***

Perfect, let’s go **step by step into Python logging** — from basics to advanced, with intuition, examples, and best practices.

---

# 🔹 1. What is Logging?

Logging is the process of recording information about what your code is doing while it runs.
It helps in:

* Debugging
* Monitoring (e.g., in production)
* Auditing (who did what, when)
* Replacing `print()` in professional applications

Unlike `print()`, logging:

* Can be turned **on/off** without changing code
* Supports **different levels of severity**
* Can be written to **files, console, network, syslog, etc.**
* Can be **formatted** (timestamps, function name, etc.)

---

# 🔹 2. Logging Levels

Python’s logging module defines **5 standard levels** (from least to most severe):

| Level      | Numeric Value | Usage Example                             |
| ---------- | ------------- | ----------------------------------------- |
| `DEBUG`    | 10            | Detailed info, usually for debugging      |
| `INFO`     | 20            | General events (e.g., app started)        |
| `WARNING`  | 30            | Something unexpected, but app still works |
| `ERROR`    | 40            | A serious problem, app is affected        |
| `CRITICAL` | 50            | Very serious error, app may crash         |

👉 Default level is **WARNING** (so INFO/DEBUG won’t show unless configured).

---

# 🔹 3. Basic Usage

```python
import logging

logging.debug("This is a debug message")   # Won’t show (below WARNING by default)
logging.info("App started")               # Won’t show
logging.warning("This is a warning!")     # ✅ Shows
logging.error("An error occurred")        # ✅ Shows
logging.critical("Critical failure!")     # ✅ Shows
```

---

# 🔹 4. Configuring Logging

You can configure **level, format, and output**.

## Example: Console Logging

```python
import logging

logging.basicConfig(
    level=logging.DEBUG,   # Show everything from DEBUG upward
    format="%(asctime)s - %(levelname)s - %(message)s"
)

logging.debug("Debugging...")
logging.info("Information message")
logging.warning("This is a warning")
logging.error("Error occurred")
logging.critical("Critical issue")
```

Output:

```
2025-09-03 07:00:00,123 - DEBUG - Debugging...
2025-09-03 07:00:00,124 - INFO - Information message
2025-09-03 07:00:00,125 - WARNING - This is a warning
2025-09-03 07:00:00,126 - ERROR - Error occurred
2025-09-03 07:00:00,127 - CRITICAL - Critical issue
```

---

# 🔹 5. Logging to a File

```python
logging.basicConfig(
    filename="app.log",
    filemode="a",   # 'w' for overwrite, 'a' for append
    level=logging.INFO,
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s"
)

logging.info("This goes to app.log")
```

---

# 🔹 6. Logger Objects (Best Practice)

Instead of using the **root logger** (`logging.info()`), define your own:

```python
logger = logging.getLogger("my_app")

logger.setLevel(logging.DEBUG)  # Set min level

# Create handler (console)
console_handler = logging.StreamHandler()
console_handler.setLevel(logging.WARNING)

# Formatter
formatter = logging.Formatter("%(asctime)s - %(levelname)s - %(message)s")
console_handler.setFormatter(formatter)

# Add handler to logger
logger.addHandler(console_handler)

# Use logger
logger.debug("Debugging...")
logger.warning("Something might be wrong")
```

👉 This is preferred in real apps because:

* You can have **different loggers** for different modules
* You can assign different **handlers** (console, file, API, etc.)

---

# 🔹 7. Advanced Handlers

Python has many handlers:

* `StreamHandler` → console
* `FileHandler` → log file
* `RotatingFileHandler` → auto-rotate when file grows large
* `TimedRotatingFileHandler` → rotate based on time
* `SMTPHandler` → send logs via email
* `SysLogHandler` → system logs
* `HTTPHandler` → send logs to server

Example with rotating log file:

```python
from logging.handlers import RotatingFileHandler

handler = RotatingFileHandler("app.log", maxBytes=2000, backupCount=3)
formatter = logging.Formatter("%(asctime)s - %(levelname)s - %(message)s")
handler.setFormatter(formatter)

logger = logging.getLogger("my_app")
logger.setLevel(logging.DEBUG)
logger.addHandler(handler)

logger.info("This will be rotated when file > 2KB")
```

---

# 🔹 8. Logging Exceptions

```python
try:
    1 / 0
except Exception as e:
    logging.error("Error occurred", exc_info=True)
```

Output includes **traceback**.

---

# 🔹 9. Logging in Multiple Modules

`main.py`

```python
import logging
from module import run_task

logging.basicConfig(level=logging.DEBUG)
logging.debug("Starting main")
run_task()
```

`module.py`

```python
import logging
logger = logging.getLogger(__name__)

def run_task():
    logger.info("Task is running")
```

👉 Using `__name__` ensures logs are tagged by module.

---

# 🔹 10. Best Practices

✅ Use `logging` instead of `print()` in real projects
✅ Configure `basicConfig()` only once (usually in `main.py`)
✅ Use **loggers per module** (`logger = logging.getLogger(__name__)`)
✅ Use appropriate **levels** (`DEBUG` for dev, `INFO` for prod, `WARNING+` for alerts)
✅ Use **handlers** for structured logging (console + file + external system)
✅ Don’t log sensitive data (passwords, tokens)
✅ Consider using `RotatingFileHandler` in production to avoid huge log files

---

# 🔹 11. External Tools

* **Log aggregation**: ELK Stack (Elasticsearch + Logstash + Kibana), Graylog, Datadog
* **Structured logging**: `jsonlogger` → logs in JSON for machine parsing
* **Async logging**: `QueueHandler` for non-blocking logging

---

👉 Would you like me to create a **visual mindmap/cheatsheet** of Python logging (levels, handlers, best practices) so you can revise it quickly?


***<----------------can you give example for logging using point no 10 .. Best Practices and also addHanlder for file !!---------------------->***Got it 👍 Let’s put **Point #10 (Best Practices)** into an **end-to-end example** with both **console handler** and **file handler**.

---

# ✅ Example: Logging with Best Practices

```
project/
│── main.py
│── worker.py
```

---

### 🔹 `main.py`

```python
import logging
from logging.handlers import RotatingFileHandler
from worker import do_work

# -------------------------------
# 1. Create logger
# -------------------------------
logger = logging.getLogger("my_app")   # app-wide logger
logger.setLevel(logging.DEBUG)         # capture everything (handlers will filter)

# -------------------------------
# 2. Create handlers
# -------------------------------

# Console handler (for devs)
console_handler = logging.StreamHandler()
console_handler.setLevel(logging.WARNING)   # only show WARNING+

# File handler (rotating logs)
file_handler = RotatingFileHandler(
    "app.log", maxBytes=5000, backupCount=3
)
file_handler.setLevel(logging.INFO)         # record INFO+

# -------------------------------
# 3. Define formatter
# -------------------------------
formatter = logging.Formatter(
    "%(asctime)s | %(name)s | %(levelname)s | %(message)s"
)

# Attach formatter
console_handler.setFormatter(formatter)
file_handler.setFormatter(formatter)

# -------------------------------
# 4. Add handlers to logger
# -------------------------------
logger.addHandler(console_handler)
logger.addHandler(file_handler)

# -------------------------------
# 5. Example usage
# -------------------------------
logger.debug("Starting application in DEBUG mode")  # won't show on console
logger.info("Application started successfully")     # goes to file
logger.warning("Low disk space!")                   # goes to console + file

try:
    result = do_work()
    logger.info(f"Work result: {result}")
except Exception:
    logger.exception("Something went wrong during work execution")
```

---

### 🔹 `worker.py`

```python
import logging

# Use module-specific logger
logger = logging.getLogger(__name__)

def do_work():
    logger.debug("Starting work...")   # captured (but not shown in console)
    # Example error
    x = 10 / 0   # will raise exception
    return x
```

---

### 🔹 Behavior

1. **Console Output** → only `WARNING+` (short + real-time alerts).

   ```
   2025-09-03 07:10:00,321 | my_app | WARNING | Low disk space!
   2025-09-03 07:10:00,322 | my_app | ERROR | Something went wrong during work execution
   ```

2. **File (`app.log`)** → everything `INFO+`, including from `worker.py`.

   ```
   2025-09-03 07:10:00,320 | my_app | INFO | Application started successfully
   2025-09-03 07:10:00,321 | my_app | WARNING | Low disk space!
   2025-09-03 07:10:00,322 | worker | DEBUG | Starting work...
   2025-09-03 07:10:00,323 | my_app | ERROR | Something went wrong during work execution
   Traceback (most recent call last):
     ...
   ZeroDivisionError: division by zero
   ```

---

### 🔹 Why this is "Best Practice"

* ✅ Use `getLogger(__name__)` → separates logs by module
* ✅ Use **handlers** → console + file
* ✅ Use **different levels per handler** (console = high priority, file = detailed)
* ✅ Use **rotating file handler** to avoid infinite log growth
* ✅ Use `logger.exception()` inside `except` → captures stack trace

---

Do you want me to also show you how to make this **config-driven logging** (using a YAML/INI config file) so you don’t write all the setup code in `main.py`? That’s another best practice for production apps.

***<-------------what is rotating file handler and what is my_app and when takling logging at modular level what if we want to log from a particular file inside the module , in that case what should I write ?-------------->***
Excellent questions 👌 Let’s unpack them one by one.

---

# 🔹 1. What is **RotatingFileHandler**?

* It is a special **file handler** that prevents your log file from growing endlessly.
* You set a **maximum size (maxBytes)**, and when the file exceeds it, it **“rotates”**:

  * The current file is renamed (e.g., `app.log.1`)
  * A fresh `app.log` is created
  * Keeps up to `backupCount` old files

👉 Example:

```python
from logging.handlers import RotatingFileHandler

handler = RotatingFileHandler("app.log", maxBytes=5000, backupCount=3)
```

* When `app.log` > 5 KB:

  * `app.log.1`, `app.log.2`, `app.log.3` are created
  * Oldest gets deleted when limit is exceeded again

This avoids disk bloat (especially in production servers where logs run 24/7).

---

# 🔹 2. What is `"my_app"`?

When you do:

```python
logger = logging.getLogger("my_app")
```

* `"my_app"` is just the **logger name**.
* It’s a string you choose to group related logs.
* In practice, it’s common to use:

  * `"my_app"` → **application-wide logger** (main entry point)
  * `__name__` → **module-specific logger**

So `"my_app"` in the earlier example meant: *all logs from this application are tagged under this name*.

---

# 🔹 3. Logging at **Modular Level**

Best practice: in each file, you create a **module-specific logger** using:

```python
logger = logging.getLogger(__name__)
```

✅ Why?

* `__name__` automatically becomes the **module’s import path**.
* Example:

  ```
  project/
  │── main.py
  │── module/
      │── __init__.py
      │── worker.py
      │── db.py
  ```

Inside `worker.py`:

```python
import logging
logger = logging.getLogger(__name__)   # becomes "module.worker"

def do_work():
    logger.info("Doing work in worker module")
```

Inside `db.py`:

```python
import logging
logger = logging.getLogger(__name__)   # becomes "module.db"

def query():
    logger.warning("Database query took too long!")
```

Now your logs will clearly show which file/module they came from:

```
2025-09-03 07:25:10,123 | module.worker | INFO | Doing work in worker module
2025-09-03 07:25:11,456 | module.db | WARNING | Database query took too long!
```

---

# 🔹 4. If You Want Logs Only from a Particular File

Say you want **only `worker.py` logs** in a separate file:
You can add a **file handler** just to that logger.

Inside `worker.py`:

```python
import logging
from logging.handlers import RotatingFileHandler

logger = logging.getLogger(__name__)  # "module.worker"
logger.setLevel(logging.DEBUG)

# Add file handler specific to this module
file_handler = RotatingFileHandler("worker.log", maxBytes=2000, backupCount=2)
formatter = logging.Formatter("%(asctime)s | %(name)s | %(levelname)s | %(message)s")
file_handler.setFormatter(formatter)
logger.addHandler(file_handler)

def do_work():
    logger.info("This goes to worker.log")
```

👉 Now:

* `main.py` logs go to `app.log`
* `worker.py` logs go to both `app.log` (via root handlers) **and** `worker.log`

---

✅ **Summary**

* **RotatingFileHandler** = prevents huge log files by rotating them
* `"my_app"` = application-level logger name (you can name it anything)
* **At module level** → always use `logger = logging.getLogger(__name__)`
* **If you want per-file logs** → attach a **file handler** only in that file’s logger

---

Would you like me to sketch a **diagram of how loggers → handlers → formatters → outputs** are connected? That often makes this modular logging system very intuitive.

***<---------------how does getLogger wokrs ? does it has my_app prebuild or something ?------------->***

Great question 🙌 — this is the “aha!” point about logging in Python.
Let’s peel back how `logging.getLogger()` actually works.

---

# 🔹 1. Does `logging` have `"my_app"` prebuilt?

👉 **No.**
When you call:

```python
logger = logging.getLogger("my_app")
```

* Python’s logging system checks:

  > “Do I already have a logger with the name `'my_app'`?”

  * If **yes** → it returns the existing logger (same object).
  * If **no** → it **creates a new Logger object**, registers it in a global dictionary, and returns it.

So `"my_app"` is just a **name string you invented**, not prebuilt.

---

# 🔹 2. How `getLogger()` Works (Internally)

* Python’s `logging` module has a **global registry** of loggers (`logging.Logger.manager.loggerDict`).
* When you call `logging.getLogger(name)`:

  1. If `name` is `None` or empty → returns the **root logger**.
  2. If `name` is given:

     * Look up `loggerDict[name]`.
     * If not found → create a new `Logger(name)` object, add it to `loggerDict`.
     * Return the logger object.

So you can think of it as a **singleton factory** per name.

---

# 🔹 3. Example

```python
import logging

a = logging.getLogger("my_app")
b = logging.getLogger("my_app")

print(a is b)   # True ✅ same object
```

👉 Both `a` and `b` point to the **same logger**, because `"my_app"` is registered once.
This is how loggers stay consistent across modules.

---

# 🔹 4. Hierarchy of Logger Names

Logger names form a **dot-separated hierarchy**.
Example:

```python
main_logger   = logging.getLogger("my_app")
worker_logger = logging.getLogger("my_app.worker")
db_logger     = logging.getLogger("my_app.worker.db")
```

* `my_app.worker` is a **child** of `my_app`.
* By default, children **propagate** their log messages to parents, unless you disable it:

  ```python
  worker_logger.propagate = False
  ```

So if you log from `my_app.worker`, it can bubble up to `my_app`’s handlers (and then to root).

---

# 🔹 5. Why `__name__` is Used

When you do:

```python
logger = logging.getLogger(__name__)
```

* If the file is `worker.py` inside a package `my_app`, then `__name__` = `"my_app.worker"`.
* This creates a **hierarchical logger tree**:

  ```
  my_app (root app logger)
     └── my_app.worker
          └── my_app.worker.db
  ```
* This way you can control logging **per module or per package**.

---

# 🔹 6. Visualization of How It Works

```
[logging.getLogger("my_app.worker")] ---> (Logger object)
       |
       |---> Handlers (console, file, etc.)
       |---> Formatter (defines log format)
       |
       V
     Log message
       |
       |---> (propagates upward)
       |
[logging.getLogger("my_app")] ---> (parent logger)
       |
       V
[root logger]
```

---

✅ **Summary**

* `getLogger("my_app")` does **not** use a prebuilt logger. It **creates/reuses** a logger with that name.
* Loggers are stored in a **registry dictionary** and reused (singleton style).
* Logger names form a **hierarchy** (`my_app.worker` → child of `my_app`).
* `__name__` is used so each module gets a unique logger tied to its import path.

---

Would you like me to show you an **experiment in code** where we print out the logger dictionary (`loggerDict`) to see how Python stores `"my_app"`, `"my_app.worker"`, etc., internally?


***<----------logger.setLevel(logging.INFO) and file_handler.setLevel(logging.DEBUG) why are they Incorrect ?--------------->***

That's an excellent and very common question about how Python's logging module works. The configuration `logger.setLevel(logging.INFO)` and `file_handler.setLevel(logging.DEBUG)` is incorrect because of the **two-tiered filtering system** that the logging module uses.

Think of it like a funnel with two filters. A log message must pass through the first filter (the **logger's level**) before it can even be considered by the second filter (the **handler's level**).

### The Logging Funnel

1.  **The Logger's Filter (The Primary Gatekeeper):**
    The `logger.setLevel(logging.INFO)` command sets the minimum severity level that the logger will even bother to process. Any message with a level lower than `INFO` (e.g., `DEBUG`) will be completely ignored and discarded at this first step. It never gets a chance to be sent to a handler.

2.  **The Handler's Filter (The Secondary Gatekeeper):**
    The `file_handler.setLevel(logging.DEBUG)` command tells the handler what level of messages it is willing to write to the file. In your case, it is willing to write `DEBUG` messages.

### Why Your Configuration Fails

Your configuration sets up a contradiction.

  * A `DEBUG` message is created.
  * It goes to the logger. The logger's level is set to `INFO`.
  * The logger asks: "Is this message's level (`DEBUG`) equal to or higher than my level (`INFO`)?"
  * The answer is **No**. `DEBUG < INFO`.
  * The logger immediately drops the message. It never reaches the `file_handler`, which is configured to accept it.

The handler's level setting is meaningless if the logger has already filtered out the message before it ever gets there.

### The Correct Configuration

To log `DEBUG` messages, both the logger and the handler must have a level of `DEBUG` or lower.

```python
# The correct way to log DEBUG messages

# Set the logger's level to DEBUG
logger.setLevel(logging.DEBUG)

# Set the handler's level to DEBUG
file_handler.setLevel(logging.DEBUG)
```

With this change, a `DEBUG` message will pass the logger's filter (`DEBUG >= DEBUG`) and then also pass the handler's filter (`DEBUG >= DEBUG`), allowing it to be written to the file.

