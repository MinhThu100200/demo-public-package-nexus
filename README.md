# Public Package Demo with Nexus

This project demonstrates how to **publish** and **install** an npm package using a self-hosted [Nexus Repository Manager](https://www.sonatype.com/products/repository-oss).

---

## 📁 Folder Structure

```
/PublicPackage
│
├── parity-checker             # The package to be published
│   ├── index.js
│   ├── package.json
│   ├── publish.sh
│   └── .npmrc                 # Auth config for Nexus
│
├── parity-demo-install        # A demo project to install the package
│   ├── index.js               # (you can create this)
│   ├── package.json
│   └── .npmrc
│
├── docker-compose.yml         # Nexus setup
└── README.md
```

---

## 🛠 Prerequisites

- Docker & Docker Compose
- Node.js & npm
- Nexus OSS accessible at `http://localhost:8081`

---

## 🚀 Step-by-Step Instructions

### 🐳 1. Start Nexus via Docker Compose

```bash
docker-compose up -d
```

Then access Nexus at: [http://localhost:8081](http://localhost:8081)

Default credentials:

- **Username:** `admin`
- **Password:** `mitu1002` _(or check initial admin password from container logs)_

---

### ⚙️ 2. Configure Nexus Repositories

Login to the Nexus UI and create the following repositories:

#### ➕ Create npm-hosted Repository

- **Name:** `npm-hosted`
- **Type:** `npm (hosted)`
- **Deployment policy:** `Allow redeploy`
- **Blob store:** `default`
- Click **Create repository**

#### 🌐 Create npm-proxy Repository

- **Name:** `npm-proxy`
- **Type:** `npm (proxy)`
- **Remote URL:** `https://registry.npmjs.org/`
- Click **Create repository**

#### 🧩 Create npm-group Repository

- **Name:** `npm-group`
- **Type:** `npm (group)`
- **Member Repositories:** `npm-hosted`, `npm-proxy`
- Click **Create repository**

---

### 📦 3. Publish a Package

Navigate to the `parity-checker` folder:

1. Ensure `.npmrc` contains:

```ini
@demo:registry=http://127.0.0.1:8081/repository/npm-hosted/
//127.0.0.1:8081/repository/npm-hosted/:_auth=dXNlcjptaXR1MTAwMg==
//127.0.0.1:8081/repository/npm-hosted/:email=admin@example.org
//127.0.0.1:8081/repository/npm-hosted/:always-auth=true
```

2. Run the publish script:

```bash
./publish.sh
```

---

### 📥 4. Install Package in Another Project

Navigate to `parity-demo-install`:

1. Ensure `.npmrc` has:

```ini
@demo:registry=http://127.0.0.1:8081/repository/npm-group/
```

2. Run:

```bash
npm install @demo/parity-checker
```

You should now be able to import and use the package in your code.

---

## ✅ Done!

You've now successfully published and consumed an npm package using your own Nexus repository.
