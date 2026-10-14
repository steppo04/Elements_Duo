# Elements Duo

  Elements Duo è un puzzle-platform 2D sviluppato in Java.
  Il gioco riprende l'idea cooperativa di due personaggi, Fireboy e Watergirl, controllati dalla stessa tastiera: entrambi devono
  attraversare il livello, evitare ostacoli e nemici, interagire con l'ambiente e raggiungere la propria uscita.

  Il progetto è stato sviluppato ponendo attenzione alla separazione delle responsabilità, alla modellazione a oggetti e
  all'utilizzo di design pattern per mantenere il codice modulare, estendibile e testabile.

  ## Requisiti

  - Java 17 o superiore
  - Gradle Wrapper incluso nel repository

  ## Avvio

  Per avviare il gioco da sorgente:

  ```bash
  ./gradlew run
  ```

  Per eseguire il JAR già presente nel repository:

  ```bash
  java -jar OOP24-elements-duo-all.jar
  ```

  Per generare un nuovo JAR eseguibile:

  ```bash
  ./gradlew shadowJar
  ```

  Il JAR viene prodotto in:

  ```text
  build/libs/OOP24-elements-duo-all.jar
  ```

  ## Comandi

  Fireboy:

  - `A`: movimento a sinistra
  - `D`: movimento a destra
  - `W`: salto

  Watergirl:

  - `Freccia sinistra`: movimento a sinistra
  - `Freccia destra`: movimento a destra
  - `Freccia su`: salto

  ## Build e test

  Compilazione:

  ```bash
  ./gradlew assemble
  ```

  Esecuzione dei test:

  ```bash
  ./gradlew test
  ```

  Controlli completi:

  ```bash
  ./gradlew check
  ```

  Il progetto usa JUnit 5 per i test e include controlli di qualità tramite Gradle.

  ## Struttura del progetto

  ```text
  src/main/java/it/unibo/elementsduo
  ├── application      Entry point dell'applicazione
  ├── controller       Coordinamento del flusso di gioco, input e navigazione
  ├── datasave         Salvataggio e caricamento della progressione
  ├── model            Logica di dominio del gioco
  ├── resources        Classi di supporto
  └── view             Interfaccia grafica Swing
  ```

  Le risorse dei livelli sono contenute in:

  ```text
  src/main/resources/levels
  ```

  I test sono contenuti in:

  ```text
  src/test/java/it/unibo/elementsduo
  ```

  ## Architettura

  Il progetto è organizzato seguendo una separazione ispirata al pattern MVC.

  Il package `model` contiene la logica principale del gioco: giocatori, nemici, ostacoli, collisioni, missioni, stato della
  partita e progressione.
  Il package `view` contiene i pannelli Swing responsabili della rappresentazione grafica.
  Il package `controller` coordina l'interazione tra modello e vista, gestendo il game loop, l'input da tastiera, la navigazione
  tra schermate e l'avanzamento della partita.

  Questa separazione permette di mantenere il comportamento del gioco indipendente dalla sua rappresentazione grafica e rende più
  semplice testare le componenti principali.

  ## Design pattern e scelte progettuali

  Durante lo sviluppo sono stati adottati diversi pattern e principi progettuali per contenere l'accoppiamento tra le classi e
  rendere il codice più estendibile.

  Tra le scelte principali:

  - **MVC**, usato come struttura generale dell'applicazione.
  - **Factory Method / Factory**, usati per centralizzare la creazione di giocatori, nemici, ostacoli ed entità di gioco.
  - **Builder**, usato nella costruzione delle risposte alle interazioni, raccogliendo più azioni prima della loro esecuzione.
  - **Command**, usato per modellare azioni prodotte dalle interazioni, come la spinta delle casse o la risoluzione di effetti
  fisici e logici.
  - **Observer / Event Listener**, usato per notificare eventi di gioco come raccolta gemme, morte dei nemici, raggiungimento
  delle uscite e attivazione di elementi interattivi.
  - **Composite**, usato nella gestione delle missioni, dove più obiettivi possono essere aggregati in una missione principale.
  - **Template Method**, usato per definire algoritmi comuni lasciando alle sottoclassi la specializzazione di alcuni passaggi, ad
  esempio nella gestione degli obiettivi e dei nemici.
  - **Strategy**, usato nella separazione di alcuni comportamenti specifici, come input, fisica e correzione delle collisioni dei
  personaggi.
  - **Handler chain / responsabilità distribuita**, usata nella gestione delle collisioni: ogni handler si occupa di una specifica
  famiglia di interazioni.


  ## Gameplay

  Ogni livello contiene ostacoli, nemici, piattaforme, leve, pulsanti, gemme e zone pericolose.
  Fireboy e Watergirl hanno comportamenti simili ma non identici: alcune superfici sono sicure per un personaggio e pericolose per
  l'altro.

  La partita termina con successo quando entrambi i personaggi raggiungono la rispettiva uscita.
  Oltre al completamento del livello, è presente una missione secondaria che richiede di raccogliere tutte le gemme, eliminare
  tutti i nemici e finire entro un tempo limite.

  ## Progressione e salvataggio

  La progressione viene salvata su file JSON in:

  ```text
  save/savegame.json
  ```

  Per ogni livello vengono mantenuti:

  - miglior tempo di completamento;
  - stato della missione secondaria;
  - livello corrente.

  La serializzazione dei salvataggi è gestita tramite Jackson.

  ## Livelli

  I livelli sono caricati da file testuali presenti nelle risorse del progetto.
  Il caricamento converte i simboli della mappa in entità di gioco tramite un assembler dedicato.

  Prima dell'avvio, ogni livello viene validato per verificare la correttezza della struttura, la presenza degli elementi
  necessari e la raggiungibilità degli obiettivi principali.

