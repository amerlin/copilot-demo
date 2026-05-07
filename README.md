# Copilot-demo - Full Stack: .NET 10 + React.
Copilot demo for DotNet Liguria 

```
mkdir copilot-dotnet-demo && cd copilot-dotnet-demo
mkdir -p .github/skills
touch .github/copilot-instructions.md
touch .github/skills/security-audit.md
```

#### .github/copilot-instructions.md

```
# Linee Guida Progetto .NET 10
- Usa **Minimal APIs** invece dei Controller classici.
- Usa **C# 14** features (es. Primary Constructors ovunque possibile).
- Il codice deve essere documentato con commenti XML in **italiano**.
- Applica il pattern **Result** (niente eccezioni per la logica di business).
- Per il Frontend: **React con TypeScript** e Tailwind CSS.
- Usa `System.Text.Json` per la serializzazione.
```

#### .github/skills/security-audit.md

```
# Skill: Security Audit .NET
Quando invocata, analizza il codice .NET per:
1. Hardcoded Connection Strings o API Keys.
2. Mancanza di validazione input (usando FluentValidation o DataAnnotations).
3. Uso di metodi obsoleti.
Proponi il fix e spiega la vulnerabilità in italiano.
```

# Steps

## Step 1: Lo Scaffolding (Agent Mode)
Apri Copilot Edits (Ctrl+Shift+I) e scrivi:

"@workspace genera una soluzione .NET 10 chiamata 'TaskMaster'. Crea un progetto Web API (Minimal API) e un progetto React (Vite) nella stessa cartella. L'app deve gestire una lista di Task (id, titolo, completato). Segui le istruzioni in .github/copilot-instructions.md."

Cosa osservare: Copilot genererà la struttura della soluzione (.sln), il progetto .csproj con il target framework net10.0 e il frontend.

## Step 2: Implementazione con C# 14 (Chat)
Apri il file Program.cs del backend e chiedi nella chat:

"Aggiungi un endpoint POST per creare una nota. Usa un Primary Constructor per il servizio di gestione."
"Aggungimi Scalar per la visualizzazione degli endpoint"

Cosa osservare: Vedrai come Copilot usa la sintassi compatta dei Primary Constructors e le Minimal APIs, documentando tutto in italiano come richiesto dalle istruzioni.

## Step 3: Test della Skill di Sicurezza
Modifica il file appsettings.json o Program.cs inserendo una stringa di connessione finta in chiaro:
var connectionString = "Server=myServer;Database=myDb;User Id=admin;Password=Password123;";

Nella chat digita:

"@workspace /security-audit"

Cosa osservare: Copilot identificherà la stringa di connessione esposta e ti suggerirà di usare User Secrets o le variabili di ambiente di .NET.

## Step 4: Integrazione Frontend-Backend
Seleziona il codice del componente React e chiedi:

"Genera il codice TypeScript per chiamare l'API di creazione note che abbiamo appena scritto nel backend. Usa Fetch API."

Cosa osservare: Copilot leggerà il contesto del backend (anche se è un linguaggio diverso) per mappare correttamente gli oggetti JSON tra C# e TypeScript.

Comandi Bonus per .NET 10:
@terminal /explain: Usalo se il comando dotnet build fallisce per mostrare come l'AI risolve gli errori di compilazione.

#selection: Seleziona un pezzo di codice C# e scrivi "Converti in una espressione switch moderna" per mostrare la conoscenza del linguaggio.

## Step 5: Creiamo l'Agent "Tech Lead"

Crea il file di configurazione nella cartella .github/copilot-agents.yaml

```
name: Tech Lead
description: Agente specializzato in architettura software, best practices .NET 10 e pattern architetturali.
instructions: |
  Sei un Senior Tech Lead. Il tuo obiettivo è garantire che il codice rispetti i principi SOLID, la pulizia e le performance.
  - Rispondi sempre in italiano.
  - Quando ti viene chiesto di revisionare il codice, suggerisci miglioramenti sulla gestione degli errori e sull'iniezione delle dipendenze.
  - Non scrivere codice direttamente, ma spiega l'approccio architetturale ideale.
skills:
  - name: review-code
    description: Analizza la complessità e le dipendenze del codice selezionato.
```

Digita nella chat standard:

"@workspace carica il nuovo agente @techlead"

Apri il file Program.cs del backend e scrivi all'agente:

"@techlead /review-code ho creato questo endpoint per salvare un task. Cosa ne pensi dal punto di vista architetturale?"

