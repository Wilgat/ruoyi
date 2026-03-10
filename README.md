# ruoyi - One-Command Installer & Launcher for RuoYi Framework

<img src="https://img.shields.io/badge/Version-1.0.6-blue?style=flat-square" alt="Version">  
<img src="https://img.shields.io/badge/Java-21-orange?style=flat-square&logo=openjdk" alt="Java 21">  
<img src="https://img.shields.io/badge/Spring%20Boot-3.3.5-brightgreen?style=flat-square&logo=springboot" alt="Spring Boot 3.3">  
<img src="https://img.shields.io/badge/Maven-3.9.9-red?style=flat-square&logo=apachemaven" alt="Maven 3.9">  
<img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="MIT License">

**ruoyi** is a simple shell script that automates the entire setup process for the popular **RuoYi** open-source admin backend framework (https://github.com/yangzongzhuan/RuoYi).

With one command you can:

- Install the script itself (if not already present)
- Set up the full development environment (SDKMAN! → Java 21 → Maven)
- Clone the official RuoYi repository
- (Optional – root only) Install MySQL, create database/user, and import initialization SQL
- Build and start the RuoYi backend server

Ideal for developers, students, or quick demos who want to get RuoYi running in minutes.

## Features

- **Zero manual configuration** for basic Java + Maven + RuoYi setup
- Uses **Java 21 (Temurin)** — updated from original RuoYi's Java 17
- Optional **MySQL auto-install + DB setup** (run as root with `ruoyi mysql`)
- Clean error handling and colorful progress output
- Self-installing & self-updating via GitHub raw URL

## Requirements

- Linux / macOS (Ubuntu/Debian/CentOS tested)
- `curl`, `git`, `sed`
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
- Prompt for root password, DB name/user/password
- Create database `ry`
- Import SQL initialization script
- Update database config in `application-druid.yml`

Then run normal start:

```bash
ruoyi
```

### 3. Update the script itself

Just re-run the curl command — it will overwrite the old version.

## Usage

```bash
ruoyi               # Setup + clone + build + run
ruoyi mysql         # (sudo) Install MySQL + create DB + import SQL
ruoyi help          # Show this help
ruoyi version       # Show version
```

## Dependency Sequence of Tasks

The script follows this strict order to ensure each step has what it needs before proceeding:

1. **Install self**  
   Checks if the `ruoyi` command is already installed → if not, downloads and installs itself to `~/.local/bin` or `/usr/local/bin`

2. **Install SDKMAN!**  
   Downloads and installs SDKMAN! (tool manager for Java & Maven) → loads it in the current session

3. **Install Java 21 (Temurin)**  
   Uses SDKMAN! to install and set as default Java 21 (Eclipse Temurin distribution)

4. **Install Maven 3.9.13**  
   Uses SDKMAN! to install and set as default Maven

5. **Clone the RuoYi project**  
   Removes any old `~/ruoyi` folder → clones fresh copy from https://github.com/yangzongzhuan/RuoYi.git

6. **Build**  
   Updates `<java.version>` to 21 in `pom.xml` → runs `mvn clean package -Dmaven.test.skip=true` → starts the server with `java -jar ruoyi-admin/target/ruoyi-admin.jar`

7. **MySQL + Database setup** (only if run with `mysql` argument as root/sudo)  
   - Installs MySQL server  
   - Runs security setup  
   - Prompts for root password, DB name/user/password  
   - Creates database & user  
   - Imports latest `ry_*.sql` script  
   - Updates `ruoyi-admin/src/main/resources/application-druid.yml`

This order ensures:
- Tools are ready before cloning or building
- No wasted time cloning if environment setup fails
- Safe to re-run multiple times

## Project Structure After Setup

```
~/ruoyi/
├── ruoyi-admin/           # Main backend module (runs on port 8080)
├── ruoyi-common/
├── ruoyi-framework/
├── ruoyi-system/
├── ruoyi-quartz/          # Included (scheduler)
├── ruoyi-generator/       # Included (code generator)
├── sql/                   # SQL init scripts
└── pom.xml
```

## Important Notes

- The script automatically sets Java version to **21** in `pom.xml`
- Default port: **8080** (change via `application.yml` if needed)
- Database config is updated automatically only in `mysql` mode

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
