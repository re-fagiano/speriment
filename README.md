# Agente autonomo Sora per video musicali

Questo repository descrive l'architettura e l'operatività di un agente autonomo che combina tracce audio generate con Suno e contenuti video creati tramite Sora per produrre video musicali completi.

## Requisiti
- Account dedicato Sora (email/password o cookie di sessione).
- Account Suno per la generazione delle tracce audio.
- Servizio Railway per l'esecuzione continua dell'agente.
- Repository Git contenente il codice dell'agente e la pipeline di post-produzione.

## Variabili d'ambiente richieste
Definire le seguenti variabili d'ambiente su Railway (o nel sistema locale durante lo sviluppo):

| Nome variabile        | Descrizione                                                                           |
|-----------------------|----------------------------------------------------------------------------------------|
| `SORA_EMAIL`          | Email di accesso all'account Sora dedicato.                                           |
| `SORA_PASSWORD`       | Password dell'account Sora dedicato (alternativamente un cookie di sessione sicuro).   |
| `SORA_API_KEY`        | Chiave API Sora, se disponibile.                                                      |
| `SUNO_API_KEY`        | Chiave API Suno per la generazione e il download delle tracce audio.                  |
| `RAILWAY_WEBHOOK`     | Webhook per notificare l'avanzamento del progetto o ricevere trigger esterni.         |
| `STORAGE_BUCKET_URL`  | Endpoint dello storage utilizzato per salvare asset temporanei e finali.             |
| `POSTPRO_API_TOKEN`   | Token per il servizio di post-produzione (es. DaVinci Resolve scripting server).      |

> Suggerimento: utilizzare prefissi coerenti (ad esempio `SORA_`, `SUNO_`) per mantenere organizzate le credenziali.

## Funzioni principali dell'agente
1. **Ingestione risorse**
   - Scarica la traccia Suno e ne estrae i metadati musicali (bpm, struttura, sentiment).
2. **Pianificazione storyboard**
   - Genera uno storyboard e definisce i prompt video a partire dai metadati musicali.
3. **Generazione prompt ottimizzati**
   - Affina i prompt per Sora basandosi su contesto, stile e durata desiderata delle clip.
4. **Produzione video con Sora**
   - Invia i prompt tramite API o automazione web, recupera le clip generate e ne traccia la qualità.
5. **Post-produzione automatica**
   - Monta le clip su timeline, sincronizza con l'audio, aggiunge titoli, transizioni e sottotitoli.
6. **Valutazione e iterazione**
   - Analizza la coerenza finale audio/video, decide se rigenerare porzioni del video e applica miglioramenti.

## Flusso operativo suggerito
1. **Bootstrap**
   - L'agente si avvia su Railway, legge le variabili d'ambiente e verifica la validità delle credenziali.
2. **Creazione progetto**
   - Crea una cartella di lavoro temporanea, inizializza i log e imposta la pipeline di messaggistica (es. Slack, webhook).
3. **Esecuzione autonoma**
   - Coordina i sotto-agenti per completare ogni fase, salvando gli stati intermedi nello storage configurato.
4. **Consegna output**
   - Pubblica il video finale nello storage o su una piattaforma configurata, inviando notifiche sul canale scelto.
5. **Cleanup e report**
   - Rimuove asset temporanei, produce un report sull'esecuzione e ruota le credenziali se necessario.

## Deployment su Railway
1. Fork/clone del repository e collegamento a Railway tramite dashboard o CLI.
2. Configurazione del servizio con Dockerfile o buildpack Python/Node (a seconda dell'implementazione dell'agente).
3. Inserimento delle variabili d'ambiente nella sezione *Variables* del progetto Railway.
4. Impostazione di *Triggers* o *Cron Jobs* per avviare automaticamente l'agente su nuovi progetti.
5. Monitoraggio dei log in tempo reale dalla dashboard Railway o tramite `railway logs`.

## Monitoraggio e manutenzione
- Abilitare alert sulle quote API di Sora e Suno.
- Integrare metriche (prometheus/grafana) per tempi di generazione e tassi di successo.
- Pianificare la rotazione periodica delle chiavi e delle credenziali.
- Documentare i processi di emergenza in caso di fallimenti ripetuti nelle generazioni.

## Risorse aggiuntive
- [Documentazione Sora](https://openai.com/sora) (verificare disponibilità API).
- [Documentazione Suno](https://www.suno.ai/).
- [Railway.app](https://railway.app/) per la configurazione del servizio.

Per ulteriori dettagli sull'architettura, consultare anche [`docs/sora_agent.md`](docs/sora_agent.md).
