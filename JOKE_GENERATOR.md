# Joke Generator Documentation

Random joke generator scripts using public APIs. No authentication required!

## Scripts

### 1. `joke_generator.py` — CLI Application

A command-line tool to fetch and display random jokes.

#### Installation

```bash
pip install requests
```

#### Usage

```bash
# Get a random joke from a random API
python3 joke_generator.py

# Get a joke from a specific source
python3 joke_generator.py --source jokeapi
python3 joke_generator.py --source official
python3 joke_generator.py --source advice

# Get multiple jokes
python3 joke_generator.py --count 5

# List available sources
python3 joke_generator.py --list

# Get 3 jokes from JokeAPI
python3 joke_generator.py --source jokeapi --count 3
```

#### Available Sources

| Source | Type | Example |
|--------|------|---------|
| `jokeapi` | Programming, knock-knock, misc | "Why do programmers prefer dark mode? Because light attracts bugs!" |
| `official` | General setup/punchline jokes | "Why did the scarecrow win? He was outstanding in his field!" |
| `advice` | Humorous advice | "Insanity is doing the same thing over and over and expecting different results." |

#### Output

```
======================================================================
📝 From: JokeAPI
======================================================================

Why do Java developers always wear glasses?
Because they do not C#!

======================================================================

✅ Successfully fetched 1/1 joke(s)
```

---

### 2. `joke_api_server.py` — Flask Web Server

A Flask web application that serves jokes via HTTP API.

#### Installation

```bash
pip install flask requests
```

#### Running the Server

```bash
python3 joke_api_server.py
```

Output:
```
======================================================================
🎭 Joke Generator API Server
======================================================================

Starting server on http://localhost:5000/
Press Ctrl+C to stop
```

#### Web Interface

Open http://localhost:5000/ in your browser for an interactive joke generator with a nice UI.

#### API Endpoints

##### `GET /` — Interactive Home Page
```
http://localhost:5000/
```

Returns an HTML page with a "Get Joke" button and API documentation.

##### `GET /joke` — Single Random Joke
```
http://localhost:5000/joke
```

Response:
```json
{
  "source": "jokeapi",
  "joke": "Why do Java developers always wear glasses? Because they do not C#!",
  "success": true
}
```

##### `GET /joke?source=SOURCE` — Joke from Specific Source
```
http://localhost:5000/joke?source=jokeapi
http://localhost:5000/joke?source=official
http://localhost:5000/joke?source=advice
```

##### `GET /joke?count=N` — Multiple Jokes
```
http://localhost:5000/joke?count=3
```

Response:
```json
{
  "jokes": [
    {"source": "official", "joke": "...", "success": true},
    {"source": "advice", "joke": "...", "success": true},
    {"source": "jokeapi", "joke": "...", "success": true}
  ],
  "count": 3
}
```

##### `GET /sources` — List Available Sources
```
http://localhost:5000/sources
```

Response:
```json
{
  "sources": ["jokeapi", "official", "advice"],
  "description": {
    "jokeapi": "JokeAPI - Programming, knock-knock, and miscellaneous jokes",
    "official": "Official Joke API - General jokes with setup/punchline",
    "advice": "Advice Slip API - Humorous advice"
  }
}
```

#### Using with `curl`

```bash
# Single joke
curl http://localhost:5000/joke | jq

# Multiple jokes
curl "http://localhost:5000/joke?count=5" | jq

# From specific source
curl "http://localhost:5000/joke?source=jokeapi&count=2" | jq

# List sources
curl http://localhost:5000/sources | jq
```

#### Using with Python

```python
import requests

# Single joke
response = requests.get("http://localhost:5000/joke")
print(response.json())

# Multiple jokes
response = requests.get("http://localhost:5000/joke?count=3")
for joke_data in response.json()["jokes"]:
    print(joke_data["joke"])
```

#### Using with JavaScript (Browser)

```javascript
// Fetch a joke
fetch('/joke')
  .then(response => response.json())
  .then(data => console.log(data.joke));

// Fetch multiple jokes
fetch('/joke?count=5')
  .then(response => response.json())
  .then(data => {
    data.jokes.forEach(j => console.log(j.joke));
  });
```

---

## External APIs Used

### 1. JokeAPI
- **URL**: https://v2.jokeapi.dev/
- **Type**: Programming jokes, knock-knock jokes, miscellaneous jokes
- **Rate Limit**: Generous (no key required)
- **Docs**: https://jokeapi.dev/

### 2. Official Joke API
- **URL**: https://official-joke-api.appspot.com/
- **Type**: General jokes with setup/punchline format
- **Rate Limit**: No rate limit (no key required)
- **Docs**: https://github.com/15Dkatz/official_joke_api

### 3. Advice Slip API
- **URL**: https://api.adviceslip.com/
- **Type**: Humorous advice slips
- **Rate Limit**: Generous (no key required)
- **Docs**: https://api.adviceslip.com/

---

## Features

✅ **No API Keys Required** — All APIs are free and public  
✅ **Multiple Sources** — Choose or randomize joke source  
✅ **Error Handling** — Graceful fallbacks if APIs are down  
✅ **CLI & Web Interface** — Use via command line or browser  
✅ **JSON API** — Programmatically fetch jokes  
✅ **Well-Documented** — Clear examples and documentation  

---

## Error Handling

Both scripts include comprehensive error handling:

- **Network timeouts** — Falls back gracefully with appropriate messages
- **API failures** — Retries or uses alternative sources
- **Invalid parameters** — Clear error messages
- **Rate limiting** — Respects API rate limits

---

## Examples

### Example 1: Get 5 Random Jokes via CLI

```bash
python3 joke_generator.py --count 5
```

### Example 2: Deploy Joke API Server

```bash
# Start server
python3 joke_api_server.py

# In another terminal, fetch jokes
curl http://localhost:5000/joke?count=10 | jq '.jokes[] | .joke'
```

### Example 3: Integration with Other Apps

```python
import requests

def get_joke():
    response = requests.get("http://localhost:5000/joke")
    data = response.json()
    return data["joke"] if data["success"] else "No joke available"

# Use in your app
print(get_joke())
```

---

## Troubleshooting

### "No module named 'requests'"
```bash
pip install requests
```

### "No module named 'flask'"
```bash
pip install flask
```

### "Connection refused"
Make sure the server is running with `python3 joke_api_server.py`

### "Failed to fetch joke"
Check your internet connection or try a different source.

---

## License

These scripts are free to use and modify. The external APIs are public and free.
