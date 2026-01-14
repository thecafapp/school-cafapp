# The Caf App School Template

This is a template repository for hosting The Caf App for a school based on the new Caf App structure.  The Caf App uses a **submodule-driven architecture**. This allows us to maintain a single codebase for the app itself while deploying unique, branded instances for various universities.  The repository you are currently in is the school-specific repo.  It acts as the host and provides the specific identity and public assets for the school.  There are two config files that define how the app runs - the [`caf.config.json`](/caf.config.json) and the `.env.local`.

## 📄 `caf.config.json` Configuration

This documentation defines the configuration file used by **The Caf App** core engine to brand and configure school-specific deployments.  The configuration file is a JSON object located in the root of each school-specific repository. It is injected into the Core engine at build time via Webpack aliases and environment variables.  

### Root Properties

| Key | Type | Description |
| --- | --- | --- |
| `schoolName` | `string` | The full name of the institution (e.g., "Mississippi College"). |
| `schoolId` | `string` | A unique lowercase identifier (often the subdomain) |
| `featureFlags` | `array` | A list of strings used to toggle specific UI elements or beta features. |

---

### `branding`

Defines the public-facing identity of the specific instance.

| Key | Type | Description |
| --- | --- | --- |
| `appTitle` | `string` | The title displayed in the browser tab and PWA install prompt. |
| `cafName` | `string` | The common name for the main dining hall. |
| `domain` | `string` | The production URL where this specific instance is hosted. |

---

### `auth`

Configures the login requirements for the student body.

| Key | Type | Description |
| --- | --- | --- |
| `method` | `string` | The authentication provider (e.g., `"google"`). |
| `restrictGoogleDomains` | `boolean` | If true, only users with a specific email suffix can log in. |
| `allowedGoogleDomain` | `string` | The required email domain (e.g., `"mc.edu"`). |

---

### `theme`

Contains color specifications for both `light` and `dark` modes. These are injected into the DOM as CSS variables.

#### Theme Object Structure

Each mode (`light`/`dark`) contains the following objects:

* **`text`**:
    * `primary`: Main body and heading color.
    * `secondary`: Subtitles and deactivated labels.


* **`background`**:
    * `primary`: Main page background.
    * `secondary`: Card and modal background colors.


* **`gradient`**:
    * `start` / `end`: Defines the primary brand gradient used in headers or buttons.


* **`leaderboard`**:
    * `highlight`: Accent color for top-ranking users.


* **`tap-highlight`**:
    * The overlay color (usually RGBA) shown when a user touches an interactive element.


### `database`

Server-side configuration for data persistence.

| Key | Type | Description |
| --- | --- | --- |
| `name` | `string` | The specific database or collection name to target for this school's data. |

---

It is critical that these values are kept in a `.env.local` file, which should **never** be committed to Git. These variables handle the connection to your database and administrative access to Firebase.

---

## `.env.local` Configuration

This file stores sensitive credentials and environment-specific variables. While the `caf.config.json` handles the public app configuration, this file handles **private infrastructure access.**

### 1. Database Configuration

| Variable | Description |
| --- | --- |
| `CAFMONGO` | The MongoDB connection string. |
| `NEXT_PUBLIC_FIREBASE_APIKEY` | The public API key used by the Firebase Client SDK to initialize the connection for authentication. |
| `FIREBASE_PRIVATE_KEY` | The RSA private key for the Firebase Service Account. It must include the `-----BEGIN PRIVATE KEY-----` and `-----END PRIVATE KEY-----` headers. |
| `FIREBASE_EMAIL` | The service account email address associated with your Google Cloud/Firebase project. |