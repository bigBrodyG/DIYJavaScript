# 🚀 GitHub Actions Workflows

Questa directory contiene i workflow per CI/CD del Web Scraper.

## 📋 Workflows Disponibili

### 1. **docker-build.yml** (Attivo di default)
Build automatico dell'immagine Docker e push su GitHub Container Registry.

**Trigger:** Push su main, PR, manuale
**Output:** Immagine Docker su `ghcr.io`

### 2. **test-and-lint.yml** (Consigliato)
Test automatici, linting e validazione del codice.

**Trigger:** Push su main/develop, PR
**Cosa fa:**
- Controllo formattazione con Black
- Linting con Flake8
- Test build Docker
- Health check del container

### 3. **deploy-railway.yml**
Deploy automatico su Railway.app

**Setup richiesto:**
1. Vai su https://railway.app
2. Crea un nuovo progetto
3. Ottieni il token API: Settings → Tokens
4. Aggiungi il secret `RAILWAY_TOKEN` su GitHub

### 4. **deploy-render.yml**
Deploy automatico su Render.com

**Setup richiesto:**
1. Vai su https://render.com
2. Crea un Web Service
3. Vai su Settings → Deploy Hook
4. Copia l'URL e aggiungilo come secret `RENDER_DEPLOY_HOOK_URL`

### 5. **deploy-fly.yml**
Deploy automatico su Fly.io

**Setup richiesto:**
```bash
# Installa flyctl
curl -L https://fly.io/install.sh | sh

# Login
flyctl auth login

# Inizializza l'app
cd Threads_exercises
flyctl launch --no-deploy

# Ottieni il token
flyctl auth token
```
Aggiungi il token come secret `FLY_API_TOKEN` su GitHub

### 6. **deploy-vps.yml**
Deploy su un VPS (server dedicato)

**Setup richiesto:**
Aggiungi questi secrets su GitHub:
- `VPS_HOST`: IP o dominio del server
- `VPS_USERNAME`: Username SSH (es: root, ubuntu)
- `VPS_SSH_KEY`: Chiave privata SSH
- `VPS_PORT`: Porta SSH (default: 22)

**Preparazione VPS:**
```bash
# Sul server
sudo apt update
sudo apt install docker.io docker-compose git -y
sudo usermod -aG docker $USER
git clone https://github.com/bigBrodyG/DIYJavaScript.git /opt/webscraper
cd /opt/webscraper/Threads_exercises
```

## 🔐 Configurazione Secrets

### Come aggiungere secrets su GitHub:
1. Vai su: Repository → Settings → Secrets and variables → Actions
2. Click su "New repository secret"
3. Aggiungi nome e valore
4. Salva

### Secrets necessari per servizio:

| Servizio | Secret | Descrizione |
|----------|--------|-------------|
| Railway | `RAILWAY_TOKEN` | Token API di Railway |
| Render | `RENDER_DEPLOY_HOOK_URL` | URL webhook deploy |
| Fly.io | `FLY_API_TOKEN` | Token API Fly.io |
| VPS | `VPS_HOST` | IP del server |
| VPS | `VPS_USERNAME` | Username SSH |
| VPS | `VPS_SSH_KEY` | Chiave privata SSH |
| VPS | `VPS_PORT` | Porta SSH (opzionale) |

## 🎯 Quale Workflow Usare?

### Per iniziare rapidamente:
1. Abilita `docker-build.yml` (già attivo)
2. Abilita `test-and-lint.yml` per qualità del codice

### Per deploy cloud gratuito:
- **Railway**: Più semplice, limite 500 ore/mese
- **Render**: Gratuito con limitazioni, più lento
- **Fly.io**: Ottimo per Docker, 3 VM gratis

### Per deploy professionale:
- **VPS**: Controllo totale, più economico su larga scala

## 🔄 Come Abilitare/Disabilitare Workflows

I workflow vengono eseguiti automaticamente in base ai trigger definiti.

Per disabilitare temporaneamente un workflow:
1. Rinomina il file aggiungendo `.disabled` (es: `deploy-railway.yml.disabled`)
2. Oppure commenta la sezione `on:` nel file

Per eseguire manualmente:
1. Vai su Actions nel repository GitHub
2. Seleziona il workflow
3. Click su "Run workflow"

## 📊 Monitoraggio

Visualizza lo stato dei workflow:
1. Tab "Actions" su GitHub
2. Vedi tutti i run passati, presenti, falliti
3. Click su un run per vedere i dettagli

## 🐛 Troubleshooting

### Workflow fallisce?
1. Controlla i logs dettagliati su Actions
2. Verifica che tutti i secrets siano configurati
3. Testa localmente: `docker-compose up --build`

### Permission denied?
Verifica che il workflow abbia i permessi:
```yaml
permissions:
  contents: read
  packages: write
```

## 🎉 Best Practices

1. **Testa sempre localmente** prima del push
2. **Usa branch protetti** per main
3. **Abilita required checks** per PR
4. **Monitora i workflow** regolarmente
5. **Ruota i secrets** periodicamente

## 📚 Risorse Utili

- [GitHub Actions Docs](https://docs.github.com/actions)
- [Railway Docs](https://docs.railway.app)
- [Render Docs](https://render.com/docs)
- [Fly.io Docs](https://fly.io/docs)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)
