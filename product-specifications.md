# Drone di sorveglianza indoor – Specifiche di prodotto

*Versione 1.4 – 30 ottobre 2026*

## Sintesi

Il prodotto è un unico micro-drone indoor autonomo, con base di ricarica, venduto una tantum in quattro configurazioni: casa, ufficio piccolo, magazzino piccolo e locali tecnici. Oggi in Italia non esiste un prodotto equivalente acquistabile: le soluzioni esistenti sono ferme, solo USA o riservate a polizia e industria.

Decisioni già prese:

- **Un solo hardware**, quattro configurazioni di base e software.
- **Solo osservazione**: niente audio, sirena o messaggi vocali.
- **Vendita una tantum**: tutte le funzioni girano in locale, nessun cloud obbligatorio né abbonamento.
- **Priorità di mercato**: 1) casa, 2) ufficio, 3) magazzino; i locali tecnici dopo il lancio.
- **Prototipazione** su ModalAI Starling 2; il prodotto finale avrà hardware proprio per stare nel costo obiettivo.

## Analisi di mercato

Nessun drone di sorveglianza indoor pronto all’uso è acquistabile in Italia: c’è spazio per un prodotto consumer e PMI. Il riferimento di prezzo consumer è il Ring Always Home Cam, a circa 250 \$.

**Prodotti esistenti**

| Prodotto                                                                                                              | Prezzo           | Stato                      | Note                                                                                 |
|-----------------------------------------------------------------------------------------------------------------------|------------------|----------------------------|--------------------------------------------------------------------------------------|
| [Ring Always Home Cam](https://www.xda-developers.com/how-to-buy-ring-always-home-cam/)                               | 249,99 \$        | Solo USA, su invito        | 5 min di volo, richiede Ring Alarm ([fonte](https://www.gadgetreview.com/?p=416642)) |
| [Indoor Robotics Tando](https://startupim.com/company/indoor-robotics)                                                | n.d.             | Azienda chiusa (feb. 2025) | Drone con base a soffitto, per uffici e data center                                  |
| [BRINC LEMUR 2](https://dronedj.com/2023/12/27/brinc-to-supply-lemur-2-drones-in-700k-schenectady-police-force-deal/) | Su richiesta     | Solo forze dell’ordine     | Contratto Schenectady di circa 700.000 \$ (più droni e servizi)                      |
| [Flyability Elios 3](https://robotomated.com/explore/drone/flyability-elios-3)                                        | 25.000–55.000 \$ | Acquistabile               | Ispezione industriale, pilotato a mano                                               |

**Piattaforme di sviluppo**

| Piattaforma                                                                                           | Prezzo                                                                                                                            | Peso        | Volo         | Adatta a                                                 |
|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|-------------|--------------|----------------------------------------------------------|
| [ModalAI Starling 2](https://www.modalai.com/collections/drones)                                      | da 2.949,99 \$                                                                                                                    | 280 g       | oltre 40 min | Prototipo multi-stanza: VIO, ToF, AI a bordo, PX4, ROS 2 |
| [Crazyflie 2.1 Brushless](https://www.robotshop.com/products/bitcraze-crazyflie-21-brushless)         | 480 \$ (bundle ricarica [1.239,53 \$](https://www.robotshop.com/products/bitcraze-infinite-flight-bundle-crazyflie-21-brushless)) | 27 g        | circa 10 min | Demo in una sola stanza                                  |
| [DJI Neo 2](https://www.yourlifeupdated.net/droni/dji-neo-2-drone-da-151g-con-controllo-gesti-da-239) | 239 €                                                                                                                             | 151 g       | breve        | Solo prove manuali, ecosistema chiuso                    |
| [Holybro X500 v2](https://shop.holybro.com/x500-kit_p1180.html)                                       | 507,99 \$                                                                                                                         | oltre 600 g | n.d.         | Non adatto: troppo grande per interni                    |

I prezzi in dollari escludono spedizione, dazi e IVA.

## Requisiti comuni

Tutte le configurazioni condividono lo stesso drone e queste regole di base.

**Funzionali**

1.  Pattuglia programmata su percorsi definiti dall’utente, con punti di osservazione configurabili.
2.  Volo su evento: decollo entro 30 s quando scatta un sensore o su richiesta dall’app.
3.  Rilevamento persone a bordo; classificazione di animali e residenti per ridurre i falsi allarmi.
4.  Ricarica autonoma: rientro e aggancio alla base senza intervento.
5.  Notifiche con foto e clip sul telefono; video live su richiesta.
6.  Zone vietate e zone mascherate configurabili sulla mappa.
7.  Solo osservazione: nessun audio, sirena o messaggio vocale.

**Non funzionali**

- Dimensioni sotto i 25 cm, eliche intubate o in gabbia.
- Peso sotto i 300 g.
- Navigazione senza GPS e senza magnetometro: VIO + sensore ToF + illuminatore IR.
- Funzionamento al buio totale.
- Elaborazione video in locale, dati cifrati, nessun cloud obbligatorio.
- Camera oscurata quando il drone è in base; indicatore luminoso quando registra.
- Failsafe: con batteria bassa o posizione persa atterra subito in un’area sicura.
- Base con controllo termico della batteria e celle certificate.
- Mappatura iniziale guidata dall’app in meno di 15 minuti.
- Obiettivo di rumore da fissare (ipotesi: sotto 55 dB a 1 m).

**Economici**

- Prezzo al pubblico versione casa: 500–800 €.
- Costo dei componenti in produzione: 150–250 €.
- Vendita una tantum, senza costi ricorrenti.

## Caso 1 – Casa (priorità 1)

Appartamento su un piano, fino a 100 m², 4–6 stanze; il drone copre tutta la casa, di giorno e di notte.

**Modalità**

- **Casa vuota**: pattuglia completa programmata + volo su allarme.
- **Notte con persone**: decollo solo su evento o richiesta; camere da letto occupate escluse come zone vietate.

**Requisiti specifici**

- Copertura multi-stanza: navigazione VIO, porte interne aperte di circa 80 cm.
- Durata di un giro completo: almeno 5 minuti di volo.
- Visione al buio: illuminatore IR a bordo; opzione di accendere le luci via Home Assistant.
- Sicurezza con persone: velocità sotto 1 m/s quando rileva persone, distanza minima 1 m, nessun sorvolo.
- Animali domestici: non generano allarmi, il drone li evita.
- Riconoscimento residenti, bambini e animali per evitare falsi allarmi notturni.
- Modalità silenziosa notturna.
- Controlli: intrusi, porte e finestre aperte, fornelli e luci; fumo e acqua tramite sensori fissi esistenti.
- Integrazione: Home Assistant + notifiche Telegram o push.

## Caso 2 – Ufficio piccolo (priorità 2)

Ufficio fino a 300 m², un solo drone che gira tutto l’ufficio con le porte del percorso lasciate aperte, solo fuori orario.

**Requisiti specifici**

- Operatività fuori orario (notte e weekend); se qualcuno è presente si attiva la modalità notte con persone del caso casa.
- Autonomia di 8–10 minuti per un giro completo.
- Procedura di chiusura serale che ricorda di lasciare aperte le porte del percorso; controllo pre-volo che segnala le porte trovate chiuse.
- Controlli: intrusi, porte e finestre, luci e apparecchi accesi; fumo e acqua tramite sensori fissi.
- Gestione multi-utente con permessi (titolare, responsabile) e registro degli accessi alle registrazioni.
- Privacy aziendale: zone mascherate e sfocatura automatica di schermi e documenti.
- Notifiche al titolare sul telefono.
- Integrazione con istituti di vigilanza: rimandata alla versione 2.

## Caso 3 – Magazzino piccolo (priorità 3)

Capannone fino a 1.000 m², solo sorveglianza intrusi, solo a capannone chiuso; marcatori visivi installati nell’ambiente.

**Requisiti specifici**

- Operatività solo a capannone chiuso (notte e weekend): niente volo tra persone e muletti.
- Solo sorveglianza intrusi: niente termocamera, inventario o ispezione strutture.
- Localizzazione con marcatori visivi (tag adesivi su scaffalature e pilastri) posati in fase di installazione, oltre a VIO.
- Illuminatore IR più potente per spazi ampi, soffitti di 6–10 m e buio totale.
- Percorso tipico di circa 300 m (perimetro + corsie), 5–7 minuti di volo.
- Tolleranza a polvere e correnti d’aria da portoni e ventilazione.
- Base riscaldata per mantenere la batteria in temperatura d’inverno.
- Stesso drone delle altre versioni; oltre 1.000 m² servirebbe una flotta o un prodotto separato.

## Caso 4 – Locali tecnici (dopo il lancio)

Cabine elettriche, centrali termiche e sottostazioni, spesso non presidiate e remote; il drone verifica un allarme senza mandare subito qualcuno sul posto.

**Requisiti specifici**

- Solo sorveglianza intrusi (furti di rame, vandalismo).
- Navigazione senza magnetometro, robusta alle interferenze elettromagnetiche di trasformatori e quadri.
- Distanze minime obbligatorie dalle parti in tensione, zone vietate, atterraggio d’emergenza solo in aree sicure predefinite.
- Zone ATEX (centrali termiche a gas) escluse dal prodotto.
- Base con connessione propria (cavo o 4G); immagini salvate a bordo e inviate al rientro.
- Qualifica di compatibilità elettromagnetica più spinta rispetto alle altre versioni.
- Clienti tipici: utility e gestori di reti, con qualifica dei fornitori lunga; partire con un’utility come partner pilota.

## Quadro comparativo

Le quattro configurazioni differiscono per base, software e illuminazione, non per il drone.

|               | Casa                         | Ufficio piccolo                     | Magazzino piccolo            | Locali tecnici                 |
|---------------|------------------------------|-------------------------------------|------------------------------|--------------------------------|
| Priorità      | 1                            | 2                                   | 3                            | Dopo il lancio                 |
| Area          | fino a 100 m²                | fino a 300 m²                       | fino a 1.000 m²              | Locali piccoli                 |
| Quando vola   | Casa vuota + notte su evento | Fuori orario                        | Capannone chiuso             | Sempre                         |
| Volo per giro | almeno 5 min                 | 8–10 min                            | 5–7 min                      | Breve                          |
| Navigazione   | VIO + ToF + IR               | VIO + ToF + IR                      | VIO + marcatori + IR potente | VIO + marcatori, senza bussola |
| Base          | Standard                     | Standard                            | Riscaldata                   | Con 4G o cavo                  |
| Software      | Zone vietate, modalità notte | Multi-utente, mascheramento schermi | Spazi ampi, marcatori        | Regole di sicurezza elettrica  |

## Normativa, privacy e certificazioni

Il punto più incerto è se le classi europee dei droni si applicano a un prodotto solo indoor: va chiarito con un consulente prima di progettare l’hardware finale.

- **Marcatura CE**: Direttiva Radio (RED), compatibilità elettromagnetica, sicurezza delle batterie e del caricatore.
- **Regolamenti UE sui droni** (2019/945 e 2019/947): il volo in ambienti chiusi è in genere escluso, ma l’applicabilità al prodotto va verificata.
- **GDPR**: privacy by design, elaborazione in locale, cifratura, informativa per ospiti e collaboratori domestici, registro degli accessi nelle versioni professionali.
- **Statuto dei Lavoratori, art. 4**: il drone in un luogo di lavoro durante l’orario richiede accordo sindacale o autorizzazione dell’Ispettorato; per questo ufficio e magazzino operano fuori orario.
- **Locali tecnici**: norme di sicurezza elettrica (CEI) e zone ATEX escluse.
- **Responsabilità**: il drone non sostituisce antifurto o antincendio certificati; va scritto chiaramente nella documentazione di prodotto.

Nota: questa sezione è un orientamento, non una consulenza legale.

## Percorso di sviluppo

Si prototipa su ModalAI Starling 2 (da 2.949,99 \$, budget complessivo circa 3.500–4.000 €) e si passa all’hardware proprio solo dopo aver dimostrato navigazione e sicurezza notturna.

| Fase               | Contenuto                                                                          | Gate per passare alla fase successiva |
|--------------------|------------------------------------------------------------------------------------|---------------------------------------|
| 1\. Prototipo      | Starling 2, navigazione in casa, docking e ricarica, missioni da Home Assistant    | Navigazione stabile                   |
| 2\. Prove notturne | Test con persone e animali, sicurezza e rumore, riconoscimento residenti e animali | Sicuro di notte                       |
| 3\. Hardware       | Drone e base propri, costo componenti 150–250 €, consulenza normativa              | Costo sotto 250 €                     |
| 4\. Lancio         | Certificazione CE, utenti pilota, prima la casa, poi ufficio                       | –                                     |

Ogni fase si chiude con un gate: la successiva parte solo se il criterio è raggiunto.

### Fase 0 opzionale – Crazyflie

Prima della fase 1 si può sviluppare il software che non dipende dal drone su un Crazyflie 2.1 Brushless (budget circa 1.300–1.600 €). Serve a lavorare presto e a basso rischio, non a validare la navigazione multi-stanza al buio.

|                | Crazyflie (fase 0)                                                | Starling 2 (fase 1)                    |
|----------------|-------------------------------------------------------------------|----------------------------------------|
| Budget         | circa 1.300–1.600 €                                               | circa 3.500–4.000 €                    |
| Peso           | 27 g, innocuo negli urti                                          | 280 g                                  |
| Volo           | circa 10 min                                                      | oltre 40 min                           |
| Localizzazione | Lighthouse (una stanza) o ancore radio (più stanze, meno precise) | VIO a bordo, senza infrastruttura      |
| Visione        | AI-deck a bassa risoluzione, senza IR                             | Più telecamere, ToF, termica opzionale |
| Ricarica       | Base di ricarica già pronta                                       | Base da progettare                     |

Cosa si sviluppa in fase 0:

- Gestore delle missioni: pattuglia, volo su evento, rientro.
- Integrazione con Home Assistant e notifiche Telegram o push.
- App con mappa, zone vietate e modalità notte.
- Logica di failsafe.

Il software va scritto su ROS 2 con uno strato separato per il drone: passando allo Starling 2 si cambia solo quello strato. Gate della fase 0: missioni da Home Assistant con rientro e ricarica funzionanti.

### Budget minimo per iniziare con il Crazyflie

Si parte da 0 € in simulazione (CrazySim e Crazyswarm2); il minimo con un drone vero che vola in autonomia è di circa 350–400 € in Europa.

| Componente                                                                        | Prezzo (USA)                                                                                                            | A cosa serve                                                   |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| [Crazyflie 2.1+](https://www.robotshop.com/products/bitcraze-crazyflie-21)        | 240 \$                                                                                                                  | Il drone (motori a spazzole)                                   |
| [Flow deck v2](https://robotshop.com/products/bitcraze-crazyflie-2x-flow-deck-v2) | circa 63 \$ (56,54 € IVA incl. su [RobotShop EU](https://eu.robotshop.com/products/bitcraze-crazyflie-2x-flow-deck-v2)) | Posizione stimata guardando il pavimento, senza infrastruttura |
| [Crazyradio 2.0](https://store.bitcraze.io/products/crazyradio-2-0)               | 43 \$                                                                                                                   | Collegamento al PC per Python e ROS 2                          |
| **Totale**                                                                        | **circa 346 \$**                                                                                                        | Con spedizione e IVA circa 350–400 €, da verificare all’ordine |

RobotShop vende anche un [AI Bundle](https://ca.robotshop.com/products/bitcraze-ai-bundle-crazyflie-21) con esattamente questi tre componenti: confrontarne il prezzo.

Cosa si può fare: decollo, brevi percorsi e atterraggio da codice; integrazione con Home Assistant, notifiche e interfaccia LLM; prove di sicurezza in casa con un drone sotto i 30 g.

Limiti:

- Niente ricarica automatica: la base di ricarica Bitcraze è solo per la versione Brushless; le batterie si cambiano a mano (ricambi da circa £6).
- Niente volo al buio: il Flow deck richiede luce e un pavimento con texture, quindi la modalità notte non si può testare.
- Solo giri brevi in una stanza: la posizione deriva nel tempo.

Percorso a gradini:

1.  Simulazione: 0 €.
2.  Kit minimo: circa 350–400 €.
3.  Ricarica automatica (Crazyflie Brushless con base, deck Lighthouse e una sola stazione) quando il software è maturo, invece del pacchetto completo da circa 1.240 \$.

## Interfaccia LLM (opzionale)

L’utente può dare comandi a parole (“controlla la cucina”), ma l’LLM sceglie solo tra funzioni predefinite e ogni sua proposta passa da un validatore deterministico prima del volo. È una funzione di comodità: allarmi, rientro e failsafe funzionano anche con l’LLM spento.

**Architettura: l’LLM propone, il validatore decide**

| Livello                        | Ruolo                                                                                                                                        |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| 1\. Utente                     | Comando a parole da Telegram, app o Home Assistant                                                                                           |
| 2\. LLM locale (pianificatore) | Sceglie una funzione dall’elenco consentito: vai_a, scatta_foto, pattuglia, torna_alla_base                                                  |
| 3\. Validatore di sicurezza    | Controlla formato, zone vietate, batteria, modalità notte, confini della mappa. Se rifiuta, rimanda all’LLM il motivo per un nuovo tentativo |
| 4\. Gestore missioni           | Esegue solo la funzione validata                                                                                                             |
| 5\. Volo e failsafe            | Crazyswarm2 o PX4 sul drone; funziona anche se l’LLM è spento                                                                                |
| Notifica                       | Un LLM multimodale descrive la foto e risponde all’utente                                                                                    |

Una proposta rifiutata torna all’LLM con il motivo e viene riprovata; se l’LLM non risponde, il drone resta fermo o torna alla base.

**Principi di progetto**

- Funzioni, non coordinate: l’LLM chiama `vai_a("cucina")`, mai coordinate o comandi di volo.
- Risposta sempre in formato strutturato e verificata automaticamente.
- Autocorrezione limitata a pochi tentativi, poi rinuncia con notifica.
- Modello piccolo in locale (mini-PC o server di Home Assistant), coerente con la vendita una tantum senza cloud; cloud solo come opzione dell’utente.
- Prova a costo zero in simulazione con CrazySim e Crazyswarm2.

**Riferimenti**

| Progetto                                                               | Cosa dimostra                                                                                     | Limite per questo progetto                                                 |
|------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| [SwarmGPT](https://arxiv.org/abs/2412.08428) (TU Munich, RA-L 2025)    | Con funzioni predefinite e autocorrezione il successo sale all’85–100%; fino a 20 Crazyflie reali | Posizione da motion capture Vicon, traiettorie precalcolate                |
| [SkySim](https://arxiv.org/abs/2602.01226v1) (Heriot-Watt Dubai, 2026) | Forme corrette al 100% con 3–30 droni simulati, controllo di sicurezza a 20 Hz                    | Solo simulazione; risposte cloud da 34–50 s, oltre 100 s nei casi peggiori |

## Validazione di business

Prima di investire in hardware si verificano tre cose: che il problema esista davvero, che i clienti paghino il prezzo necessario e che accettino un drone che vola in casa o in ufficio. L’ultimo è il rischio meno ovvio: il Ring Always Home Cam è stato venduto solo su invito negli USA e non è mai arrivato su larga scala.

| Attività                                                                                                  | Quando                            | Costo indicativo                   | Cosa si verifica                                                        |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------|------------------------------------|-------------------------------------------------------------------------|
| 1\. Interviste a 20–30 persone per segmento (casa, ufficio, magazzino)                                    | Subito, 2–4 settimane             | Quasi 0 €                          | Quale segmento sente il problema più forte; reazioni a privacy e rumore |
| 2\. Confronto con le alternative reali (telecamere fisse, antifurti, vigilanza)                           | Subito, 1 settimana               | 0 €                                | Se il drone conviene rispetto a ciò che il cliente usa oggi             |
| 3\. Landing page con lista d’attesa o prenotazione simbolica                                              | Dopo le interviste, 2–4 settimane | 100–500 € di pubblicità            | Interesse reale misurato, separato per segmento                         |
| 4\. Video dimostrativo con il kit minimo Crazyflie                                                        | In parallelo alla fase 0          | Incluso nel kit da circa 350–400 € | Materiale per landing page, interviste, partner e investitori           |
| 5\. Incontri con i canali (installatori, vigilanza, integratori domotici, comunità Home Assistant)        | Durante la fase 0                 | Quasi 0 €                          | Se lo venderebbero e a che margine                                      |
| 6\. Verifiche con consulenti: costo di produzione, normativa CE e droni UE, brevetti (libertà di operare) | Prima dell’hardware proprio       | Da preventivare                    | Fattibilità economica e rischi legali                                   |
| 7\. Piloti con 3–5 clienti reali per 1–3 mesi                                                             | Dopo la fase 1 (Starling 2)       | Prototipi a prezzo scontato        | Falsi allarmi, guasti, utilità reale, intenzione d’acquisto             |
| 8\. Prevendita o crowdfunding; verifica dei bandi per startup innovative (es. Invitalia)                  | Prima della produzione            | Costi della campagna               | Validazione e finanziamento insieme                                     |

**Criteri per andare avanti**: si fissano prima di raccogliere i dati, per non leggere i risultati in modo favorevole. Esempi da adattare:

- Interviste: almeno un segmento in cui la maggioranza descrive il problema come importante.
- Landing page: tasso di iscrizione chiaramente superiore alla media del settore.
- Canali: almeno 2–3 installatori o vigilanze interessati a provarlo.
- Piloti: la maggioranza dei clienti pilota lo comprerebbe al prezzo previsto.

## Rischi e punti aperti

| Rischio                          | Impatto                              | Mitigazione                                                       |
|----------------------------------|--------------------------------------|-------------------------------------------------------------------|
| VIO poco affidabile al buio      | Il drone perde la posizione di notte | IR a bordo, ToF, marcatori, luci via Home Assistant               |
| Costo componenti sopra 250 €     | Prezzo fuori mercato consumer        | Hardware proprio dopo il prototipo, stima del costo già in fase 1 |
| Autonomia di volo insufficiente  | Giro incompleto in ufficio           | Obiettivo 8–10 min, rientro intermedio alla base                  |
| Batteria LiPo in carica di notte | Rischio incendio e responsabilità    | Base con controllo termico, celle certificate                     |
| Incertezza normativa UE          | Costi e tempi di certificazione      | Consulenza prima del design hardware                              |
| Urti con persone o animali       | Sicurezza e reputazione              | Eliche intubate, peso sotto 300 g, velocità ridotta               |

**Punti aperti**

- [ ] Fissare l’obiettivo di rumore in dB.
- [ ] Verificare con un consulente l’applicabilità dei regolamenti UE sui droni a un prodotto solo indoor.
- [ ] Stimare il costo dei componenti del prodotto finale.
- [ ] Decidere i protocolli di integrazione con la vigilanza (versione 2).
- [ ] Definire il prezzo al pubblico delle versioni ufficio e magazzino.
