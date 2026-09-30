# Minimal HDFS Deployment

Deployment minimale di un cluster Apache Hadoop HDFS con ambiente Jupyter per interagire tramite Python.

## Servizi Disponibili

- **Apache Hadoop HDFS (v3.5.0)**:
  - `namenode`: Porta Web UI HDFS `9870`, Porta RPC `8020`
  - `datanode`: 3 repliche
- **Jupyter Notebook**:
  - Servizio `jupyter`: Porta `8888` (accessibile su [http://localhost:8888](http://localhost:8888))
  - Volume `./datasets` montato su `/app/datasets`
  - Volume `./notebooks` montato su `/app/notebooks`

## Gestione del cluster

### Avvio
```bash
docker compose up -d
```

### Arresto dei container mantenendo i dati su HDFS
Fersma i container mantenendone lo stato. Alla successiva esecuzione di `docker compose start` o `docker compose up -d`, i file salvati su HDFS saranno ancora presenti:
```bash
docker compose stop
```

### Rimozione dei container (Reset di HDFS)
Rimuove i container e la rete Docker. **I dati caricati all'interno di HDFS verranno persi** e il cluster si riavvierà pulito al successivo `up`:
```bash
docker compose down
```
*(I file locali nelle cartelle `./datasets` e `./notebooks` rimangono sempre preservati).*

## Notebook di Esempio

All'interno di `./notebooks/` è presente il notebook [`hdfs_interaction.ipynb`](file:///Users/giovannifarina/Minimal-HDFS-Deployment/notebooks/hdfs_interaction.ipynb) che esegue:
1. Connessione a HDFS via WebHDFS tramite il client Python `hdfs`.
2. Caricamento del dataset locale (`/app/datasets/Solar_Energy_Production.csv`) su HDFS in `/datasets/Solar_Energy_Production.csv`.
3. Verifica della presenza del file su HDFS e visualizzazione dei relativi metadati.
4. Download del dataset da HDFS salvandolo con un nome diverso (`/app/notebooks/Solar_Energy_Production_renamed.csv`).
