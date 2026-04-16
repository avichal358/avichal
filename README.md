# avichal

A personal WordPress-based website project for [avichal358](https://github.com/avichal358). The repository tracks the custom theme, plugin, and configuration files that make up the site while keeping WordPress core, uploads, caches, and secrets out of version control (see `.gitignore`).

## Purpose

The goal of this project is to provide a version-controlled home for the custom parts of a self-hosted WordPress site. It lets the site be:

- Developed locally and deployed reproducibly to a hosting environment.
- Shared and reviewed through pull requests instead of editing files directly on the server.
- Extended over time with custom themes, plugins, and site content without polluting the repo with generated files, user uploads, or credentials.

## Technologies Used

- **[WordPress](https://wordpress.org/)** – the underlying CMS the site is built on.
- **PHP 7.4+** – required by modern versions of WordPress.
- **MySQL 5.7+ / MariaDB 10.3+** – database backing WordPress.
- **Apache or Nginx** – web server to serve WordPress (Apache is assumed by the `.htaccess` entry in `.gitignore`).
- **HTML, CSS, and JavaScript** – used in themes and front-end assets.
- **Git** – for version control of themes, plugins, and site configuration.

## Repository Layout

The repository is intended to live alongside a WordPress installation. Files and directories that are managed by WordPress itself or contain environment-specific data are excluded via `.gitignore`, including:

- `wp-config.php` – contains database credentials and secret keys.
- `wp-content/uploads/`, `wp-content/cache/`, `wp-content/backups/`, `wp-content/backup-db/`, `wp-content/upgrade/`, `wp-content/blogs.dir/` – generated or user-uploaded content.
- `wp-content/advanced-cache.php`, `wp-content/wp-cache-config.php` – cache plugin artifacts.
- `wp-content/plugins/hello.php` – the default "Hello Dolly" plugin shipped with WordPress.
- `.htaccess`, `license.txt`, `readme.html`, `sitemap.xml`, `sitemap.xml.gz` – server/runtime files that shouldn't be tracked.
- `*.log` – log files from WordPress, PHP, or the web server.

## Setup Instructions

### Prerequisites

Make sure the following are installed and available on your machine:

- PHP 7.4 or later with the standard WordPress extensions (`mysqli`, `mbstring`, `curl`, `gd`, `xml`, `zip`).
- MySQL 5.7+ or MariaDB 10.3+.
- A web server such as Apache (with `mod_rewrite`) or Nginx.
- [Git](https://git-scm.com/).
- Optional: [WP-CLI](https://wp-cli.org/) for managing WordPress from the command line, and a local development environment such as [Local](https://localwp.com/), [XAMPP](https://www.apachefriends.org/), [MAMP](https://www.mamp.info/), or [Docker](https://www.docker.com/) with a WordPress image.

### 1. Clone the repository

```bash
git clone https://github.com/avichal358/avichal.git
cd avichal
```

### 2. Install WordPress

Download WordPress into this directory (or point your web server's document root at this directory after placing WordPress core files here):

```bash
# Using WP-CLI
wp core download

# Or manually
curl -O https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz --strip-components=1
rm latest.tar.gz
```

### 3. Create the database

Create an empty database and a user with full privileges on it:

```sql
CREATE DATABASE avichal_wp CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'avichal_wp'@'localhost' IDENTIFIED BY 'change-me';
GRANT ALL PRIVILEGES ON avichal_wp.* TO 'avichal_wp'@'localhost';
FLUSH PRIVILEGES;
```

### 4. Configure `wp-config.php`

Copy the sample config and fill in the database credentials and secret keys:

```bash
cp wp-config-sample.php wp-config.php
```

Then edit `wp-config.php` and set `DB_NAME`, `DB_USER`, `DB_PASSWORD`, and `DB_HOST` to match the database created above. Generate fresh secret keys at <https://api.wordpress.org/secret-key/1.1/salt/> and paste them into the "Authentication Unique Keys and Salts" block.

> `wp-config.php` is listed in `.gitignore` so that credentials are never committed.

### 5. Run the WordPress installer

Serve the site locally (for example with PHP's built-in server) and walk through the installation wizard:

```bash
php -S localhost:8000
```

Then open <http://localhost:8000> in your browser and follow the on-screen instructions to finish the WordPress install (site title, admin user, etc.).

Alternatively, with WP-CLI:

```bash
wp core install \
    --url="http://localhost:8000" \
    --title="avichal" \
    --admin_user="admin" \
    --admin_password="change-me" \
    --admin_email="you@example.com"
```

### 6. Start developing

Custom themes and plugins live under `wp-content/themes/` and `wp-content/plugins/`. Commit the ones that belong to this project; generated and third-party files are excluded by `.gitignore`.

## Contributing

1. Create a feature branch from `master`.
2. Make your changes and commit them with a clear message.
3. Open a pull request against `master` for review.

## License

No license has been specified for this repository yet. Until one is added, all rights are reserved by the repository owner.
