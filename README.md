# 🎬 Movie App Integration

## API Overview

The Movie Database (TMDb) API provides rich, community-driven movie, TV show, and people data. It includes endpoints for searching, discovering, and viewing detailed info such as cast, reviews, images, ratings, trending items, and more. It also supports account-based actions like watchlists and ratings. [oai_citation:0‡docs.rs](https://docs.rs/crate/tmdb_client/latest?utm_source=chatgpt.com)

## Version

This project uses **TMDb API version 3**. The Rust client library references “API version: 3” in its docs.

## Available Endpoints

- `GET /movie/popular` – Fetches a list of popular movies
- `GET /movie/now_playing` – Movies currently in theaters
- `GET /movie/top_rated` – Highest-rated movies
- `GET /movie/upcoming` – Upcoming movie releases
- `GET /movie/{movie_id}` – Details for a specific movie
- `GET /search/movie` – Search movies by title
- `GET /person/popular` – Trending actors
- `GET /trending/{media_type}/{time_window}` – Trending media (all types)  
  _(…plus many more for TV shows, people, accounts, reviews, images, etc.)_ [oai_citation:1‡docs.rs](https://docs.rs/crate/tmdb_client/latest?utm_source=chatgpt.com)

## Request and Response Format

Most requests are HTTP `GET` requests with parameters included in the URL query string, and responses are in JSON format.

**Request example:**
GET https://api.themoviedb.org/3/movie/550?api_key=YOUR_API_KEY&language=en-US
**Example response:**

```json
{
  "id": 550,
  "title": "Fight Club",
  "overview": "...",
  "release_date": "1999-10-15",
  "genres": [
    { "id": 18, "name": "Drama" }
  ],
  "vote_average": 8.4,
  ...
}
```

## Authentication

TMDb offers two authentication methods:

# 1. API Key (v3)

Required as a query string:
?api_key=YOUR_API_KEY
Or alternatively in headers (if supported by a wrapper).

# 2. Bearer Token (v4 only)

Sent in the header:
Authorization: Bearer YOUR_ACCESS_TOKEN
For basic read access, v3 API keys are sufficient.

## Authentication

TMDb offers two primary methods for authenticating API requests:

1. **API Key (v3 Authentication)**

   - Add your API key as a query parameter:
     ```
     https://api.themoviedb.org/3/movie/550?api_key=YOUR_API_KEY
     ```

2. **Bearer Token (v4 Authentication)**
   - Use an HTTP header with a Bearer token:
     ```
     Authorization: Bearer YOUR_ACCESS_TOKEN
     ```

For most basic data access (search, movie details, etc.), the API key method is sufficient.

---

## Error Handling

TMDb returns HTTP status codes along with a JSON object containing a `status_code` and a human-readable `status_message`.

| HTTP Code | `status_code` | Description                       |
| --------- | ------------- | --------------------------------- |
| 401       | 7, 32         | Invalid or missing authentication |
| 404       | 34            | Resource not found                |
| 422       | 5             | Invalid parameters in request     |
| 429       | -             | Too many requests (rate-limited)  |
| 500+      | -             | Server-side error                 |

**Sample error response:**

```json
{
  "status_code": 34,
  "status_message": "The resource you requested could not be found."
}

## Usage Limits and Best Practices

### Usage Limits

The TMDb API enforces usage limits to ensure fair access and system stability:

- **Rate Limit**: Approximately **40 requests every 10 seconds** per IP address for API v3.
- **Page Limit**: Pagination for list endpoints (e.g., `/movie/popular`) is capped at **500 pages**.
- **Key Suspension**: Excessive violations of rate limits or misuse may result in your API key being **temporarily suspended** or **permanently revoked**.

You can monitor your usage and rate limit status via TMDb's account dashboard.

---

### Best Practices

To use the API efficiently and avoid errors:

- **Cache responses** where possible, especially for popular or static data like movie lists or genres.
- **Paginate** results using `page` query parameters to handle large datasets.
- **Throttle or debounce** API calls in your UI (e.g., for search fields) to avoid flooding the API.
- **Retry failed requests** with exponential backoff when you receive `429 Too Many Requests`.
- **Handle errors gracefully** to ensure a stable user experience even when API limits are hit.
- **Use compression** (e.g., `Accept-Encoding: gzip`) to reduce response size.
- **Follow attribution requirements**:
  > *This product uses the TMDb API but is not endorsed or certified by TMDb.*

Implementing these practices will keep your application efficient, user-friendly, and compliant with TMDb’s terms of use.
```
