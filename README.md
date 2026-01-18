# OpenSimulator-PHP-Data

## Übersicht

Dieses Modul stellt eine universelle PHP-Klasse (`RobustDB`) zur Verfügung, mit der du auf alle Tabellen der OpenSim Robust-Datenbank zugreifen kannst. Die Klasse kapselt die Datenbankverbindung und bietet Methoden zum Lesen, Schreiben, Aktualisieren und Löschen von Einträgen. Sie ist so gestaltet, dass sie in anderen PHP-Skripten einfach eingebunden und genutzt werden kann.

---

## Voraussetzungen

- PHP 7.4 oder neuer
- PDO-Extension für MySQL
- Eine funktionierende Datenbankverbindung (siehe `datainc.php`)

---

## Einbindung und Nutzung

1. **Einbinden der Datei**

```php
require_once __DIR__ . '/data.php';
```

1. **Verbindung zur Datenbank herstellen**

Am einfachsten nutzt du die vorhandene Verbindung aus `datainc.php`:

```php
$db = new RobustDB('datainc');
```

Alternativ kannst du eigene Zugangsdaten angeben:

```php
$db = new RobustDB('localhost', 'robust', 'benutzer', 'passwort');
```

1. **Tabellenstruktur**

Alle Tabellen und Spalten der Robust-Datenbank sind im Array `$ROBUST_TABLES` verfügbar:

```php
global $ROBUST_TABLES;
print_r($ROBUST_TABLES['assets']); // Gibt alle Spalten der Tabelle 'assets' aus
```

---

## CRUD-Operationen (Beispiele)

### Alle Einträge einer Tabelle lesen

```php
$users = $db->getAll('UserAccounts');
```

### Einzelnen Eintrag lesen

```php
$user = $db->getById('UserAccounts', 'PrincipalID', 'uuid-hier-einfügen');
```

### Eintrag einfügen

```php
$neu = [
    'PrincipalID' => 'neue-uuid',
    'ScopeID' => 'scope-uuid',
    'FirstName' => 'Max',
    'LastName' => 'Mustermann',
    // ...weitere Felder...
];
$db->insert('UserAccounts', $neu);
```

### Eintrag aktualisieren

```php
$db->update('UserAccounts', 'PrincipalID', 'uuid-hier', [ 'FirstName' => 'Erika' ]);
```

### Eintrag löschen

```php
$db->delete('UserAccounts', 'PrincipalID', 'uuid-hier');
```

### Eigene SQL-Abfrage

```php
$result = $db->query('SELECT * FROM regions WHERE regionName LIKE :name', ['name' => '%Test%']);
```

---

## Hinweise

- Die Datenbankstruktur wird nicht verändert, alle Operationen erfolgen auf den bestehenden Tabellen.
- Für die Tabelle `assets` wird das BLOB-Feld `data` bei der Anzeige im Testskript automatisch ausgeblendet.
- Fehler bei der Verbindung oder Abfrage werden als Exception geworfen.

---

## Testskript

Mit `datatest.php` kannst du testen, ob der Zugriff auf alle Tabellen funktioniert. Es wird aus jeder Tabelle der erste Eintrag angezeigt (bei `assets` ohne das BLOB-Feld).

---
