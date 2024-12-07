---
lab:
  title: Erzwingen von Modellsicherheit
  module: Design and build tabular models
---

# Erzwingen von Modellsicherheit

## Überblick

**Die geschätzte Dauer dieses Labs beträgt 45 Minuten.**

In diesem Lab aktualisieren Sie ein vorab entwickeltes Datenmodell, um die Sicherheit zu erzwingen. Insbesondere sollten Vertriebsmitarbeiter im Adventure Works-Unternehmen nur in der Lage sein, Verkaufsdaten im Zusammenhang mit ihrer zugeordneten Vertriebsregion anzuzeigen.

In diesem Lab lernen Sie Folgendes:

- Erstellen von statischen Rollen.

- Erstellen von dynamische Rollen.

- Überprüfen von Rollen.

- Zuordnen von Sicherheitsprinzipals zu Datasetrollen.

## Erste Schritte

In dieser Übung bereiten Sie Ihre Umgebung vor.

### Klonen des Repositorys für diesen Kurs

1. Öffnen Sie im Startmenü die Eingabeaufforderung

    ![](../images/command-prompt.png)

1. Navigieren Sie im Eingabeaufforderungsfenster zum D-Laufwerk, indem Sie Folgendes eingeben:

    `d:` 

   Drücken Sie die Eingabetaste.

    ![](../images/command-prompt-2.png)


1. Geben Sie im Eingabeaufforderungsfenster den folgenden Befehl ein, um die Kursdateien herunterzuladen und in einem Ordner namens DP500 zu speichern.
    
    `git clone https://github.com/MicrosoftLearning/DP-500-Azure-Data-Analyst DP500`
   
1. Wenn das Repository geklont wurde, schließen Sie das Eingabeaufforderungsfenster. 
   
1. Öffnen Sie das D-Laufwerk im Datei-Explorer, um sicherzustellen, dass die Dateien heruntergeladen wurden.

### Öffnen Sie den Power BI Desktop

In dieser Aufgabe richten Sie den Power BI Desktop ein.

1. Um den Datei-Explorer zu öffnen, wählen Sie auf der Taskleiste den Shortcut **Datei-Explorer** aus.

2. Wechseln Sie zum Ordner **D:\DP500\Allfiles\09\Starter**.

3. Um eine vorentwickelte Power BI Desktop-Datei zu öffnen, doppelklicken Sie auf die Datei **Umsatzanalyse - Enforce model security.pbix**.

4. Wenn Sie noch nicht angemeldet sind, wählen Sie in der oberen rechten Ecke vom Power BI Desktop **Anmelden** aus. Verwenden Sie die Lab-Anmeldeinformationen, um den Anmeldevorgang abzuschließen.

    ![](../images/dp500-enforce-model-security-image2.png)

5. Um die Datei zu speichern, wählen Sie im Menüband **Datei** die Option **Speichern unter** aus.

6. Navigieren Sie im Fenster **Speichern unter** zum Ordner **D:\DP500\Allfiles\09\MySolution**.

7. Wählen Sie **Speichern**.

    *Sie aktualisieren die Power BI Desktop-Lösung, um die Sicherheit auf Zeilenebene zu erzwingen.*

### Anmelden beim Power BI-Dienst

Bei dieser Aufgabe melden Sie sich beim Power BI-Dienst an, starten eine Testlizenz und erstellen einen Arbeitsbereich.

*Wichtig: Wenn Sie Power BI bereits in Ihrer VM-Umgebung eingerichtet haben, fahren Sie mit der nächsten Aufgabe fort.*

1. Rufen Sie in einem Webbrowser [https://powerbi.com](https://powerbi.com/) auf.

2. Verwenden Sie die Lab-Anmeldeinformationen, um den Anmeldevorgang abzuschließen.

    *Wichtig: Sie müssen dieselben Anmeldeinformationen verwenden, die zum Anmelden von Power BI Desktop verwendet werden.*

3. Wählen Sie oben rechts das Profilsymbol und dann **Testversion starten** aus.

    ![](../images/dp500-enforce-model-security-image3.png)

4. Wenn Sie dazu aufgefordert werden, wählen Sie **Testversion starten** aus.

5. Führen Sie alle verbleibenden Aufgaben aus, um die Testeinrichtung abzuschließen.

    *Tipp: Die Power BI-Webbrowserumgebung wird als **Power BI-Dienst** bezeichnet.*

### Erstellen eines Arbeitsbereichs

In dieser Aufgabe erstellen Sie einen Arbeitsbereich.

1. Wählen Sie im Power BI-Dienst zum Erstellen eines Arbeitsbereichs im **Navigations **-bereich** (auf der linken Seite) Arbeitsbereiche** aus, und wählen Sie dann **Arbeitsbereich erstellen** aus.

    ![](../images/dp500-enforce-model-security-image5.png)


2. Geben Sie einen im Bereich **Einen Arbeitsbereich erstellen** (rechts), im Feld **Arbeitsbereichsname** einen Namen für den Arbeitsbereich ein.

    *Der Name des Arbeitsbereichs muss innerhalb des Mandanten eindeutig sein.*

    ![](../images/dp500-enforce-model-security-image6.png)

3. Wählen Sie **Speichern**.

    *Nach der Erstellung wird der Arbeitsbereich geöffnet. In einer späteren Übung veröffentlichen Sie ein Dataset in diesem Arbeitsbereich.*

### Überprüfen Sie das Datenmodell

In dieser Aufgabe überprüfen Sie das Datenmodell.

1. Auf dem Power BI Desktop links wechseln Sie zur Ansicht **Modell**.

    ![](../images/dp500-enforce-model-security-image8.png)


2. Verwenden Sie das Modelldiagramm, um den Modellentwurf zu überprüfen.

    ![](../images/dp500-enforce-model-security-image9.png)

    *Das Modell besteht aus sechs Dimensionstabellen und einer Faktentabelle. Die Faktentabelle **Umsatz** speichert Details zu Kundenaufträgen. Es handelt sich um ein klassisches Sternschema*.

3. Erweitern Sie die Tabelle **Vertriebsgebiet**.

    ![](../images/dp500-enforce-model-security-image10.png)

4. Beachten Sie, dass die Tabelle eine Spalte **Region** enthält.

    *Die Spalte **Region** speichert die Verkaufsregionen von Adventure Works. In dieser Organisation dürfen Vertriebsmitarbeiter nur Daten sehen, die sich auf ihr zugewiesenes Vertriebsgebiet beziehen. In dieser Übung werden Sie zwei verschiedene Sicherheitstechniken auf Zeilenebene implementieren, um Datenberechtigungen zu erzwingen.*

## Erstellen von statischen Rollen

In dieser Übung werden Sie statische Rollen erstellen und validieren und dann sehen, wie Sie Sicherheitsprinzipale den Datensatzrollen zuordnen.

### Erstellen von statischen Rollen

In dieser Aufgabe werden Sie zwei statische Rollen erstellen.

1. Wechseln Sie zur **Berichtansicht**.

    ![](../images/dp500-enforce-model-security-image11.png)

2. Beachten Sie in der Legende im visuellen Diagramm mit gestapelten Säulen (vorerst), dass es möglich ist, viele Bereiche anzuzeigen.

    ![](../images/dp500-enforce-model-security-image12.png)

    *Im Moment sieht das Diagramm noch sehr unübersichtlich aus. Das liegt daran, dass alle Regionen sichtbar sind. Wenn die Lösung die Sicherheit auf Zeilenebene erzwingt, sieht der Berichtsbenutzer*innen nur eine Region.*


3. Um eine Sicherheitsrolle hinzuzufügen, wählen Sie auf der Registerkarte des Menübands **Modellierung** in der Gruppe **Sicherheit** die Option **Rollen verwalten**.

    ![](../images/dp500-enforce-model-security-image13.png)

4. Wählen Sie im Fenster **Rollen verwalten** die Option **Erstellen** aus.

    ![](../images/dp500-enforce-model-security-image14.png)

5. Um die Rolle zu benennen, ersetzen Sie den markierten Text durch **Australien** und drücken dann die **Eingabetaste**.

    ![](../images/dp500-enforce-model-security-image15.png)


6. Wählen Sie in der Liste **Tabellen** für die Tabelle **Vertriebsgebiet** die Auslassungspunkte aus und wählen Sie dann **Filter hinzufügen** > **[Region]**.

    ![](../images/dp500-enforce-model-security-image16.png)

7. Ersetzen Sie im Feld **Tabellenfilter DAX-Ausdruck** **Wert** durch **Australien**.

    ![](../images/dp500-enforce-model-security-image17.png)

    *Dieser Ausdruck filtert die Spalte **Region** nach dem Wert **Australien**.*

8. Um eine andere Rolle zu erstellen, drücken Sie **Erstellen**.

    ![](../images/dp500-enforce-model-security-image18.png)


9. Wiederholen Sie die Schritte in dieser Aufgabe, um eine Rolle namens **Kanada** zu erstellen, die die Spalte **Region** nach **Kanada** filtert.

    ![](../images/dp500-enforce-model-security-image19.png)

    *In diesem Lab erstellen Sie nur die beiden Rollen. Berücksichtigen Sie jedoch, dass in einer realen Lösung eine Rolle für jede der 11 Adventure Works-Regionen erstellt werden muss.*

10. Wählen Sie **Speichern**.

    ![](../images/dp500-enforce-model-security-image20.png)

### Überprüfen von statischen Rollen

In dieser Aufgabe überprüfen Sie eine der statischen Rollen.

1. Wählen Sie auf der Registerkarte des Menübands **Modellierung** in der Gruppe **Sicherheit** die Option **Anzeigen als**.

    ![](../images/dp500-enforce-model-security-image21.png)


2. Wählen Sie im Fenster **Als Rollen anzeigen** die Rolle **Australien**.

    ![](../images/dp500-enforce-model-security-image22.png)

3. Klickan Sie auf **OK**.

    ![](../images/dp500-enforce-model-security-image23.png)

4. Auf der Berichtsseite sehen Sie, dass das gestapelte Säulendiagramm nur Daten für Australien anzeigt.

    ![](../images/dp500-enforce-model-security-image24.png)

5. Am oberen Rand des Berichts finden Sie ein gelbes Banner, das die erzwungene Rolle bestätigt.

    ![](../images/dp500-enforce-model-security-image25.png)

6. Um die Anzeige mit Hilfe der Rolle zu beenden, wählen Sie rechts neben dem gelben Banner **Anzeige beenden**.

    ![](../images/dp500-enforce-model-security-image26.png)

### Den Bericht veröffentlichen

In dieser Aufgabe wird der Bericht veröffentlicht.

1. Speichern Sie die Power BI Desktop-Datei.

    ![](../images/dp500-enforce-model-security-image27.png)
 

2. Klicken Sie zum Veröffentlichen Ihres Berichts auf der Registerkarte des Menübands auf **Start**, wählen Sie **Veröffentlichen** aus.

    ![](../images/dp500-enforce-model-security-image28.png)

3. Wählen Sie im Fenster **In Power BI veröffentlichen** Ihren Arbeitsbereich und wählen Sie dann **Auswählen**.

    ![](../images/dp500-enforce-model-security-image29.png)

4. Wenn die Veröffentlichung erfolgreich war, klicken Sie auf **OK**.

    ![](../images/dp500-enforce-model-security-image30.png)

### Konfigurieren der Sicherheit auf Zeilenebene (*optional*)

In dieser Aufgabe erfahren Sie, wie Sie die Sicherheit auf Zeilenebene im Power BI-Dienst konfigurieren. 

Diese Aufgabe setzt das Vorhandensein einer Sicherheitsgruppe **Salespeople_Australia** in dem Mandanten voraus, in dem Sie arbeiten. Diese Sicherheitsgruppe ist nicht automatisch im Mandanten vorhanden. Wenn Sie über Berechtigungen für Ihren Mandanten verfügen, können Sie die folgenden Schritte ausführen. Wenn Sie einen Mandanten verwenden, der Ihnen in der Schulung zur Verfügung gestellt wird, verfügen Sie nicht über die entsprechenden Berechtigungen zum Erstellen von Sicherheitsgruppen. Bitte lesen Sie die Aufgaben durch, beachten Sie jedoch, dass Sie diese nicht abschließen können, wenn die Sicherheitsgruppe nicht vorhanden ist. **Nach dem Durchlesen fahren Sie mit der Aufgabe "Bereinigung" fort.**

1. Wechseln Sie zum Power BI-Dienst (Webbrowser).

2. Auf der Landing Page des Arbeitsbereichs finden Sie den Datensatz **Verkaufsanalyse - Modellsicherheit erzwingen**.

    ![](../images/dp500-enforce-model-security-image31.png)


3. Bewegen Sie den Mauszeiger über den Datensatz, und wenn die Auslassungspunkte erscheinen, wählen Sie die Auslassungspunkte aus und wählen dann **Sicherheit** aus.

    ![](../images/dp500-enforce-model-security-image32.png)

    *Die Option **Sicherheit** unterstützt die Zuordnung von Microsoft Azure Active Directory (Azure AD) Sicherheitsprinzipalen, die Sicherheitsgruppen und Benutzer*innen umfassen.*

4. Beachten Sie links die Liste der Rollen, und dass **Australien** ausgewählt ist.

    ![](../images/dp500-enforce-model-security-image33.png)

5. Beginnen Sie im Feld **Mitglieder*innen** mit der Eingabe von **Salespeople_Australia**. 

    *Die Schritte 5 bis 8 dienen nur zu Demonstrationszwecken, da sie sich auf die Erstellung oder Existenz einer Salespeople_Australia Sicherheitsgruppe verlassen. Wenn Sie über Berechtigungen und das Wissen zum Erstellen von Sicherheitsgruppen verfügen, können Sie fortfahren. Fahren Sie andernfalls mit der Bereinigungsaufgabe fort.*

    ![](../images/dp500-enforce-model-security-image34.png)

6. Wählen Sie **Hinzufügen** aus.

    ![](../images/dp500-enforce-model-security-image35.png)

7. Um die Rollenzuordnung abzuschließen, wählen Sie **Speichern**.

    ![](../images/dp500-enforce-model-security-image36.png)

    *Jetzt werden alle Mitglieder der **Salespeople_Australia** Sicherheitsgruppe der Rolle **Australien** zugeordnet, wodurch der Datenzugriff auf die Anzeige nur australischer Verkäufe beschränkt wird.*

    *In einer realen Lösung sollte jede Rolle einer Sicherheitsgruppe zugeordnet werden.*

    *Dieser Entwurfsansatz ist einfach und effektiv, wenn Sicherheitsgruppen für jede Region vorhanden sind. Es gibt jedoch Nachteile: Es erfordert mehr Aufwand zum Erstellen und Einrichten. Außerdem muss das Dataset aktualisiert und erneut veröffentlicht werden, wenn neue Regionen eingebunden werden.*

    *In der nächsten Übung erstellen Sie eine dynamische Rolle, die datengesteuert ist. Dieser Entwurfsansatz kann dabei helfen, diese Nachteile zu beheben.*

8. Um zur Landing Page des Arbeitsbereichs zurückzukehren, wählen Sie im Bereich **Navigation** den Arbeitsbereich aus.


### Bereinigen Sie die Lösung

In dieser Aufgabe bereinigen Sie die Lösung, indem Sie das Dataset und die Modellrollen entfernen.

1. Um den Datensatz zu entfernen, bewegen Sie den Mauszeiger über den Datensatz. Wenn die Auslassungspunkte erscheinen, wählen Sie die Auslassungspunkte aus und wählen dann **Löschen**.

    ![](../images/dp500-enforce-model-security-image37.png)

    *Sie veröffentlichen ein überarbeitetes Dataset in der nächsten Übung erneut.*

2. Wenn Sie aufgefordert werden, den Löschvorgang zu bestätigen, klicken Sie auf **Löschen**.

    ![](../images/dp500-enforce-model-security-image38.png)

3. Wechseln Sie zurück zu Power BI Desktop.
 

4. Um die Sicherheitsrollen zu entfernen, wählen Sie auf der Registerkarte des Menübands **Modellierung** in der Gruppe **Sicherheit** die Option **Rollen verwalten**.

    ![](../images/dp500-enforce-model-security-image39.png)

5. Im Fenster **Rollen verwalten** wählen Sie **Löschen**, um die erste Rolle zu entfernen.

    ![](../images/dp500-enforce-model-security-image40.png)

6. Wenn Sie aufgefordert werden, den Löschvorgang zu bestätigen, drücken Sie **Ja, löschen**.

    ![](../images/dp500-enforce-model-security-image41.png)

7. Entfernen Sie auch die zweite Rolle.

8. Wählen Sie **Speichern**.

    ![](../images/dp500-enforce-model-security-image42.png)


## Erstellen einer dynamischen Rolle

In dieser Übung fügen Sie dem Modell eine Tabelle hinzu, erstellen und überprüfen eine dynamische Rolle und ordnen dann der Datasetrolle einen Sicherheitsprinzipal zu.

### Erstellen der Tabelle „Vertriebsmitarbeiter“

In dieser Aufgabe fügen Sie dem Modell die Tabelle **Vertriebsmitarbeiter** hinzu.

1. Wechseln Sie zur Ansicht**Modell**.

    ![](../images/dp500-enforce-model-security-image43.png)

2. Wählen Sie auf der Registerkarte des Menübands **Start** in der Gruppe **Abfragen** das Symbol **Daten transformieren** aus.

    ![](../images/dp500-enforce-model-security-image44.png)


3. Klicken Sie im Fenster **Power Query Editor** im Bereich **Queries** (auf der linken Seite) mit der rechten Maustaste auf die Abfrage **Kunde** und wählen Sie dann **Duplizieren**.

    ![](../images/dp500-enforce-model-security-image45.png)

    *Da die **Kundenabfrage** bereits Schritte zum Verbinden des Data Warehouse enthält, ist die Duplizierung eine effiziente Möglichkeit, die Entwicklung einer neuen Abfrage zu beginnen.*

4. Ersetzen Sie im Bereich **Abfrageeinstellungen** (auf der rechten Seite) im Feld **Name** den Text durch **Vertriebsmitarbeiter**.

    ![](../images/dp500-enforce-model-security-image46.png)


5. Klicken Sie in der Liste **Angewandte Schritte** mit der rechten Maustaste auf den Schritt **Entfernte andere Spalten** (dritter Schritt), und wählen Sie dann **Löschen bis Ende**.

    ![](../images/dp500-enforce-model-security-image47.png)

6. Wenn Sie aufgefordert werden, das Löschen des Schritts zu bestätigen, wählen Sie **Löschen**.

    ![](../images/dp500-enforce-model-security-image48.png)

7. Um Daten aus einer anderen Data-Warehouse-Tabelle zu beziehen, wählen Sie in der Liste **Angewandte Schritte** im Schritt **Navigation** (zweiter Schritt) das Zahnradsymbol (rechts).

    ![](../images/dp500-enforce-model-security-image49.png)

8. Wählen Sie im Fenster **Navigation** die Tabelle **DimEmployee**.

    ![](../images/dp500-enforce-model-security-image50.png)


9. Klickan Sie auf **OK**.

    ![](../images/dp500-enforce-model-security-image51.png)

10. Klicken Sie auf der Registerkarte **Start** des Menübands in der Gruppe **Spalten verwalten** auf das Symbol **Spalten auswählen**, um unnötige Spalten zu entfernen.

    ![](../images/dp500-enforce-model-security-image52.png)

11. Deaktivieren Sie im Fenster **Spalten auswählen** das Element **(Alle Spalten auswählen)**, um alle Spalten zu deaktivieren.

    ![](../images/dp500-enforce-model-security-image53.png)

12. Benennen Sie die folgenden drei Spalten um:

    - EmployeeKey

    - SalesTerritoryKey

    - EmailAddress

13. Klickan Sie auf **OK**.

    ![](../images/dp500-enforce-model-security-image54.png)

14. Um die Spalte **EmailAdresse** umzubenennen, doppelklicken Sie auf die Spaltenüberschrift **EmailAdresse**.

15. Ersetzen Sie den Text durch **UPN**, und drücken Sie dann die **Eingabetaste**.

    *UPN ist ein Akronym für User Principal Name. Die Werte in dieser Spalte entsprechen den Azure AD-Kontonamen.*

    ![](../images/dp500-enforce-model-security-image55.png)

16. Um die Tabelle in das Modell zu laden, wählen Sie auf der Registerkarte des Menübands **Start **das Symbol** &amp;Anwenden** aus.

    ![](../images/dp500-enforce-model-security-image56.png)

17. Nach dem Hinzufügen der Tabelle zum Modell ist zu beachten, dass automatisch eine Beziehung zur Tabelle **Vertriebsgebiet** erstellt wurde.

### Konfigurieren von Beziehungen

In dieser Aufgabe konfigurieren Sie die Eigenschaften der neuen Beziehung.

1. Klicken Sie mit der rechten Maustaste auf die Beziehung zwischen den Tabellen **Verkäufer** und **Verkaufsgebiet**, und wählen Sie dann **Eigenschaften**.

    ![](../images/dp500-enforce-model-security-image57.png)


2. Wählen Sie im Fenster **Beziehung bearbeiten** in der Dropdownliste **Kreuzfilterrichtung** die Option **Beide** aus.

3. Aktivieren Sie das Kontrollkästchen **Sicherheitsfilter in beide Richtungen anwenden**.

    ![](../images/dp500-enforce-model-security-image58.png)

     *Da es eine eins-zu-viele-Beziehung zwischen der Tabelle **Vertriebsgebiet** und der Tabelle **Vertriebsmitarbeiter*innen** gibt, werden Filter nur von der Tabelle **Vertriebsgebiet** zur Tabelle **Vertriebsmitarbeiter*inn** übertragen. Um die Weitergabe in die andere Richtung zu erzwingen, muss die Crossfilter-Richtung auf "beide" gesetzt werden.*

4. Klickan Sie auf **OK**.

    ![](../images/dp500-enforce-model-security-image59.png)

5. Um die Tabelle auszublenden, wählen Sie oben rechts in der Tabelle **Vertriebsmitarbeiter*innen** das Augensymbol.

    ![](../images/dp500-enforce-model-security-image60.png)

    *Der Zweck der Tabelle **Vertriebsmitarbeiter*innen** ist es, Datenberechtigungen zu erzwingen. Wenn sie ausgeblendet ist, sehen die Berichtsautoren und die Q&A-Erfahrung die Tabelle oder ihre Felder nicht.*
 

### Erstellen einer dynamischen Rolle

In dieser Aufgabe erstellen Sie eine dynamische Rolle, die Berechtigungen basierend auf Daten im Modell erzwingt.

1. Wechseln Sie zur **Berichtansicht**.

    ![](../images/dp500-enforce-model-security-image61.png)

2. Um eine Sicherheitsrolle hinzuzufügen, wählen Sie auf der Registerkarte des Menübands **Modellierung** in der Gruppe **Sicherheit** die Option **Rollen verwalten**.

    ![](../images/dp500-enforce-model-security-image62.png)

3. Wählen Sie im Fenster **Rollen verwalten** die Option **Erstellen** aus.

    ![](../images/dp500-enforce-model-security-image63.png)

4. Ersetzen Sie den markierten Text durch **Vertriebsmitarbeiter**, um die Rolle zu benennen.

    ![](../images/dp500-enforce-model-security-image64.png)

    *Diesmal muss nur eine Rolle erstellt werden.*

5. Fügen Sie der **UPN-Spalte** der Tabelle **Vertriebsmitarbeiter** einen Filter hinzu.

    ![](../images/dp500-enforce-model-security-image65.png)

6. Ersetzen Sie im Feld **Tabellenfilter DAX-Ausdruck** **"Wert"** durch **USERPRINCIPALNAME()**.

    ![](../images/dp500-enforce-model-security-image66.png)

    *Dieser Ausdruck filtert die Spalte **UPN** nach der Funktion USERPRINCIPALNAME, die den Benutzerprinzipalnamen (UPN) des/der authentifizierten Benutzer*in zurückgibt.*

    *Wenn die UPN die Tabelle **Vertriebsmitarbeiter*innen** filtert, filtert sie die Tabelle **Vertriebsgebiet**, die wiederum die Tabelle **Umsatz** filtert. Auf diese Weise sieht der authentifizierte Benutzende nur die Umsatzdaten für das ihm zugewiesene Gebiet.*

7. Wählen Sie **Speichern**.

    ![](../images/dp500-enforce-model-security-image67.png)

### Überprüfen der dynamischen Rolle

In dieser Aufgabe überprüfen Sie die dynamische Rolle.

1. Wählen Sie auf der Registerkarte des Menübands **Modellierung** in der Gruppe **Sicherheit** die Option **Anzeigen als**.

    ![](../images/dp500-enforce-model-security-image68.png)


2. Im Fenster **Als Rollen anzeigen** markieren Sie **Anderer Benutzer**, und geben Sie dann in das entsprechende Feld ein: **michael9@adventure-works.com**

    ![](../images/dp500-enforce-model-security-image69.png)

    *Zu Testzwecken ist **Anderer Benutzender** der Wert, der von der Funktion USERPRINCIPALNAME zurückgegeben wird. Beachten Sie, dass diese*r Vertriebsmitarbeiter*in der Region **Nordost** zugeordnet ist.*

3. Wählen Sie die Rolle **Salespeople** aus.

    ![](../images/dp500-enforce-model-security-image70.png)

4. Klickan Sie auf **OK**.

    ![](../images/dp500-enforce-model-security-image71.png)

5. Beachten Sie auf der Berichtsseite, dass das Diagramm mit gestapelten Säulen nur Daten für Nordosten anzeigt.

    ![](../images/dp500-enforce-model-security-image72.png)

6. Am oberen Rand des Berichts finden Sie ein gelbes Banner, das die erzwungene Rolle bestätigt.

    ![](../images/dp500-enforce-model-security-image73.png)


7. Um die Anzeige mit Hilfe der Rolle zu beenden, wählen Sie rechts neben dem gelben Banner **Anzeige beenden**.

    ![](../images/dp500-enforce-model-security-image74.png)

### Fertigstellen des Entwurfs

In dieser Aufgabe schließen Sie den Entwurf ab, indem Sie den Bericht veröffentlichen und eine Sicherheitsgruppe der Rolle zuordnen.

*Die Schritte in dieser Aufgabe sind bewusst kurz. Ausführliche Informationen finden Sie in den Aufgabenschritten der vorherigen Übung.*

1. Speichern Sie die Power BI Desktop-Datei.

    ![](../images/dp500-enforce-model-security-image75.png)

2. Veröffentlichen Sie den Bericht im Arbeitsbereich, den Sie am Anfang des Labs erstellt haben. 

3. Schließen Sie Power BI Desktop.

4. Wechseln Sie zum Power BI-Dienst (Webbrowser).

5. Gehen Sie zu den Sicherheitseinstellungen für den Datensatz **Vertriebsanalyse - Modellsicherheit erzwingen**.

6. Ordnen Sie der Sicherheitsgruppe **Vertriebsmitarbeiter*innen** die Rolle **Vertriebsmitarbeiter*innen** zu.

    ![](../images/dp500-enforce-model-security-image76.png)

    *Nun werden alle Mitglieder der Sicherheitsgruppe **Vertreibsmitarbeiter*in** der Rolle **Vertriebsmitarbeiter*in** zugeordnet. Sofern der authentifizierte Benutzer durch eine Zeile in der Tabelle **Verkäufer** repräsentiert wird, wird das zugewiesene Vertriebsgebiet zum Filtern der Umsatztabelle verwendet.*

    *Dieser Entwurfsansatz ist einfach und effektiv, wenn das Datenmodell die Werte für den Benutzerprinzipalnamen speichert. Wenn Vertriebsmitarbeiter hinzugefügt oder entfernt oder verschiedenen Vertriebsgebieten zugewiesen werden, funktioniert dieser Entwurfsansatz einfach.*
