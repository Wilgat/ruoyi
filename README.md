# ruoyi - One-Command Installer & Launcher for RuoYi Framework

<img src="https://img.shields.io/badge/Version-1.0.4-blue?style=flat-square" alt="Version">  
<img src="https://img.shields.io/badge/Java-21-orange?style=flat-square&logo=openjdk" alt="Java 21">  
<img src="https://img.shields.io/badge/Spring%20Boot-3.3.5-brightgreen?style=flat-square&logo=springboot" alt="Spring Boot 3.3">  
<img src="https://img.shields.io/badge/Maven-3.9.9-red?style=flat-square&logo=apachemaven" alt="Maven 3.9">  
<img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="MIT License">

**ruoyi** is a simple shell script that automates the entire setup process for the popular **RuoYi** open-source admin backend framework (https://github.com/yangzongzhuan/RuoYi).

With one command you can:

- Install the script itself (if not already present)
- Install & configure **SDKMAN!** → **Java 21 (Temurin)** → **Maven 3.9.13**
- Clone the latest RuoYi repository
- (Optional – root only) Install MySQL, create database/user, run initialization SQL
- Build and start the RuoYi backend server

Ideal for developers, students, or quick demos who want to get RuoYi running in minutes.

## Features

- **Zero manual configuration** for basic Java + Maven + RuoYi setup
- Supports **Java 21** (updated from original RuoYi's Java 17 requirement)
- Optional **MySQL auto-install + DB setup** (run as root with `ruoyi mysql`)
- Cleans up unused modules (generator, quartz, etc.) for a minimal footprint
- Colorful progress output & error handling
- Self-installing & self-updating via GitHub raw URL

## Requirements

- Linux / macOS (Ubuntu/Debian/CentOS tested)
- `curl`, `git`, `sed`, `mysql` client (for DB mode)
- Internet connection
- Root/sudo access **only** if you want automatic MySQL installation

## Quick Start

### 1. Normal installation & run (most users)

```bash
curl -fsSL https://raw.githubusercontent.com/Wilgat/ruoyi/main/ruoyi | bash
```

This will:
- Install the `ruoyi` command
- Set up Java 21 + Maven
- Clone RuoYi to `~/ruoyi`
- Build and start the server

Open browser: http://localhost:8080

Default login (after DB init):  
**admin** / **admin123**

### 2. One-time MySQL + DB setup (recommended first time – run as root)

```bash
sudo ruoyi mysql
```

This will:
- Install MySQL server
- Run `mysql_secure_installation`
- Prompt for root password, new DB user & password
- Create database `ry`
- Grant privileges
- Import `ry_20250416.sql` (or latest init script)

Then run normal start:

```bash
ruoyi
```

### 3. Update the script itself (when you push new version)

Just re-run the curl command — it will overwrite the old version.

## Usage

```bash
ruoyi               # Setup + clone + build + run
ruoyi mysql         # (sudo) Install MySQL + create DB + import SQL
ruoyi help          # Show this help
ruoyi version       # Show version
```

## Project Structure After Setup

```
~/ruoyi/
├── ruoyi-admin/           # Main backend module (runs on port 8080)
├── ruoyi-common/
├── ruoyi-framework/
├── ruoyi-quartz/          # Removed in minimal mode
├── ruoyi-generator/       # Removed in minimal mode
├── sql/                   # SQL init scripts
└── pom.xml
```

## Important Notes

- The script modifies `<java.version>` to **21** in the root `pom.xml`
- Some optional modules (code generator, scheduler) are removed to keep startup fast
- If you need those modules back → comment out the `rm -rf` lines in the script
- Database configuration is written to `ruoyi-admin/src/main/resources/application-druid.yml`
- Default port: **8080** (change via `application.yml` if needed)

## Troubleshooting

**Build fails?**

```bash
cd ~/ruoyi
mvn clean package -Dmaven.test.skip=true
```

**MySQL permission error?**

Make sure you're running `sudo ruoyi mysql` and entering the correct root password.

**Java version wrong?**

```bash
sdk list java
sdk install java 21-tem
```

**Port already in use?**

Kill the old process or change port in `application.yml`.

## Credits

- RuoYi framework: https://github.com/yangzongzhuan/RuoYi
- Inspired by simple one-click installers (SDKMAN, nvm, etc.)

## License

MIT
