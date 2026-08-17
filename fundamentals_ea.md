# Fundamentals of Enterprise Architecture — Notizen

> **Quelle:** *Fundamentals of Enterprise Architecture* (Tanu McCabe, O'Reilly, 2024).
> Zusammenfassungen mit Anwendungshinweisen, entstanden im Kontext der Diskussion **Architecture-as-Code / EA-Governance im KI-Zeitalter**.

## Orientierung: zwei Kategorien, die man auseinanderhalten muss

- **Deliverables** = Dinge, die Architekten *produzieren*: Architecture Decision (ADR), Architecture Pattern, Capability Target Architecture, Application Target Architecture.
- **Informationstypen** = Dinge, die Entscheidungen *speisen*: Prinzipien, Standards, Best Practices, Frameworks, Diagramme, Metriken — und frühere Entscheidungen (das einzige Deliverable, das zugleich Informationstyp ist).

Verbindendes Leitmotiv des ganzen Buchs: Alles davon ist nur wirksam, wenn es **embedded & accessible** ist — am Ort der Entscheidung verfügbar, im Moment der Entscheidung.

**Roter Faden für unser Thema:** EA-Governance verschiebt sich von **Gatekeeping** (Gremien, Reviews vor Go-Live) zu **Guardrails** (maschinenlesbare Regeln, die die Pipeline automatisch durchsetzt). Fast jeder Abschnitt unten hat dazu eine Brücke.

---

## Warum EA: die Symptome schlechter Praxis

### Unkontrollierte Technologievielfalt (Symptom 1–2)

Drei versteckte Kosten:

1. **Talent skaliert nicht:** Jede Technologie braucht eigene Skills; Wissen ist nicht übertragbar, Teamwechsel werden schwer, Recruiting leidet.
2. **Security & Governance skalieren nicht:** Jede zusätzliche Technologie braucht eigene Absicherung, Scanning-Fähigkeiten und Aufsicht — die Kosten wachsen pro Technologiewahl mit.
3. **Nachträgliche Standardisierung ist teuer:** Wer Standards erst einführt, wenn der Wildwuchs schon da ist, zahlt mit Refactoring und Umbau.

Das 5.000-Pipelines-Beispiel bringt es auf den Punkt: Jedes Team baute seine eigene CI/CD-Lösung, das Unternehmen zahlte 5.000-mal Lernkurve und Wartung — und die nachträgliche Zentralisierung wurde zur Qual.

**In einem Satz:** Technologie-Standards müssen vor der Verbreitung stehen, nicht danach — sonst multiplizieren sich Kosten für Skills, Security und spätere Konsolidierung mit jeder Abweichung.

### Decision vs. Debt (Symptom 3)

- **Architekturentscheidung:** bewusstes Eingehen von Trade-offs und Risiken — legitim und unvermeidbar.
- **Technical Debt:** die künftigen Sanierungskosten, die aus *kurzsichtigen* Entscheidungen entstehen — also aus Entscheidungen, die nur das unmittelbare Bedürfnis bedienen.

Der Kernmechanismus: Ohne klare Architekturprinzipien und Entscheidungskriterien kippen Entscheidungen systematisch von „bewusster Trade-off" zu „Schnellschuss" — und die angehäufte Schuld erstickt genau das, was Architektur ermöglichen soll: Innovation und Anpassungsfähigkeit.

Das Beispiel: Lift-and-Shift in die Cloud unter Zeitdruck — Anwendungen, die für Rechenzentren gebaut wurden (kein horizontales Skalieren, hartcodierte IPs), unverändert migriert. Ergebnis: höhere Kosten, weiterhin keine Skalierung — in der Cloud, aber ohne deren Nutzen. Die Schuld wurde nicht getilgt, nur umgezogen.

**In einem Satz:** Schlechte EA erkennt man daran, dass Entscheidungen ohne Prinzipien getroffen werden — dann wird aus jedem bewussten Trade-off unbemerkt Schuld, und die Schuld frisst die Agilität, für die man die Abkürzung genommen hat.

---

## North Star: Vision und Mission der EA-Praxis

**Vision (das Was):** EA definiert den North Star — die strategische Richtung, die alle Technologie-Investitionen leitet und Business mit Technologie verbindet. Ziel ist ein gemeinsames Zielbild über Business, IT und Architektur hinweg. Beispiel: „Technologiestrategie definieren, die Organisationsgrenzen überwindet und unterschiedliche Ziele zu gemeinsamen Unternehmenszielen verbindet."

**Mission (das Wie):** Der North Star entsteht nicht per Dekret, sondern über vertrauensbasierte Entscheidungsfindung. Beispiel: „Enable great architecture decisions to deliver great solutions as one team" — mit drei bewusst gewählten Begriffen:

- **Enable:** Entscheidungen *ermöglichen*, nicht selbst alle treffen — EA als Befähiger, nicht als Flaschenhals.
- **Great:** heisst tragfähig und bedarfsgerecht, nicht perfekt — gute Entscheidungen bedeuten immer, Trade-offs zu erkennen und zu akzeptieren.
- **One team:** Die objektive Unternehmensperspektive ist das Alleinstellungsmerkmal von EA — entscheiden für das Unternehmen, nicht für einzelne Silos.

**In einem Satz:** EA gibt die Richtung vor (North Star) und erreicht sie nicht durch Kontrolle, sondern indem sie Teams befähigt, gute — nicht perfekte — Entscheidungen im Interesse des Gesamtunternehmens zu treffen.

*Brücke zu Architecture-as-Code:* „Enable, not decide" ist die Buchfassung dessen, was Guardrails-statt-Gatekeeping technisch umsetzt.

---

# Teil A — Deliverables

## Architecture Decision (ADR)

Eine dokumentierte Architekturentscheidung enthält vier Dinge: die Empfehlung selbst, den Kontext (Ziele, Constraints), die Alternativenanalyse und wer wann entschieden hat.

Sie erfüllt drei Funktionen:

- **Nachvollziehbarkeit:** Das Warum bleibt für die Nachwelt erhalten und verhindert, dass künftige Entscheidungen das Rad neu erfinden.
- **Alignment:** Kollaborationsmechanismus, der betroffene Stakeholder auf die Entscheidung eint.
- **Governance:** dokumentierter Rationale-Nachweis, besonders bei Abweichung von einem Standard.

**In einem Satz:** Die Entscheidung selbst ist nur die halbe Miete — der dokumentierte Weg dorthin ist das, was Alignment schafft, Rework verhindert und Abweichungen governbar macht.

> **Begriff „Rationale":** die Begründung — das dokumentierte *Warum*. Die Entscheidung veraltet, das Rationale bleibt nützlich: Es entscheidet später, ob man sicher revidieren kann („Grund gilt nicht mehr") oder besser nicht („Grund gilt noch immer").

### ADR-Template (Felder, aus Kap. 1)

| Feld | Zweck |
|---|---|
| **Identifier** | Eindeutige Namenskonvention zur Unterscheidung der Entscheidungen |
| **Description** | Menschenlesbare Beschreibung des gelösten Problems — für die Suche nach relevanten Entscheidungen |
| **Metadata** | Konsistente Taxonomie von Tags/Labels (org-spezifisch), für Transparenz und Auffindbarkeit quer durch die Organisation |
| **Stakeholders** | Explizit: wer ist accountable, responsible, consulted, informed (RACI) — stellt sicher, dass alle Relevanten einbezogen wurden |
| **Status** | Ist die Entscheidung in Kraft oder nicht — *entspricht der Position im Lifecycle aus Kap. 14 (Draft → In Progress → Review → Approved → Deprecated)* |
| **Date** | Datum der Entscheidung — ermöglicht Refresh |
| **Problem statement** | Explizites Problem, das die Entscheidung löst — vermeidet Ambiguität |
| **Alternatives** | Geprüfte Optionen mit Pros/Cons bzw. Trade-offs |
| **Rationale** | Warum diese Option gewann; bei späterer Änderung auch deren Begründung |
| **Implications** | Folgen der Umsetzung — inklusive explizit ausgewiesener Technical Debt und Risiken |

*Praxis:* De-facto-Standard für die Umsetzung ist das **MADR-Template** — simple Markdown-Files, nummeriert im Repo, alte Records nie löschen, sondern „superseded by" verlinken.

## Architecture Pattern

Ein Pattern dokumentiert eine bewährte Lösung für ein wiederkehrendes Problem, in zwei Formen: **Design** (technologieagnostisch) und **Implementierungsempfehlung** (technologiespezifisch), jeweils mit Hinweisen zu Einschränkungen und Überlegungen.

Zwei Anforderungen machen es wertvoll: Es ist ein **wiederverwendbarer Blueprint** (Standard-Weg, etwas zu tun) und es muss **durch Implementierung bewiesen** sein — nicht bloss Theorie.

Reifungsstufen der Kodifizierung: Pattern (Doku) → **Referenzarchitektur** (mehrere Patterns kombiniert als Blueprint für einen Anwendungstyp) → **Referenzimplementierung** (lauffähige Software dazu) → **Patterns as Code** (Templates, Snippets, Module, Libraries).

**In einem Satz:** Ein Pattern ist erst dann Best Practice, wenn es bewiesen ist — und erst dann wirksam, wenn es als Code konsumierbar ist statt als Dokument.

*Brücke:* Patterns as Code = „Golden Paths" — EA-Vorgaben werden vom Dokument zum direkt nutzbaren Baustein, den auch KI-Agenten als Kontext verwerten.

## Capability Target Architecture (typisch: Solution Architect)

Dokumentiert das gemeinsame Zielbild für eine Gruppe von Capabilities und deren Lösungen — eine **Architekturdomäne**. Strategisches Dokument: Es beantwortet auf Domänenebene, wo investiert vs. abgelöst wird, wo Emerging Tech gebraucht wird und wo Konvergenz/Konsolidierung möglich ist.

Typische Sichten, als Abstraktionstreppe:

- **Konzeptionell:** Domäne in fachliche Capabilities zerlegt
- **Logisch:** logische Bausteine, gemappt auf die Capabilities
- **Physisch:** konkrete Lösungen, die die logischen Funktionen liefern
- **Sequenzdiagramme:** Interaktionen in konkreten Workflows (z. B. PlantUML)
- **Architekturentscheidungen:** dasselbe ADR-Deliverable, nur auf Domänenebene

**In einem Satz:** Die Capability Target Architecture übersetzt den North Star in ein konkretes Zielbild pro Domäne — von fachlicher Capability über logische Struktur bis zur physischen Lösung, inklusive der Invest-/Deprecate-Entscheidungen dahinter.

## Application Target Architecture (eine Ebene tiefer, pro Anwendung)

Mindestens drei Sichten: **Context View** (Interaktion mit anderen Anwendungen), **Component View** (logische Zerlegung in Funktionen und deren Zusammenspiel), **Deployment View** (Deployment inkl. Resilienz- und Security-Eigenschaften). Dazu die Architekturentscheidungen auf Anwendungsebene.

Zweck: dokumentierter Zielzustand, der Entscheidungen über den Weg von Ist zu Soll ermöglicht — plus leicht verständliche Visualisierungen für Reviews und Risikoerkennung.

**In einem Satz:** Das Zielbild einer einzelnen Anwendung in drei Pflichtsichten — Kontext, Komponenten, Deployment — als Grundlage für den Ist-zu-Soll-Pfad und Risiko-Reviews.

*Brücke:* Das ist fast wörtlich das **C4-Modell** (Context → Container/Component → Deployment) — mit Structurizr/LikeC4 direkt als Code abbildbar: ein Modell, drei generierte Sichten, versioniert neben dem Anwendungscode. Mapping der Deliverable-Hierarchie: Capability Target Architecture = Domänen-Level (EAM-Repo), Application Target Architecture = C4-Modell im Git.

---

# Teil B — Informationstypen

## Architekturprinzipien

Ein Prinzip ist eine Regel oder Leitidee. Architekturprinzipien sorgen dafür, dass Architekturentscheidungen im ganzen Unternehmen *konsistent* getroffen werden. Definiert von der EA-Strategiefunktion, abgestimmt auf die Überzeugungen der Unternehmensführung — sie kodifizieren im Kern deren Risikohaltung.

**Das Beispiel — ein Prinzip aus Risikotoleranz abgeleitet:**

Ausgangsfrage ist die Haltung zum **Vendor-Lock-in** bei Cloud-Services (Abhängigkeit von einem Anbieter durch dessen proprietäre Dienste). Daraus können zwei gegensätzliche Prinzipien entstehen:

- **Cloud Native:** „Wir nutzen die proprietären Managed Services des Cloud-Anbieters voll aus." Begründung: Nutzen (weniger Eigenbetrieb, schnellere Entwicklung) > Lock-in-Risiko.
- **Cloud Agnostic:** „Wir bauen so, dass wir jederzeit den Anbieter wechseln könnten." Begründung: Unabhängigkeit > Kosten der Abstraktionsschicht.

**So wirkt das Prinzip:** Ein Team fragt sich, ob es einen Serverless-Functions-Dienst des Cloud-Anbieters (z. B. AWS Lambda — proprietär, nicht portabel) einsetzen soll. Cloud-Native-Unternehmen: ja, sofern technische Anforderungen erfüllt. Cloud-Agnostic-Unternehmen: nein — containerisierte Services, weil portierbar. Dieselbe Frage, gegenteilige Antworten — beide *richtig*, weil konsistent mit dem jeweiligen Prinzip. Ohne Prinzip hätten 50 Teams 50 Bauchentscheidungen getroffen.

**Die Bedingung:** Prinzipien wirken nur, wenn Entscheider sie kennen und verstehen — transparent eingebettet, am Ort der Entscheidung zugänglich.

**In einem Satz:** Prinzipien übersetzen die Risikohaltung der Führung in Entscheidungsregeln (Cloud Native vs. Cloud Agnostic als Musterfall) — und sind wertlos, wenn sie nicht dort verfügbar sind, wo entschieden wird.

*Brücke:* „Zugänglich" heisst im KI-Zeitalter: als Markdown im Repo, wo Entwickler *und* Coding-Agenten sie im Moment der Entscheidung lesen — nicht als PDF im Intranet.

## Architekturstandards

Ein Standard definiert **Anforderungen** — Dinge, die *wahr sein müssen* — und löst bei Verstoss Governance-Massnahmen aus. Unterschied zum Prinzip: Prinzipien *leiten* (Sollen), Standards *verpflichten* (Müssen, mit Konsequenz).

Arbeitsteilung über die drei EA-Funktionen: Die **Strategiefunktion definiert** den Standard, die **Enablement-Funktion befähigt** die Teams, ihn zu erfüllen, die **Oversight-Funktion erzwingt** die Einhaltung. Kernaussage: Standards funktionieren nur, wenn sie sowohl **enabled als auch enforced** werden können.

Bandbreite: von granular (freigegebene Programmiersprachen) bis breit (was eine „moderne Anwendung" ausmacht).

**In einem Satz:** Standards sind das Muss (Prinzipien das Soll) — und ein Standard, den man nur erzwingen, aber nicht erfüllbar machen kann (oder umgekehrt), ist keiner.

*Brücke:* Enablement = Golden Paths + Patterns-as-Code; Enforcement = OPA-Policies + CI/CD-Gates. Die Trias „define, enable, enforce" ist exakt das, was Policy-as-Code technisch umsetzt.

## Architecture Best Practices

Eine Best Practice ist ein *bewiesener* Weg, ein Problem besser zu lösen als die Alternativen — meist dokumentiert in einem Architecture Pattern. „Should"-Kategorie: empfohlen, nicht mit Standard-Strenge durchgesetzt.

Zwei Punkte: **Relevanz** — Best Practices dort definieren, wo die Reise hingeht, nicht wo man herkommt (wer in die Cloud modernisiert, braucht Practices für Migration und Cloud-Betrieb, nicht für On-Premises-Pflege). Und: Die Investition **rentiert nur bei Nutzung** — also wieder: eingebettet und zugänglich am Ort der Entscheidung.

**In einem Satz:** Best Practices sind bewiesene Lösungswege ohne Erzwingungsanspruch — definiert entlang der Zielrichtung der Organisation, und wertlos, wenn sie nicht dort liegen, wo gearbeitet wird.

## Frameworks

*(Im Buch eigener Abschnitt zwischen Standards und Best Practices — hier nur als Kurzeintrag, da nicht im Detail gelesen.)* Frameworks helfen zu definieren, was *wahr sein sollte*, und strukturieren Analysen und Entscheidungen. In den Buchbeispielen tauchen auf: **Build vs. Buy**, **Domain-Driven Design**, **Analysis of Alternatives** (Alternativenanalyse).

## Architekturdiagramme

Ein Diagramm visualisiert Eigenschaften einer Lösung oder eines Prozesses. Die EA-Strategiefunktion sollte **Diagramm-Standards** definieren: Diagrammtypen, Notation (Industriestandards), Tools, Legende (Farben, Formen, Linien) — für konsistente Lesbarkeit über Teams hinweg.

Das eigentliche Problem: Diagramme **veralten schnell**. Deshalb sind Aktualität und Dynamik Kernanforderungen — das Buch fragt selbst: *Wie halte ich ein Diagramm lebendig? Wie kodifiziere ich es, sodass es generierbar und abfragbar ist statt statisch?*

**In einem Satz:** Diagramme brauchen Standards für Konsistenz — aber vor allem müssen sie von statischen Zeichnungen zu generierbaren, abfragbaren Artefakten aus Code werden, sonst sind sie beim ersten Review schon veraltet.

*Brücke:* Genau diese Frage beantworten Structurizr DSL und LikeC4 — versioniertes Modell als Quelle, Diagramme als generierte Sichten. Das Buch verortet Diagramm-Kodifizierung als KR im Objective „embedded and accessible" — also messbares Governance-Ziel, kein Tooling-Detail.

## Architekturmetriken

Eine quantifizierbare Messgrösse zu einem Architekturanliegen — selbst ein Informationstyp, der Entscheidungen informiert.

Zwei Stossrichtungen: **Nach aussen** — TCO-/ROI-Metriken machen den Business Case für eine Technologiewahl belastbar. **Nach innen** — Metriken zu Standard-Adoption und Requirement-Einhaltung zeigen faktenbasiert, was beim Enablen und Enforcen funktioniert und was nicht.

**In einem Satz:** Metriken machen Architektur steuerbar in beide Richtungen — sie begründen Technologieentscheidungen gegenüber dem Business und messen, ob die eigene Governance (Enable/Enforce) überhaupt wirkt.

*Brücke:* Adoption/Adherence sind nur messbar, wenn Standards maschinenprüfbar sind — Policy-as-Code liefert die Metriken gratis (jeder Gate-Durchlauf ist ein Datenpunkt); PDF-Standards sind gar nicht messbar.

## Architekturinformation speist Architekturentscheidungen

Kernaussage: Alle Informationstypen existieren nicht für sich — ihr Zweck ist, **zum Entscheidungszeitpunkt** verfügbar zu sein. Drei Beispiele auf den drei Architekturebenen:

**1. Applikationsarchitektur: Hochverfügbarkeits-Design** (Business fordert High Availability)

- *Prinzipien:* was als Fault Domain gilt; Cloud Native vs. nicht
- *Standards:* RTO (Recovery Time Objective), RPO (Recovery Point Objective)
- *Patterns:* Best Practices für hochverfügbare Anwendungsarchitekturen
- *Frühere Entscheidungen:* zu Skalierung, Monitoring/Alerting, Datenbanktyp
- *Diagramme:* Deployment-Architektur der Anwendung

**2. Solution-Architektur: Investition in eine neue Capability**

- *Prinzipien:* z. B. Modernisierung
- *Standards:* Technologiestandards — zentrale Prüffrage: gibt es bereits eine Lösung, die den Bedarf abdeckt?
- *Frameworks:* Build vs. Buy, Domain-Driven Design
- *Frühere Entscheidungen:* zu verwandten Capabilities

**3. Enterprise-Architektur: neuer Technologiestandard** (z. B. Standardisierung auf einen Datenbanktyp)

- *Prinzipien:* z. B. Vendor Stickiness, Modernisierung
- *Frameworks:* z. B. Analysis of Alternatives
- *Frühere Entscheidungen:* zu verwandten Standards

**Folgerungen:** Jede Rolle — *und alle Partner, mit denen sie entscheidet* — braucht die passenden Informationstypen **zum Zeitpunkt der Entscheidung**. Je eingebetteter, zugänglicher, just-in-time verfügbarer, desto mehr Wiederverwendung und Effizienz. Doppelter Nutzen: bessere Entscheidungen *und* bessere Standard-Einhaltung.

*Auffällig:* Frühere **Entscheidungen sind in allen drei Beispielen Input** — das ADR-Archiv ist die einzige Quelle, die auf jeder Ebene wirkt.

---

# Teil C — Qualität und Messung

## High-Quality Target Architecture: elf Kriterien

Eine Zielarchitektur ist hochwertig, wenn sie zu *well-architected* Anwendungen führt:

| Kriterium | Bedeutung |
|---|---|
| **Scalable** | wächst und schrumpft automatisch mit der Last |
| **Resilient** | erholt sich schnell von Ausfällen, meist automatisiert |
| **Reliable** | Ausfallrisiken (Abhängigkeiten, Deployment-Architektur) so mitigiert, dass weder Daten noch kritische Transaktionen verloren gehen |
| **Cost-optimized** | Kostenhebel im Deployment bewusst abgewogen |
| **Functional** | erfüllt die Business-Anforderungen |
| **Future-proof** | *optimierte* Menge an Technical Debt — passt sich Änderungen leicht an |
| **Aligned** | alle betroffenen Stakeholder tragen die Build-/Buy-Entscheidung mit |
| **Secure** | schützt Daten, folgt den Cybersecurity-Prinzipien |
| **Modular** | unabhängig deploybar und wiederverwendbar, ohne andere Lösungen zu beeinträchtigen |
| **Extensible** | wohldefinierte Schnittstellen für Wachstum und Interoperabilität |
| **Compliant** | hält alle anwendbaren Policies/Standards ein — oder hat genehmigte Ausnahmen |

**Messung:**

- *Frühindikatoren (leading):* **Lieferkosten der Anwendung** (zu hoch → nicht kostenoptimiert, drückt auf den Gewinn); **verursachte Incidents** (zu viele → nicht skalierbar/resilient/zuverlässig, gefährdet Marke/Reputation).
- *Spätindikator (lagging):* **Anzahl genehmigter Architektur-Ausnahmen** (zu viele → akzeptiertes Risikoniveau gefährdet Compliance- und Betriebslage).

Zwei Feinheiten: „Future-proof" heisst **optimierte**, nicht null Technical Debt — Schulden sind ein Hebel, kein Verbot. Und der Ausnahmen-Indikator misst Architekturqualität indirekt über die Governance selbst — jede Exception ist dokumentierte, akzeptierte Abweichung; ihre Häufung ist das Warnsignal.

*Brücke:* Ein guter Teil der elf Kriterien ist automatisiert prüfbar — Scalable/Resilient via Chaos-Tests, Cost via FinOps-Daten, Secure/Compliant via Policy-as-Code, Modular/Extensible via Fitness Functions.

## Kapitelzusammenfassung (Kap. 2): Strategie für die EA-Praxis selbst

Auch die EA-Praxis braucht eine eigene Strategie — strukturiert über **OKRs**. Messung koppelt Architekturarbeit an Business Outcomes; outcome-basierte KRs sind KPIs, die Fortschritt anzeigen *und Verhalten incentivieren*.

Die vier Objectives (je ein Vertiefungskapitel, Kap. 3–6):

1. **Shared Alignment:** Stakeholder einigen — basiert auf Vertrauenskultur. Management-Prinzipien: Disagree and Commit, Command and Control, konsensbasiert/-getrieben.
2. **Embedded and Accessible:** Architekturinformation in Alltagsprozesse und -Tools einbetten, nutzbar für die Entscheider.
3. **Enable and Enforce:** Standards erfüllbar machen *und* durchsetzen.
4. **Proactive and Reactive:** strategische und taktische Entscheidungen treffen und ausbalancieren.

*Brücke:* Die vier Objectives sind die Prüfsteine jeder Architecture-as-Code-Initiative — AaC zahlt vor allem auf #2 (Repo statt Intranet) und #3 (Golden Paths + Policy-as-Code) ein; #1 und #4 bleiben menschlich.

---

# Teil D — Architecture Decision Making (Kap. 14)

## Fundament 1: Architecture Decision Registry (das Werkzeug)

Das primäre Werkzeug: die **Registry** — das Verzeichnis aller Entscheidungsrecords. Doppelter Zweck: beste Wahl unter den Umständen treffen *und* historisches Protokoll des Rationale führen, damit jedes (auch neue) Teammitglied versteht, was warum entschieden wurde.

**Lösungswahl nach Organisationskultur:** Software-geprägt → Markdown im Source-Repo, zugänglich über Developer Portal. Traditioneller → Wiki/Knowledge Base. Kein richtig/falsch — die Lösung muss zu den Nutzern passen.

**Drei Auswahlkriterien:**

- **Accessibility:** leicht verfügbar für *alle*, die mit den Records arbeiten
- **Usability:** leicht durchsuchbar — Nutzer finden die für sie relevanten Records
- **Auditability:** Unveränderlichkeit der Records, nachvollziehbare Genehmigungen und Zeitstempel

**Die eiserne Regel:** Genehmigte Records sind **immutable**. Bei Änderungen: neuer Record, alter wird „deprecated" — mit Verlinkung für Traceability.

*Anwendungshinweis:* Git erfüllt alle drei Kriterien nativ — Historie unveränderlich, Approval = Merge mit Timestamp und Approver, Suche über Portal/Grep. Ein Wiki muss Immutability organisatorisch simulieren.

## Fundament 2: Lifecycle eines Decision Records (der Prozess)

Fünf Zustände (Fig. 14-1): **Draft → In Progress → Review ⇄ In Progress → Approved → Deprecated**

1. **Draft:** Record anlegen, mit Standard-Template. Kürzester Schritt — *ausser* die Ownership-Frage ist strittig (zuerst klären!).
2. **In Progress:** Anreichern mit Research, Daten, Feedback.
3. **Review:** mit **Feedback-Schleife** zurück zu In Progress — so oft wie nötig, bis alle zu konsultierenden Stakeholder durchlaufen sind.
4. **Approved:** finale Review durch die Instanz mit Genehmigungsbefugnis. Danach abgeschlossen und unveränderlich.
5. **Deprecated:** wenn etwas die Entscheidung invalidiert oder eine neue nötig macht — immer zugunsten eines neuen Records, mit dokumentiertem Link.

**Wichtigster Praxistipp:** In der Feedback-Schleife gezielt die Stakeholder suchen, die am ehesten **widersprechen**. Sie stärken die Entscheidung: Bedenken proaktiv entkräften oder als Risiko dokumentieren. Alignment kann Kompromiss erfordern — keine Entscheidung ist perfekt; sie muss *good enough* sein auf Basis des aktuellen Wissens (festgehalten als Annahmen und Implikationen).

## Fundament 3: Decision Workflow (die Governance)

Der Workflow regelt, **wer was entscheiden darf** — ein Kulturstatement: Autokratie behält Entscheidungsmacht oben (Teams warten und führen aus); Empowerment verteilt sie. Buchempfehlung: **Autonomie plus right-sized Governance**. Der Workflow „shines the light of truth on empowerment" — er zeigt schonungslos, wie ernst die Organisation Autonomie meint.

**Volume vs. Impact (Fig. 14-2):** Drei Entscheider-Kategorien — **Team** (einzelnes Produkt-/App-Team), **Org Unit** (Einheit mit vielen Teams), **Enterprise** (alles). Entscheidungs-*Volumen* ist beim Team am höchsten, beim Enterprise am niedrigsten — der *Impact* umgekehrt (Trichter). Entscheidend: Jeder kann Entscheidungen treffen, deren Wirkung über die eigene Ebene hinausreicht.

Beispiele: Ein Team pumpt das Enterprise Data Warehouse weit über die Projektionen voll — eine „lokale" Entscheidung sprengt Kosten/Performance eines Enterprise-Services. Positiv: Ein Team verbessert seine Codequalität und inspiriert Qualitätsstandards für die ganze Org Unit.

**Entscheidungshierarchie (Fig. 14-3):** Enterprise-Forum ← Org-Unit-Foren + Horizontal-Foren (Mittelebene) ← Teams + Working Groups (lokal).

- **Team** = kleinste atomare Entscheidungseinheit; ad-hoc, cross-funktionale **Working Groups** als flexible Struktur.
- **Mittelebene** = organisationsspezifisch: Lines of Business plus **Horizontals** quer über die Einheit (z. B. Architekturteam, zentrale SRE-Funktion).
- **Enterprise-Forum** = verantwortlich für enterprise-weite Entscheidungen (z. B. neuer Technologiestandard); Horizontals über alle Einheiten (z. B. Cybersecurity).

**Spielregeln:**

- Jede Einheit hat **Autonomie** innerhalb ihres Scope und Impact.
- **Transparenz in beide Richtungen** über alle Ebenen.
- **Eskalation nur bei Impact-Überschreitung:** Beispiel: neuer Drittanbieter mit völlig neuem Konnektivitätsmuster → bis Enterprise eskalieren (Muster-Validierung). Gegenbeispiel: Org Unit definiert *strengere* Standards als das Enterprise → keine Eskalation nötig.
- **Mehrere Horizontals im Governance-Modell (Fig. 14-4):** Transparenz zu Cyber, Data, Risk, „Other" (z. B. Procurement, Legal).

**Tipp gegen Dauerpatt:** Jedes Forum braucht eine klare **Charter mit explizitem Zweck und Vorsitz** — sonst wird geredet statt entschieden.

## Architecture Decision Training (People)

**Warum:** Autonomie funktioniert nur, wenn die Leute wissen, *wie* man entscheidet — sonst Fehlentscheide oder Rückdelegation nach oben.

**Rollen (Fig. 14-5),** gemappt auf die Hierarchie:

- **ECA (Enterprise Chief Architect):** leitet das Enterprise-Forum (mit DCAs + Horizontal-Gruppen)
- **DCA (Divisional Chief Architect):** leitet das Forum der eigenen Org Unit
- **SA (Solution Architect):** leitet bei Bedarf Horizontal-Foren seiner Architekturdomäne
- **AA (Application Architect):** Team hat formal keinen Chair, aber der AA ist auf Teamebene **accountable** — in Partnerschaft mit dem Team, nicht über ihm

**Nicht nur Architekten:** Business-/Produkt- und Technologiepartner sitzen als Stakeholder mit am Tisch (Rollen in der Forums-Charter präzisiert) und brauchen ebenfalls Training — sie sind an Entscheidung *und* Umsetzung beteiligt. Verankerung: **Role-based Onboarding** (oder allgemeines Onboarding). Tipp: Terminologie für Partner anpassen — „Technologieentscheidungen" statt „Architekturentscheidungen".

**Vier Trainingsinhalte:** (1) Foren richtig nutzen — selbst entscheiden vs. Impact-bedingt eskalieren; (2) Template beherrschen; (3) Registry bedienen; (4) Best Practices, insbesondere **NFRs** pro Entscheidungstyp mitdenken (Kap. 5).

**Damit ist das Fundament komplett:** Tooling (Registry) + Prozess (Lifecycle, Governance) + People (Training).

*Anwendungshinweis:* Training als Onboarding-Baustein reproduziert sich selbst; als einmalige Schulungswelle verpufft es. Und: Liegt Entscheidungswissen (Template, Eskalationsregeln, NFR-Checklisten) maschinenlesbar im Repo, wird ein Teil des „Trainings" zur Laufzeit-Hilfe — Agent oder Portal soufflieren die Regeln im Moment der Entscheidung.

## Framework for Architecture Decision Making

**Zielbild:** Entscheidungsfindung **enterpriseweit skalieren** — konsistent, hochwertig, transparent. Drei überlappende Phasen als kontinuierlicher Feedback-Loop (Fig. 14-6, konzentrische Kreise):

1. **Satisfy Prerequisites** — Fundament: People, Processes, Tools
2. **Monitor Execution** — verifizieren, dass Entscheidungen konsistent und hochwertig sind
3. **Evolve and Enhance** — kontinuierlich verbessern auf Basis der Monitoring-Erkenntnisse

**Phase 2: Monitor Execution — zwei Messobjekte:**

a) *Den Prozess überwachen*, mit Leitfragen pro Voraussetzung: Registry — wird sie genutzt oder entstehen **Grassroots-Alternativen**? Nutzer zufrieden? Lifecycle/Workflow — Dauer pro Phase, **Bottlenecks**, funktionieren die Feedback-Schleifen? Training — alle Rollen abgedeckt, Refresher nötig?

b) *Die Wirkung der Entscheidungen überwachen* (Fig. 14-7): **Make Decision → Make Change(s) → Validate Alignment.** Nach Umsetzung prüfen, ob die Änderungen der Entscheidung entsprechen, plus kurzes **Postmortem**: War die Entscheidung ein Treffer? Wenn nicht — warum nicht? (Erfolge feiern, Fehlschläge als Lernchance.)

Messinstrument: **Architecture Fitness Functions** (*Building Evolutionary Architectures*, Ford et al., O'Reilly 2017). Beispiel Datenbank-Auswahl: Die Entscheidung löste Anforderungen an Datenzugriff, Konsistenz, Verfügbarkeit, Performance — Nutzungsmessungen über die Zeit belegen, ob sie erfüllt werden. Die Entscheidung wird **nachträglich validierbar** statt nur dokumentiert.

**Phase 3: Evolve and Enhance:** Neue Gewohnheiten brauchen **Zeit, Wiederholung, Anreize**. Erkenntnisse fliessen in zwei Kanäle: (1) kontinuierliche Verbesserung + strategische OKRs; (2) **Kommunikation/Marketing der EA** — EA balanciert permanent zwischen Bürokratie und Value-Add. Deckt Monitoring einen Bottleneck auf: **Control the message** — Schmerzpunkt anerkennen und Behebung zeigen, oder erklären, warum die Strenge wegen der Risiken nötig ist. Schweigen ist die einzige falsche Option.

**Anwendungs-Essenz:** Fundament bauen → beides messen (Prozessgesundheit *und* Entscheidungswirkung) → Erkenntnisse in Verbesserung und Kommunikation zurückspielen. Die meisten Organisationen messen höchstens, *dass* entschieden wurde — nicht, ob der Prozess flutscht und ob sich die Entscheidung bewährt hat.

*Brücke:* Fitness Functions in ihrer zweiten Rolle — nicht nur Governance-Gate *vor* der Umsetzung, sondern Validierungsinstrument *danach* („Validate Alignment" automatisiert). Und die Prozessmetriken fallen bei einer Git-basierten Registry gratis ab: PR-Durchlaufzeiten, Review-Zyklen, Commit-Statistiken.

## Anwendungs-Checkliste (kondensiert)

1. Registry wählen (Accessibility, Usability, Auditability) — Immutability sicherstellen
2. Standard-Template (siehe ADR-Felder oben) + Lifecycle mit definierten Zuständen etablieren
3. Ownership-Frage pro Entscheidung zuerst klären
4. Kritiker aktiv in die Review-Schleife holen
5. Entscheidungsebenen (Team / Org Unit / Enterprise) und Eskalationsregel „Impact > Scope → eskalieren" definieren
6. Foren mit Charter, Zweck und Chair ausstatten; Transparenz zu Cyber/Data/Risk/Legal herstellen
7. Training im Role-based Onboarding verankern (Architekten *und* Business-/Tech-Partner)
8. Monitoring aufsetzen: Prozessgesundheit (Registry-Nutzung, Phasendauer, Bottlenecks) und Entscheidungswirkung (Validate Alignment, Postmortems, Fitness Functions)
9. Erkenntnisse in OKRs und EA-Kommunikation zurückspielen

*Gesamtbrücke zu Architecture-as-Code:* Die Eskalationsregel nach Impact ist die gesuchte Trennlinie der Governance im KI-Zeitalter — was die Pipeline automatisch prüft (Team-Scope, Standard eingehalten) vs. was ins Gremium muss (Impact über Scope hinaus).