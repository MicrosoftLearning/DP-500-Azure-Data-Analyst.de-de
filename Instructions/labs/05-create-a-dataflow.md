---
lab:
  title: Erstellen eines Dataflows
  module: Prepare data for tabular models in Power BI
---

# Erstellen eines Dataflows

## Übersicht

**Die geschätzte Dauer dieses Labs beträgt 45 Minuten.**

In diesem Lab erstellen Sie einen Datenfluss, um Datumsdimensions-Daten bereitzustellen, die aus dem Azure Synapse Adventure Works-Data Warehouse bezogen werden. Der Dataflow bietet eine konsistente Definition von datumsbezogenen Daten für die Verwendung durch die Geschäftsanalysten der Organisation.

In diesem Lab lernen Sie Folgendes:

- Verwenden Sie Power Query Online, um einen Datenfluss zu entwickeln.

- Verwenden Sie Power BI Desktop, um einen Datenfluss zu nutzen.

## Erste Schritte

In dieser Übung bereiten Sie Ihre Umgebung vor.

### Laden von Daten in Azure Synapse Analytics

   > **Hinweis**: Wenn Sie bereits Daten mithilfe eines Git-Klons in Azure Synapse Analytics geladen haben, können Sie diese Aufgabe überspringen und mit dem Einrichten von Power BI fortfahren. ****

1. Melden Sie sich mit den Anmeldeinformationen auf der Registerkarte Ressourcen auf der rechten Seite der VM beim [Azure-Portal](https://portal.azure.com) an.
2. Verwenden Sie rechts neben der Suchleiste oben auf der Seite die Schaltfläche **[\>_]** , um eine neue Cloud Shell-Instanz im Azure-Portal zu erstellen. Wählen Sie eine ***PowerShell***-Umgebung aus, und erstellen Sie Speicher, falls Sie dazu aufgefordert werden. Die Cloud Shell bietet eine Befehlszeilenschnittstelle in einem Bereich am unteren Rand des Azure-Portals, wie hier gezeigt:

    ![Azure-Portal mit einem Cloud Shell-Bereich](../images/cloud-shell.png)

    > **Hinweis**: Wenn Sie zuvor eine Cloudshell erstellt haben, die eine *Bash-Umgebung* verwendet, verwenden Sie das Dropdownmenü oben links im Bereich der Cloudshell, um sie in*** Power Shell ***zu ändern.

3. Beachten Sie, dass Sie die Größe der Cloud Shell durch Ziehen der Trennzeichenleiste oben im Bereich ändern können oder den Bereich mithilfe der Symbole **&#8212;**, **&#9723;** und **X** oben rechts minimieren, maximieren und schließen können. Weitere Informationen zur Verwendung von Azure Cloud Shell finden Sie in der [Azure Cloud Shell-Dokumentation](https://docs.microsoft.com/azure/cloud-shell/overview).

4. Geben Sie im Terminal die folgenden Befehle ein, um dieses Repository zu klonen:

    ```
    rm -r dp500 -f
    git clone https://github.com/MicrosoftLearning/DP-500-Azure-Data-Analyst dp500
    ```

5. Nachdem das Repository geklont wurde, geben Sie die folgenden Befehle ein, um in den **Setup**-Ordner zu wechseln und führen Sie das darin enthaltene Skript **setup.ps1** aus:

    ```
    cd dp500/Allfiles/04
    ./setup.ps1
    ```

6. Wenn Sie dazu aufgefordert werden, geben Sie ein geeignetes Kennwort ein, das für Ihren Azure Synapse SQL-Pool festgelegt werden soll.

    > **Hinweis**: Merken Sie sich unbedingt das Kennwort!

7. Warten Sie, bis das Skript abgeschlossen ist – dies dauert in der Regel etwa 20 Minuten; in einigen Fällen kann es jedoch länger dauern.

1. Nachdem Sie den Synapse-Arbeitsbereich und den SQL-Pool erstellt und die Daten geladen haben, hält das Skript den Pool an, um unnötige Azure-Gebühren zu vermeiden. Wenn Sie bereit sind, mit Ihren Daten in Azure Synapse Analytics zu arbeiten, müssen Sie den SQL-Pool fortsetzen.

### Klonen des Repositorys für diesen Kurs

1. Öffnen Sie im Startmenü die Eingabeaufforderung

    ![](../images/command-prompt.png)

1. Navigieren Sie im Eingabeaufforderungsfenster zum D-Laufwerk, indem Sie Folgendes eingeben:

    `d:` 

   Drücken Sie die Eingabetaste.

    ![](../images/command-prompt-2.png)


1. Geben Sie im Eingabeaufforderungsfenster den folgenden Befehl ein, um die Kursdateien herunterzuladen und in einem Ordner namens DP500 zu speichern.
    
    `
    git clone https://github.com/MicrosoftLearning/DP-500-Azure-Data-Analyst DP500
    `
   
2. Wenn das Repository geklont wurde, schließen Sie das Eingabeaufforderungsfenster. 
   
3. Öffnen Sie das D-Laufwerk im Datei-Explorer, um sicherzustellen, dass die Dateien heruntergeladen wurden.

### Öffnen Sie den Power BI Desktop

In dieser Aufgabe richten Sie den Power BI Desktop ein.

1. Um den Datei-Explorer zu öffnen, wählen Sie auf der Taskleiste den Shortcut **File Explorer** aus.

1. Navigieren Sie zum Ordner **D:\DP500\Allfiles\05\Starter**.

1. Um eine vordefinierte Power BI Desktop-Datei zu öffnen, doppelklicken Sie auf die Datei **Sales Analysis - Create a dataflow.pbix**.

1. Wenn Sie noch nicht angemeldet sind, wählen Sie in der oberen rechten Ecke von Power BI Desktop die Option **Anmelden** aus. Verwenden Sie die Lab-Anmeldeinformationen, um den Anmeldevorgang abzuschließen.

    ![](../images/dp500-create-a-dataflow-image2.png)

1. Um die Datei zu speichern, wählen Sie im Menüband **Datei** die Option **Speichern unter** aus.

1. Navigieren Sie im Fenster **Speichern unter** zum Ordner **D:\PL300\MySolution**.

1. Navigieren Sie zum Power BI Desktop und wählen **Datei** aus, dann **Optionen und Einstellungen** dann **Optionen** dann **Sicherheit** und unter Authentifizierung Überprüfung des Browsers **Meinen Standard-Web-Brower verwenden** und wählen Sie **Speichern** aus.

    *Sie aktualisieren die Power BI Desktop-Lösung so, dass ein Datenfluss verwendet wird, um Datumsdimensionsdaten zu erhalten.*

### Anmelden beim Power BI-Dienst

Bei dieser Aufgabe melden Sie sich beim Power BI-Dienst an, starten eine Testlizenz und erstellen einen Arbeitsbereich.

*Wichtig: Wenn Sie Power BI bereits in Ihrer VM-Umgebung eingerichtet haben, fahren Sie mit der nächsten Aufgabe fort.*

1. Rufen Sie in einem Webbrowser [https://powerbi.com](https://powerbi.com/) auf.

1. Verwenden Sie die Lab-Anmeldeinformationen, um den Anmeldevorgang abzuschließen.

    *Wichtig: Sie müssen dieselben Anmeldeinformationen verwenden, die zum Anmelden von Power BI Desktop verwendet werden.*

1. Wählen Sie oben rechts das Profilsymbol und dann **Testversion starten** aus.

    ![](../images/dp500-create-a-dataflow-image3.png)

1. Wenn Sie dazu aufgefordert werden, wählen Sie **Testversion starten** aus.


2. Führen Sie alle verbleibenden Aufgaben aus, um die Testeinrichtung abzuschließen.

    *Tipp: Die Power BI-Webbrowserumgebung wird als **Power BI-Dienst** bezeichnet.*

9. Wählen Sie Arbeitsbereiche und **Einen Arbeitsbereich erstellen**.

    ![](../images/dp500-create-a-star-schema-model-image2a.png)

10. Erstellen Sie einen Arbeitsbereich mit dem Namen DP500 Labs, und wählen Sie **Speichern**aus.

    *Hinweis: Der Arbeitsbereichsname muss innerhalb des Mandanten eindeutig sein. Wenn eine Fehlermeldung angezeigt wird, ändern Sie den Arbeitsbereichsnamen.*

Der Arbeitsbereich wird erstellt und geöffnet. In einer späteren Übung erstellen Sie einen Datenfluss für diesen Arbeitsbereich.

### Starten des SQL-Pools

In dieser Aufgabe starten Sie den SQL-Pool.

1. Rufen Sie in einem Webbrowser [https://portal.azure.com](https://portal.azure.com/) auf.

1. Verwenden Sie die Lab-Anmeldeinformationen, um den Anmeldevorgang abzuschließen.

1. Verwenden Sie die Suchleiste, um Azure Synapse Analytics zu suchen. 

1. Wählen Sie die Azure Synapse Analytics-Instanz.
    ![](../images/synapse-instance.png)

1. Suchen und wählen Sie den dedizierten SQL-Pool aus.
    ![](../images/dedicated-sql-pool.png)

1. Fortsetzen des dedizierten SQL-Pools.

    ![](../images/resume-sql-pool.png)

    *Wichtig: Der SQL-Pool ist eine kostspielige Ressource. Beschränken Sie die Verwendung dieser Ressource, wenn Sie an diesem Labor arbeiten. Der letzte Vorgang in dieser Übung weist Sie an, die Ressource anzuhalten.*

## Entwickeln eines Datenflusses

In dieser Übung entwickeln Sie einen Datenfluss zur Unterstützung der Power BI-Modellentwicklung. Sie stellt eine konsistente Darstellung der Data Warehouse-Datumsdimensionstabelle bereit.

### Überprüfen Sie das Datenmodell

In dieser Aufgabe überprüfen Sie das in Power BI Desktop entwickelte Datenmodell.

1. Wechseln Sie zur Power BI Desktop-Lösung.

1. Wechseln Sie links zur **Modellansicht**.

    ![](../images/dp500-create-a-dataflow-image8.png)

1. Beachten Sie im Modelldiagramm die Tabelle **Datum**.

    ![](../images/dp500-create-a-dataflow-image9.png)

    *Die Tabelle **Datum** wurde vom Geschäftsanalysten erstellt. Sie stellt keine konsistente Definition von datumsbezogenen Daten dar und enthält keine hilfreichen Offsetspalten, um relative Datumsfilter zu unterstützen. In einer späteren Übung ersetzen Sie diese Tabelle durch eine neue Tabelle, die aus einem Datenfluss stammt.*

### Erstellen eines Dataflows

In dieser Aufgabe erstellen Sie einen Datenfluss, der eine konsistente Definition datumsbezogener Daten darstellt.

1. Wählen Sie im Power BI-Dienst **Neu **;** Datenfluss** aus.

    ![](../images/dp500-create-a-dataflow-image10.png)

1. Wählen Sie unter Titel **Neue Tabellen definieren** die Option **Neue Tabellen hinzufügen**.

    ![](../images/dp500-create-a-dataflow-image12.png)

    *Das Hinzufügen neuer Tabellen umfasst die Verwendung von Power Query Online zum Definieren von Abfragen.*

1. Um eine Datenquelle auszuwählen, wählen Sie **Azure Synapse Analytics (SQL DW)** aus.

    ![](../images/dp500-create-a-dataflow-image13.png)

    *Tipp: Sie können das Suchfeld (oben rechts) verwenden, um die Datenquelle zu finden.*

1. Geben Sie die Einstellungen für die Synapse-Verbindung ein.

     - Geben Sie den Servernamen aus dem Azure-Portal ein
     
     ![](../images/synapse-sql-pool-connection-string.png)
     
      Der Servername sollte etwa wie folgt aussehen:
      
      synapsewsxxxxx.sql.azuresynapse.net
      
     - Sicherstellen, dass die Authentifizierung ein **Organisations-Konto** ist. Wenn Sie aufgefordert werden, sich anzumelden, verwenden Sie die bereitgestellten Anmeldeinformationen des Labors.
     ![](../images/synapse-sql-pool-sign-in.png)

1. Unten rechts, wählen Sie **Weiter**aus.

    ![](../images/dp500-create-a-dataflow-image14.png)

1. Erweitern Sie im Power Query-Navigationsbereich den Sqldw, und wählen Sie die Tabelle**DimDate** aus (nicht überprüfen).

    ![](../images/dp500-create-a-dataflow-image15.png)

1. Beachten Sie die Vorschau von Tabellendaten.

1. Um eine Abfrage zu erstellen, überprüfen Sie die Tabelle **DimDate**.

    ![](../images/dp500-create-a-dataflow-image16.png)

1. Wählen Sie unten rechts **Transformieren von Daten**.

    ![](../images/dp500-create-a-dataflow-image17.png)

    *Power Query Online wird jetzt verwendet, um Transformationen auf die Tabelle anzuwenden. Es bietet eine nahezu identische Oberfläche mit dem Power Query-Editor in Power BI Desktop.*

1. Im Bereich **Abfrageeinstellungen** (rechts) ersetzen Sie, um die Abfrage umzubenennen, im Feld **Name**, den Text mit **Datum**, und dann drücken Sie die **Eingabetaste**.

    ![](../images/dp500-create-a-dataflow-image18.png)

1. Klicken Sie auf der Registerkarte **Start** des Menübands in der Gruppe **Spalten verwalten** auf das Symbol **Spalten auswählen**, um Spalten zu entfernen.

    ![](../images/dp500-create-a-dataflow-image19.png)

1. Deaktivieren Sie im Fenster **Spalten auswählen** das erste Kontrollkästchen, um alle Kontrollkästchen zu deaktivieren.

    ![](../images/dp500-create-a-dataflow-image20.png)


1. Überprüfen Sie die folgenden fünf Spalten.

    - DateKey

    - FullDateAlternateKey

    - MonthNumberOfYear

    - FiscalQuarter

    - FiscalYear

    ![](../images/dp500-create-a-dataflow-image21.png)

1. Wählen Sie **OK** aus.

    ![](../images/dp500-create-a-dataflow-image22.png)

  
1. Beachten Sie im Bereich **Abfrageeinstellungen** in der Liste **Angewendete Schritte**, dass ein Schritt zum Entfernen anderer Spalten hinzugefügt wurde.

    ![](../images/dp500-create-a-dataflow-image23.png)

    *Power Query definiert Schritte, um die gewünschte Struktur und Daten zu erhalten. Jede Transformation ist ein Schritt in der Abfragelogik.*

1. Um die Spalte **FullDateAlternateKey** umzubenennen, doppelklicken Sie auf die Spaltenüberschrift **FullDateAlternateKey**.

1. Ersetzen Sie den Text durch **Datum**, und drücken Sie die **Eingabetaste**.

    ![](../images/dp500-create-a-dataflow-image24.png)

1. Um eine berechnete Spalte hinzuzufügen, klicken Sie auf der Registerkarte des Menübands **Spalte hinzufügen** in der Gruppe **Allgemein** auf **Benutzerdefinierte Spalte**.

    ![](../images/dp500-create-a-dataflow-image25.png)

   

1. Ersetzen Sie im Fenster **Benutzerdefinierte Spalte** im Feld **Neuer Spaltenname** den Text durch **Jahr**.

1. Wählen Sie in der Dropdown-Liste **Datentyp** **Text**.

    ![](../images/dp500-create-a-dataflow-image26.png)

1. Geben Sie in das Feld **Benutzerdefinierte Spaltenformel** die folgende Formel ein:

    *Tipp: Alle Formeln können Sie aus**D:\DP500\Allfiles\05\Assets\Snippets.txt** kopieren und einfügen.*


    ```
    "FY" & Number.ToText([FiscalYear])
    ```


1. Wählen Sie **OK** aus.

    *Fügen Sie jetzt vier weitere benutzerdefinierte Spalten hinzu.*

1. Fügen Sie eine weitere benutzerdefinierte Spalte mit dem Namen **Quartal** und dem Datentyp **Text** unter Verwendung der folgenden Formel hinzu:


    ```
    [Year] & " Q" & Number.ToText([FiscalQuarter])
    ```


1. Fügen Sie eine weitere benutzerdefinierte Spalte mit dem Namen **Monat** und dem Datentyp **Text** unter Verwendung der folgenden Formel hinzu:


    ```
    Date.ToText([Date], "yyyy-MM")
    ```

1. Fügen Sie eine weitere benutzerdefinierte Spalte mit dem Namen **Monats Offset** (mit einem Leerzeichen zwischen den Wörtern) und dem Datentyp **Ganze Zahl** unter Verwendung der folgenden Formel hinzu:


    ```
    ((Date.Year([Date]) * 12) + Date.Month([Date])) - ((Date.Year(DateTime.LocalNow()) * 12) + Date.Month(DateTime.LocalNow()))
    ```


    *Diese Formel bestimmt die Anzahl der Monate ab dem aktuellen Monat. Der aktuelle Monat ist Null, die vergangenen Monate sind negativ und die zukünftigen Monate sind positiv. Zum Beispiel hat der letzte Monat den Wert -1.*

   

1. Fügen Sie eine weitere benutzerdefinierte Spalte mit dem Namen **Monats Offset Filter** (mit Leerzeichen zwischen den Wörtern) mit dem Datentyp **Text** unter Verwendung der folgenden Formel hinzu:


    ```
    if [Month Offset] > 0 then Number.ToText([Month Offset]) & " month(s) future"

    else if [Month Offset] = 0 then "Current month"

    else Number.ToText(-[Month Offset]) & " month(s) ago"
    ```


    *Diese Formel wandelt den numerischen Offset in ein benutzerfreundliches Textformat um.*

    *Tipp: Alle Formeln können Sie aus**D:\DP500\Allfiles\05\Assets\Snippets.txt** kopieren und einfügen.*

1. Wählen Sie auf der Registerkarte des Menübands **Startseite** in der Gruppe **Spalten verwalten** das Symbol **Spalten auswählen**, um nicht benötigte Spalten zu entfernen.

    ![](../images/dp500-create-a-dataflow-image27.png)

1. Deaktivieren Sie im Fenster **Spalten auswählen** die folgenden Spalten:

    - MonthNumberOfYear

    - FiscalQuarter

    - FiscalYear

    ![](../images/dp500-create-a-dataflow-image28.png)

1. Wählen Sie **OK** aus.

1. Wählen Sie unten rechts **Speichern &amp;Schließen**.

    ![](../images/dp500-create-a-dataflow-image29.png)

1. Geben Sie im Fenster **Speichern Sie Ihren Datenfluss** in das Feld **Name** **Unternehmensdatum** ein.

1. Geben Sie in das Feld **Beschreibung** ein: **Konsistente Datumsdefinition, die in allen Adventure Works-Datensätzen verwendet werden soll**.

1. Tipp: Die Beschreibung können Sie aus **D:\DP500\Allfiles\05\Assets\Snippets.txt** kopieren und einfügen.

    ![](../images/dp500-create-a-dataflow-image30.png)

1. Wählen Sie **Speichern**.

    ![](../images/dp500-create-a-dataflow-image31.png)

1. Wählen Sie im Power BI-Dienst im Bereich **Navigation** den Namen Ihres Arbeitsbereichs.

    *Diese Aktion öffnet die Startseite für den Arbeitsbereich.*

1. Um den Datenfluss zu aktualisieren, bewegen Sie den Cursor auf den Datenfluss **Unternehmensdatum** und wählen Sie das Symbol **Jetzt aktualisieren**.

    ![](../images/dp500-create-a-dataflow-image32.png)

  

1. Um zu den Datafloweinstellungen zu gelangen, bewegen Sie den Cursor auf den Datenfluss **Unternehmensdatum**, wählen die Auslassungspunkte aus und wählen dann **Einstellungen**.

    ![](../images/dp500-create-a-dataflow-image33.png)

1. Beachten Sie die Konfigurationsoptionen.

    ![](../images/dp500-create-a-dataflow-image34.png)

    *Es gibt zwei Einstellungen, die konfiguriert werden müssen. Erstens sollte die geplante Aktualisierung so konfiguriert werden, dass die Dataflowdaten täglich aktualisiert werden. Auf diese Weise werden die monatlichen Offsets anhand des aktuellen Datums berechnet. Zweitens sollte der Datenfluss als zertifiziert (von einem autorisierten Prüfer) bestätigt werden. Ein zertifizierter Datenfluss erklärt gegenüber anderen, dass er Qualitätsstandards erfüllt und als zuverlässig und verbindlich angesehen werden kann.*

    *Zusätzlich zur Konfiguration der Einstellungen sollte allen Inhaltserstellenden die Erlaubnis erteilt werden, den Datenfluss zu nutzen.*.

## Nutzen eines Dataflows

In dieser Übung ersetzen Sie in der Power BI Desktop-Lösung die bestehende Tabelle **Datum** durch eine neue Tabelle, die ihre Daten aus dem Datenfluss bezieht.

### Entfernen der ursprünglichen Datumstabelle

In dieser Aufgabe entfernen Sie die ursprüngliche Tabelle **Datum**.

1. Wechseln Sie zur Power BI Desktop-Lösung.

1. Klicken Sie im Modelldiagramm mit der rechten Maustaste auf die Tabelle **Datum** und wählen Sie dann **Aus Modell löschen**.

    ![](../images/dp500-create-a-dataflow-image35.png)

1. Wenn Sie aufgefordert werden, die Tabelle zu löschen, wählen Sie **OK**.

    ![](../images/dp500-create-a-dataflow-image36.png)

  


### Fügen Sie eine neue Datumstabelle hinzu

In dieser Aufgabe fügen Sie eine neue **Datums-** tabelle hinzu, die die zugehörigen Daten aus dem Datenfluss angibt.

1. Auf dem Menüband **Startseite** wählen Sie aus der Gruppe **Daten** das Symbol **Abrufen von Daten** aus.

    ![](../images/dp500-create-a-dataflow-image37.png)

1. Im Fenster **Abrufen von Daten**, links, wählen Sie **Power Platform** aus und dann wählen Sie **Power BI Datenflüsse** aus.

    ![](../images/dp500-create-a-dataflow-image38.png)

1. Wählen Sie **Verbinden**.

    ![](../images/dp500-create-a-dataflow-image39.png)

  

1. Wählen Sie im Fenster **Power BI Datenflüsse** **Anmelden** aus.

    ![](../images/dp500-create-a-dataflow-image40.png)

1. Verwenden Sie die Lab-Anmeldeinformationen, um den Anmeldevorgang abzuschließen.

    *Wichtig: Sie müssen dieselben Anmeldeinformationen verwenden, die zum Anmelden bei der Power BI-Dienst verwendet werden.*

1. Wählen Sie **Verbinden**.

    ![](../images/dp500-create-a-dataflow-image41.png)

1. Erweitern Sie im Fenster **Navigator** im linken Bereich Ihren Arbeitsbereichsordner, und erweitern Sie dann den Datenflussordner **Unternehmensdatum**.

    ![](../images/dp500-create-a-dataflow-image42.png)


1. Prüfen Sie die **Datums** tabelle.

    ![](../images/dp500-create-a-dataflow-image43.png)

1. Wählen Sie **Laden** aus.

    ![](../images/dp500-create-a-dataflow-image44.png)

    *Es ist möglich, die Daten mithilfe der Power Query-Editor zu transformieren.*

1. Wenn die neue Tabelle dem Modell hinzugefügt wird, erstellen Sie eine Beziehung durch Ziehen der Spalte **DateKey** aus der**Daten**tabellen zur Spalte **OrderDateKey** der Tabelle **Vertrieb**.

    ![](../images/dp500-create-a-dataflow-image45.png)

    *Es gibt viele andere Modellkonfigurationen, z. B. das Ausblenden von Spalten oder das Erstellen einer Hierarchie, die durchgeführt werden kann.*

### Überprüfen des Modells

In dieser Aufgabe testen Sie das Modell, indem Sie ein einfaches Berichtslayout erstellen.

1. Wechseln Sie links zur Ansicht **Bericht**.

    ![](../images/dp500-create-a-dataflow-image46.png)

1. Um der Seite ein Diagramm im Bereich **Visualisierungen** hinzuzufügen, wählen Sie das gestapelte Balkendiagramm.

    ![](../images/dp500-create-a-dataflow-image47.png)

1. Ändern Sie die Größe des Diagramms so, dass es die Bereichtsseite einnimmt.

  

1. Im Bereich **Daten** erweitern Sie die **Daten** tabelle und dann ziehen Sie das **Month Offset Filter** Feld in das Balkendiagramm.

    ![](../images/dp500-create-a-dataflow-image48.png)

1. Erweitern Sie im Bereich **Daten** die Tabelle **Vertrieb**, und ziehen Sie dann das Feld **Umsatzbetrag** in das visuelle Balkendiagramm.

    ![](../images/dp500-create-a-dataflow-image49.png)


1. To sort the vertical axis, oben rechts vom Diagramm, wählen Sie die Auslassungspunkte, und wählen Sie dann **Achse sortieren** > **Month Offset Filter** aus.

    ![](../images/dp500-create-a-dataflow-image50.png)

1. Um den Monat offset zu sichern, sind die Werte chronologisch sortiert in der Bereich **Daten**, wählen Sie das **Month Offset Filter** Feld aus.

1. Auf dem Menüband **Column Tools** wählen Sie aus der Gruppe **Sortieren** **Sortieren**, und wählen Sie dann **Month Offset** aus.

    ![](../images/dp500-create-a-dataflow-image51.png)

1. Überprüfen Sie das visuelle Balkendiagramm, das jetzt chronologisch sortiert wird.

    *Der Standard Vorteil der Verwendung von Datumsoffsetspalten besteht darin, dass Berichte auf angepasste Weise nach relativen Datumsangaben filtern können. (Datenschnitte und Filter und filtert auch nach relativen Datums- und Uhrzeitperioden, aber dieses Verhalten kann nicht angepasst werden. Sie lassen auch keine Filterung nach Quartalen zu.)*

1. Speichern Sie die Power BI Desktop-Datei.

1. Schließen Sie Power BI Desktop.

### Anhalten des SQL-Pools

In dieser Aufgabe halten Sie den SQL-Pool an.

1. Rufen Sie in einem Webbrowser [https://portal.azure.com](https://portal.azure.com/) auf.

1. Suchen Sie den SQL-Pool.

1. Anhalten des SQL-Pools.
