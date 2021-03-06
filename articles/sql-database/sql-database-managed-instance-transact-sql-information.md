---
title: Azure SQL Database Managed Instance T-SQL-verschillen | Microsoft Docs
description: In dit artikel worden besproken die de T-SQL-verschillen tussen Azure SQL Database Managed Instance en SQL Server
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, bonova
manager: craigg
ms.date: 10/24/2018
ms.openlocfilehash: 6ffa09343560e4ac55b1fd62325fd4e3bd370848
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242174"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>Azure SQL Database Managed Instance T-SQL-verschillen van SQL Server

Azure SQL Database Managed Instance biedt extra compatibiliteit met on-premises SQL Server Database Engine. De meeste van de Database-Engine van SQL Server-functies worden ondersteund in het beheerde exemplaar. Omdat er nog steeds enkele verschillen in de syntaxis en het gedrag, wordt in dit artikel bevat een overzicht van en worden deze verschillen uitgelegd.

- [T-SQL-verschillen en niet-ondersteunde functies](#Differences)
- [Functies waarvoor verschillend gedrag in het beheerde exemplaar](#Changes)
- [Tijdelijke beperkingen en bekende problemen](#Issues)

## <a name="Differences"></a> T-SQL-verschillen van SQL Server

In deze sectie bevat een overzicht van belangrijke verschillen in T-SQL-syntaxis en het gedrag tussen Managed Instance en on-premises SQL Server Database Engine, evenals de niet-ondersteunde functies.

### <a name="always-on-availability"></a>Always On-beschikbaarheid

[Hoge beschikbaarheid](sql-database-high-availability.md) is gratis ingebouwd in de Managed Instance en kan niet worden bepaald door gebruikers. De volgende instructies worden niet ondersteund:

- [EINDPUNT MAKEN... VOOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
- [BESCHIKBAARHEIDSGROEP MAKEN](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
- [ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
- [DROP AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
- [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) -component van de [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql) instructie

### <a name="auditing"></a>Controleren

De belangrijkste verschillen tussen de SQL-controle in de Managed Instance, Azure SQL Database en SQL Server on-premises zijn:

- In het beheerde exemplaar SQL Audit werkt op het serverniveau en de winkels `.xel` bestanden in Azure blob storage-account.  
- In Azure SQL Database werkt SQL-controle op het databaseniveau van de.
- In on-premises SQL Server / virtuele machine, SQL-controle werkt op het serverniveau van de, maar gebeurtenissen worden opgeslagen in bestanden system/windows-gebeurtenislogboeken.  
  
XEvent-controle in het beheerde exemplaar biedt ondersteuning voor Azure blob storage-doelen. Bestands- en windows logboeken worden niet ondersteund.

De sleutel verschillen de `CREATE AUDIT` syntaxis voor controle naar Azure blob-opslag zijn:

- Een nieuwe syntaxis `TO URL` wordt geleverd en kunt u de URL van de Azure blob Storage-container op te geven waar `.xel` bestanden worden geplaatst
- De syntaxis van de `TO FILE` wordt niet ondersteund omdat het beheerde exemplaar heeft geen toegang tot Windows-bestandsshares.

Zie voor meer informatie:  

- [SERVERAUDIT MAKEN](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)  
- [ALTER SERVERAUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Controle](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="backup"></a>Backup

Beheerd exemplaar is automatische back-ups en waarmee gebruikers volledige database maken `COPY_ONLY` back-ups. Differentiële, het logboek en bestand momentopnameback-ups worden niet ondersteund.

- Managed Instance kunt back-up van een database alleen voor een Azure Blob Storage-account:
  - Alleen `BACKUP TO URL` wordt ondersteund
  - `FILE`, `TAPE`, en back-upapparaten worden niet ondersteund.  
- De meeste van de algemene `WITH` opties worden ondersteund
  - `COPY_ONLY` is verplicht
  - `FILE_SNAPSHOT` Niet ondersteund
  - Tapeopties: `REWIND`, `NOREWIND`, `UNLOAD`, en `NOUNLOAD` worden niet ondersteund
  - Logboek-specifieke opties: `NORECOVERY`, `STANDBY`, en `NO_TRUNCATE` worden niet ondersteund

De volgende beperkingen:  

- Managed Instance kunt back-up van een database naar een back-up met maximaal 32 striping, wat voldoende is voor de databases van maximaal 4 TB als back-compressie wordt gebruikt.
- Maximumgrootte van de back-up stripe is 195 GB (maximaal blobgrootte). Verhoog het aantal streepjes in de back-opdracht voor het verkleinen van de afzonderlijke stripe en binnen deze limiet blijven.

> [!TIP]
> Deze beperking on-premises back-up te omzeilen `DISK` in plaats van de back-up naar `URL`, back-upbestand voor blob-en zet vervolgens uploaden. Ondersteunt grotere bestanden herstellen, omdat een andere blob-type wordt gebruikt.  

Zie voor meer informatie over back-ups met behulp van T-SQL [back-up](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

### <a name="buffer-pool-extension"></a>De buffergroepuitbreiding

- [De extensie van de buffer](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) wordt niet ondersteund.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` wordt niet ondersteund. Zie [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="bulk-insert--openrowset"></a>Bulksgewijs invoegen / openrowset

Beheerd exemplaar geen toegang tot gedeelde bestanden en mappen, Windows, zodat de bestanden moeten worden geïmporteerd uit Azure blob-opslag:

- `DATASOURCE` is vereist in `BULK INSERT` opdracht tijdens het importeren van bestanden vanuit Azure blob storage. Zie [BULKSGEWIJS invoegen](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` is vereist in `OPENROWSET` wanneer u een inhoud van een bestand lezen uit Azure blob-opslag. Zie [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).

### <a name="certificates"></a>Certificaten

Managed Instance heeft geen toegang tot bestandsshares en Windows-mappen. Daarom zijn de volgende beperkingen van toepassing:

- `CREATE FROM`/`BACKUP TO` bestand wordt niet ondersteund voor certificaten
- `CREATE`/`BACKUP` certificaat van `FILE` / `ASSEMBLY` wordt niet ondersteund. Bestanden met persoonlijke sleutel kunnen niet worden gebruikt.  

Zie [certificaat maken](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) en [back-certificaat](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql).  
  
**Tijdelijke oplossing**: script certificaten/persoonlijk sleutelpaar, worden opgeslagen als .sql-bestanden en maken op basis van het binaire bestand:

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="clr"></a>CLR

Managed Instance heeft geen toegang tot bestandsshares en Windows-mappen. Daarom zijn de volgende beperkingen van toepassing:

- Alleen `CREATE ASSEMBLY FROM BINARY` wordt ondersteund. Zie [maken van de ASSEMBLY van binaire](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` wordt niet ondersteund. Zie [ASSEMBLY maken vanuit bestand](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` kan niet verwijzen naar bestanden. Zie [ALTER ASSEMBLY](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="compatibility-levels"></a>Compatibiliteitsniveaus

- Ondersteunde compatibiliteitsniveaus zijn: 100, 110, 120, 130, 140  
- Compatibiliteitsniveaus lager dan 100 worden niet ondersteund.
- Het standaardcompatibiliteitsniveau voor nieuwe databases is 140. Voor de herstelde databases blijft compatibiliteitsniveau ongewijzigd als er 100 en hoger.

Zie [ALTER databasecompatibiliteitsniveau](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="credential"></a>Referentie

Alleen Azure Key Vault en `SHARED ACCESS SIGNATURE` identiteiten worden ondersteund. Windows-gebruikers worden niet ondersteund.

Zie [maken referentie](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) en [ALTER referentie](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Cryptografische providers

Beheerd exemplaar heeft geen toegang tot bestanden, zodat de cryptografische providers kunnen niet worden gemaakt:

- `CREATE CRYPTOGRAPHIC PROVIDER` wordt niet ondersteund. Zie [maken CRYPTOGRAFISCHE PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` wordt niet ondersteund. Zie [ALTER CRYPTOGRAFISCHE PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="collation"></a>Sortering

Server-sortering wordt `SQL_Latin1_General_CP1_CI_AS` en kan niet worden gewijzigd. Zie [sorteringen](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="database-options"></a>Opties voor de database

- Meerdere logboekbestanden worden niet ondersteund.
- Objecten in het geheugen worden niet ondersteund in de categorie Algemeen gebruik-service.  
- Er is een limiet van 280 bestanden per exemplaar: maximaal 280 bestanden per database voor de overdracht. Zowel de gegevens en logboekbestanden worden geteld naar deze limiet.  
- Database mag geen bestandsgroepen met filestream-gegevens bevatten.  Herstellen mislukt als .bak bevat `FILESTREAM` gegevens.  
- Elk bestand is in Azure Premium storage geplaatst. I/o- en doorvoer per bestand afhankelijk van de grootte van elk afzonderlijk bestand, op dezelfde manier als voor Azure Premium Storage-schijven. Zie [prestaties Azure Premium-schijf](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)  

#### <a name="create-database-statement"></a>De instructie CREATE DATABASE

Hieronder vindt u `CREATE DATABASE` beperkingen:

- Bestanden en bestandsgroepen kan niet worden gedefinieerd.  
- `CONTAINMENT` optie wordt niet ondersteund.  
- `WITH`opties worden niet ondersteund.  
   > [!TIP]
   > Als tijdelijke oplossing gebruiken `ALTER DATABASE` nadat `CREATE DATABASE` databaseopties bestanden toe te voegen of om in te stellen containment instellen.  

- `FOR ATTACH` optie wordt niet ondersteund
- `AS SNAPSHOT OF` optie wordt niet ondersteund

Zie voor meer informatie, [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>Instructie ALTER DATABASE

Sommige bestandseigenschappen kunnen niet worden ingesteld of gewijzigd:

- Pad naar bestand kan niet worden opgegeven `ALTER DATABASE ADD FILE (FILENAME='path')` T-SQL-instructie. Verwijder `FILENAME` plaatst de bestanden van het script omdat beheerd exemplaar automatisch.  
- De bestandsnaam van het kan niet worden gewijzigd met behulp van `ALTER DATABASE` instructie.

De volgende opties zijn standaard ingesteld en kunnen niet worden gewijzigd:

- `MULTI_USER`
- `ENABLE_BROKER ON`
- `AUTO_CLOSE OFF`

De volgende opties kunnen niet worden gewijzigd:

- `AUTO_CLOSE`
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)`
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)`
- `DISABLE_BROKER`
- `EMERGENCY`
- `ENABLE_BROKER`
- `FILESTREAM`
- `HADR`
- `NEW_BROKER`
- `OFFLINE`
- `PAGE_VERIFY`
- `PARTNER`
- `READ_ONLY`
- `RECOVERY BULK_LOGGED`
- `RECOVERY_SIMPLE`
- `REMOTE_DATA_ARCHIVE`  
- `RESTRICTED_USER`
- `SINGLE_USER`
- `WITNESS`

Wijzig de naam wordt niet ondersteund.

Zie voor meer informatie, [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="database-mirroring"></a>Databasespiegeling

Databasespiegeling wordt niet ondersteund.

- `ALTER DATABASE SET PARTNER` en `SET WITNESS` opties worden niet ondersteund.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` wordt niet ondersteund.

Zie voor meer informatie, [ALTER DATABASE SET PARTNER en SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) en [EINDPUNT maken... VOOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="dbcc"></a>DBCC

Niet-gedocumenteerde DBCC-instructies die zijn ingeschakeld in SQL Server worden niet ondersteund in het beheerde exemplaar.

- `Trace Flags` worden niet ondersteund. Zie [traceermarkeringen](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` wordt niet ondersteund. Zie [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` wordt niet ondersteund. Zie [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Gedistribueerde transacties

Geen van beide MSDTC noch [elastische transacties](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-transactions-overview) worden momenteel ondersteund in het beheerde exemplaar.

### <a name="extended-events"></a>Uitgebreide gebeurtenissen

Sommige Windows-specifieke doelen voor XEvents worden niet ondersteund:

- `etw_classic_sync target` wordt niet ondersteund. Store `.xel` bestanden in Azure blob-opslag. Zie [etw_classic_sync doel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etwclassicsynctarget-target).
- `event_file target`wordt niet ondersteund. Store `.xel` bestanden in Azure blob-opslag. Zie [event_file doel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#eventfile-target).

### <a name="external-libraries"></a>Externe bibliotheken

In-database R en Python externe tapewisselaars worden nog niet ondersteund. Zie [SQL servermachine Learning-Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FILESTREAM en bestandstabel

- FILESTREAM-gegevens wordt niet ondersteund.
- Database geen bestandsgroepen met `FILESTREAM` gegevens
- `FILETABLE` wordt niet ondersteund
- Tabellen geen `FILESTREAM` typen
- De volgende functies worden niet ondersteund:
  - `GetPathLocator()`
  - `GET_FILESTREAM_TRANSACTION_CONTEXT()`
  - `PathName()`
  - `GetFileNamespacePat)`
  - `FileTableRootPath()`

Zie voor meer informatie, [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) en [Bestandstabellen](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Semantische zoekopdrachten in volledige tekst

[Semantisch zoeken](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) wordt niet ondersteund.

### <a name="linked-servers"></a>Gekoppelde servers

Gekoppelde servers in het beheerde exemplaar ondersteuning voor een beperkt aantal doelen:

- Doelen ondersteund: SQL Server en SQL-Database
- Niet ondersteund doelen: bestanden, Analysis Services en andere RDBMS.

Bewerkingen

- Cross-instance schrijftransacties worden niet ondersteund.
- `sp_dropserver` wordt ondersteund voor het verwijderen van een gekoppelde server. Zie [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- `OPENROWSET` de functie kan worden gebruikt voor het uitvoeren van query's alleen op SQL Server-exemplaren (ofwel beheerd, on-premises of in virtuele Machines). Zie [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- `OPENDATASOURCE` de functie kan worden gebruikt voor het uitvoeren van query's alleen op SQL Server-exemplaren (ofwel beheerd, on-premises of in virtuele machines). Alleen `SQLNCLI`, `SQLNCLI11`, en `SQLOLEDB` waarden als provider worden ondersteund. Bijvoorbeeld: `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Zie [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).

### <a name="logins--users"></a>Aanmeldingen / gebruikers

- SQL-aanmeldingen die zijn gemaakt `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY`, en `FROM SID` worden ondersteund. Zie [maken aanmelding](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Windows-aanmeldingen die zijn gemaakt met `CREATE LOGIN ... FROM WINDOWS` syntaxis worden niet ondersteund.
- Azure Active Directory (Azure AD)-gebruiker die heeft gemaakt van het exemplaar heeft [onbeperkte beheerdersbevoegdheden](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#unrestricted-administrative-accounts).
- Niet-Azure Active Directory (Azure AD) op databaseniveau beheerders kunnen worden gemaakt met `CREATE USER ... FROM EXTERNAL PROVIDER` syntaxis. Zie [gebruiker maken... VAN DE EXTERNE PROVIDER](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#non-administrator-users)

### <a name="polybase"></a>PolyBase

Externe tabellen verwijzen naar de bestanden in HDFS of Azure blob-opslag worden niet ondersteund. Zie voor meer informatie over Polybase [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replicatie

Replicatie is beschikbaar voor openbare preview-versie op Managed Instance. Zie voor meer informatie over replicatie [SQL Server-replicatie](http://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).

### <a name="restore-statement"></a>Instructie herstellen

- Ondersteunde syntaxis
  - `RESTORE DATABASE`
  - `RESTORE FILELISTONLY ONLY`
  - `RESTORE HEADER ONLY`
  - `RESTORE LABELONLY ONLY`
  - `RESTORE VERIFYONLY ONLY`
- Niet-ondersteunde syntaxis
  - `RESTORE LOG ONLY`
  - `RESTORE REWINDONLY ONLY`
- Bron  
  - `FROM URL` (Azure blob-opslag) is alleen ondersteunde optie.
  - `FROM DISK`/`TAPE`/ back-upapparaat wordt niet ondersteund.
  - Back-upsets worden niet ondersteund.
- `WITH` opties worden niet ondersteund (geen `DIFFERENTIAL`, `STATS`, enz.)
- `ASYNC RESTORE` -Herstel wordt voortgezet zelfs als client-verbinding wordt verbroken. Als de verbinding wordt verbroken, kunt u controleren `sys.dm_operation_status` weergeven voor de status van een herstelbewerking uit (evenals voor het maken en -neerzetten-database). Zie [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).  

De volgende databaseopties zijn ingesteld/genegeerd en kunnen later worden gewijzigd:  

- `NEW_BROKER` (als broker is niet ingeschakeld in .bak-bestand)  
- `ENABLE_BROKER` (als broker is niet ingeschakeld in .bak-bestand)  
- `AUTO_CLOSE=OFF` (als een database in een bestand met .bak `AUTO_CLOSE=ON`)  
- `RECOVERY FULL` (als een database in een bestand met .bak `SIMPLE` of `BULK_LOGGED` herstelmodus)
- Geoptimaliseerd voor geheugen-bestandsgroep is toegevoegd en XTP genoemd wanneer dit niet in het bronbestand .bak  
- Eventuele bestaande geoptimaliseerd voor geheugen-bestandsgroep is gewijzigd in XTP  
- `SINGLE_USER` en `RESTRICTED_USER` opties worden geconverteerd naar `MULTI_USER`

De volgende beperkingen:  

- `.BAK` bestanden met meerdere back-upsets kunnen niet worden hersteld.
- `.BAK` bestanden met meerdere logboekbestanden kunnen niet worden hersteld.
- Herstellen mislukt als .bak bevat `FILESTREAM` gegevens.
- Back-ups van databases met actieve In-memory-objecten die kunnen niet op dit moment worden hersteld.  
- Back-ups met databases wanneer op een bepaald moment In-Memory objecten bestaat kunnen niet op dit moment worden hersteld.
- Back-ups van databases in de modus alleen-lezen die kunnen niet op dit moment worden hersteld. Deze beperking wordt binnenkort verwijderd.

Zie voor meer informatie over de Restore-instructies [herstellen instructies](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Service broker

Cross-exemplaar van service broker wordt niet ondersteund:

- `sys.routes` -Voorwaarde: adres selecteren in sys.routes. Adres moet lokaal op elke route. Zie [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE` -u kunt geen `CREATE ROUTE` met `ADDRESS` dan `LOCAL`. Zie [maken ROUTE](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE` kan geen `ALTER ROUTE` met `ADDRESS` dan `LOCAL`. Zie [ALTER ROUTE](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  

### <a name="service-key-and-service-master-key"></a>De hoofdsleutel van sleutels en service-service

- [Back-up van de hoofdsleutel](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) (beheerd door SQL Database-service) wordt niet ondersteund
- [Restore master key](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) (beheerd door SQL Database-service) wordt niet ondersteund
- [Back-up van service master key](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) (beheerd door SQL Database-service) wordt niet ondersteund
- [Service master key restore](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) (beheerd door SQL Database-service) wordt niet ondersteund

### <a name="stored-procedures-functions-triggers"></a>Opgeslagen procedures, functies, triggers

- `NATIVE_COMPILATION` wordt momenteel niet ondersteund.
- De volgende [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) opties worden niet ondersteund:
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `max text repl size`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` wordt niet ondersteund. Zie [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` wordt niet ondersteund. Zie [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` worden niet ondersteund, met inbegrip van `sp_addextendedproc`  en `sp_dropextendedproc`. Zie [uitgebreide opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql)
- `sp_attach_db`, `sp_attach_single_file_db`, en `sp_detach_db` worden niet ondersteund. Zie [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), en [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).
- `sp_renamedb` wordt niet ondersteund. Zie [sp_renamedb](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-renamedb-transact-sql).

### <a name="sql-server-agent"></a>SQL Server Agent

- SQL Agent-instellingen zijn alleen-lezen. Procedure `sp_set_agent_properties` wordt niet ondersteund in het beheerde exemplaar.  
- Taken
  - T-SQL-taakstappen worden ondersteund.
  - De volgende replicatietaken worden ondersteund:
    - Transactie-log reader.  
    - Een momentopname.
    - Distributor
  - SSIS-taakstappen worden ondersteund
  - Andere typen taakstappen worden momenteel niet ondersteund, met inbegrip van:
    - Samenvoegen van stap in logboektaak replicatie wordt niet ondersteund.  
    - Lezer van de wachtrij wordt niet ondersteund.  
    - Opdrachtshell is nog niet ondersteund.
  - Beheerd exemplaar heeft geen toegang tot externe bronnen (bijvoorbeeld netwerkshares via robocopy).  
  - PowerShell is nog niet ondersteund.
  - Analyseservices worden niet ondersteund.
- Meldingen worden gedeeltelijk ondersteund
- E-mailmelding wordt ondersteund, is vereist voor het configureren van een Database-e-mailprofiel. Er mag slechts één database-e-mailprofiel en moet worden aangeroepen `AzureManagedInstance_dbmail_profile` in openbare preview-versie (tijdelijke beperking).  
  - Pager wordt niet ondersteund.  
  - Net Send wordt niet ondersteund.
  - Waarschuwingen worden nog niet niet ondersteund.
  - Proxy's worden niet ondersteund.  
- Gebeurtenislogboek wordt niet ondersteund.

De volgende functies worden momenteel niet ondersteund, maar in de toekomst worden ingeschakeld:

- Proxy's
- Plannen van taken op niet-actieve CPU
- Agent in-/ uitschakelen
- Waarschuwingen

Zie voor meer informatie over SQL Server Agent [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).

### <a name="tables"></a>Tabellen

Het volgende wordt niet ondersteund:

- `FILESTREAM`
- `FILETABLE`
- `EXTERNAL TABLE`
- `MEMORY_OPTIMIZED`  

Zie voor meer informatie over het maken en wijzigen van tabellen [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) en [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="Changes"></a> Gedragswijzigingen

De volgende variabelen, taken en weergaven kunt u verschillende resultaten retourneren:

- `SERVERPROPERTY('EngineEdition')` retourneert de waarde 8. Deze eigenschap identificatie unieke van Managed Instance. Zie [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` retourneert NULL, omdat het concept van instantie als deze bestaat voor SQL Server niet van toepassing is op Managed Instance. Zie [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` retourneert volledige DNS-verbindbaarheid'-naam, bijvoorbeeld Mijn-managed-instance.wcus17662feb9ce98.database.windows.net. Zie [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` -retourneert 'koppelbaar' DNS-naam, zoals volledige `myinstance.domain.database.windows.net` voor eigenschappen 'name' en 'data_source'. Zie [SYS. SERVERS](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` retourneert NULL, omdat het concept van de service als deze bestaat voor SQL Server niet van toepassing is op Managed Instance. Zie [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` wordt ondersteund. Retourneert NULL als AAD-aanmelding niet in sys.syslogins is. Zie [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` wordt niet ondersteund. Retourneert onjuiste gegevens (tijdelijke bekend probleem). Zie [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql).
- `GETDATE()` en andere functies van de ingebouwde datum/tijd retourneert altijd tijd in UTC-tijdzone. Zie [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a> Bekende problemen en beperkingen

### <a name="tempdb-size"></a>Grootte van TEMPDB

`tempdb` is opgesplitst in 12 bestanden met de maximale grootte van 14 GB per bestand. Deze maximale grootte per bestand kan niet worden gewijzigd en nieuwe bestanden kunnen niet worden toegevoegd aan `tempdb`. Deze beperking wordt binnenkort verwijderd. Aantal query's mogelijk een fout geretourneerd als ze nodig hebben meer dan 168 GB in `tempdb`.

### <a name="exceeding-storage-space-with-small-database-files"></a>Meer dan opslagruimte met kleine databasebestanden

Elk beheerd exemplaar met maximaal 35 TB opslag is gereserveerd voor Azure Premium-schijfruimte en elk databasebestand wordt geplaatst op een afzonderlijke fysieke schijf. Schijfgrootten is 128 GB, 256 GB, 512 GB, 1 TB of 4 TB. Ongebruikte ruimte op schijf is niet in rekening gebracht, maar de totale som van Azure Premium Disk-groottes mag niet meer dan 35 TB. In sommige gevallen een beheerd exemplaar dat niet 8 TB in totaal hoeft kan groter zijn dan de 35 TB Azure de limiet voor opslaggrootte, vanwege een interne fragmentatie.

Voor een beheerd exemplaar kan bijvoorbeeld een bestand 1,2 TB in grootte dat wordt geplaatst op een schijf met 4 TB en 248 bestanden 1 GB modules grootte die op afzonderlijke 128 GB schijven worden geplaatst. In dit voorbeeld:

- de grootte van de totale schijfruimte is 1 x 4 TB + 248 x 128 GB = 35 TB.
- de totale gereserveerde ruimte voor databases op het exemplaar is 1 x-1,2 TB + 248 x 1 GB = 1,4 TB.

Dit ziet u dat onder bepaalde omstandigheden, vanwege een zeer specifieke distributie van bestanden, een beheerd exemplaar de 35 TB gereserveerd voor de gekoppelde Azure Premium-schijf wanneer u niet verwacht het dat mogelijk mogelijk bereikt.

In dit voorbeeld bestaande databases blijven werken en zonder problemen kan groeien zolang er nieuwe bestanden worden niet toegevoegd. Echter nieuwe databases kunnen niet worden gemaakt of hersteld omdat er niet voldoende ruimte voor nieuwe schijfstations, zelfs als de totale grootte van alle databases niet aan de grootte van het exemplaar komt. De fout die wordt geretourneerd is in dat geval niet wissen.

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>Onjuiste configuratie van de SAS-sleutel tijdens de database herstellen

`RESTORE DATABASE` die leest .bak-bestand kan worden voortdurend opnieuw wordt geprobeerd om te lezen .bak-bestand en de geretourneerde fout na een lange periode als Shared Access Signature in `CREDENTIAL` is onjuist. UPSETS worden uitgevoerd vóór het herstellen van een database om er zeker van te zijn dat de SAS-sleutel juist is.
Zorg ervoor dat u de voorloopspaties verwijderen `?` van de SAS-sleutel die wordt gegenereerd met behulp van Azure portal.

### <a name="tooling"></a>Hulpprogramma 's

SQL Server Management Studio en SQL Server Data Tools mogelijk enkele problemen bij het openen van Managed Instance. Alle hulpprogramma's problemen verholpen voordat deze algemeen beschikbaar.

### <a name="incorrect-database-names-in-some-views-logs-and-messages"></a>Onjuiste databasenamen in bepaalde weergaven, logboeken en berichten

Verschillende systeemweergaven, prestatiemeteritems, foutberichten, XEvents en fouten in logboekvermeldingen weergegeven GUID database-id's in plaats van de werkelijke databasenamen. Vertrouw niet op deze GUID-id's omdat ze zou worden vervangen door de werkelijke databasenamen in de toekomst.

### <a name="database-mail-profile"></a>Database-e-mailprofiel

Er mag slechts één database-e-mailprofiel en moet worden aangeroepen `AzureManagedInstance_dbmail_profile`. Dit is een tijdelijke beperking die wordt binnenkort verwijderd.

### <a name="error-logs-are-not-persisted"></a>Foutenlogboeken zijn niet-persistente

Foutenlogboeken die beschikbaar in het beheerde exemplaar zijn niet permanent en de grootte is niet opgenomen in de limiet voor maximale opslag. Foutenlogboeken mogelijk in het geval van failover automatisch worden gewist.

### <a name="error-logs-are-verbose"></a>Foutenlogboeken zijn uitgebreide

Beheerd exemplaar plaatst uitgebreide informatie in de foutenlogboeken en veel van deze zijn niet relevant. De hoeveelheid gegevens in foutenlogboeken wordt in de toekomst worden verlaagd.

**Tijdelijke oplossing**: gebruik een aangepaste procedure voor het lezen van foutenlogboeken die bepaalde niet-relevante items filter-out. Zie voor meer informatie, [Azure SQL database Managed Instance-sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/).

### <a name="transaction-scope-on-two-databases-within-the-same-instance-is-not-supported"></a>Transactiebereik op twee databases binnen dezelfde instantie wordt niet ondersteund

`TransactionScope` klasse in .net werkt niet als de twee query's worden verzonden naar de twee databases binnen hetzelfde exemplaar onder de dezelfde transactie:

```C#
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

Hoewel deze code met gegevens in dezelfde instantie werkt vereist het MSDTC.

**Tijdelijke oplossing**: Gebruik [SqlConnection.ChangeDatabase(String)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) andere database gebruiken in de verbindingscontext in plaats van twee verbindingen.

### <a name="clr-modules-and-linked-servers-sometime-cannot-reference-local-ip-address"></a>CLR-modules en gekoppelde servers enige tijd kunnen niet verwijzen naar een lokaal IP-adres

De IP-adres van het lokale exemplaar kunnen niet worden omgezet in CLR-modules geplaatst in de Managed Instance en gekoppelde servers/gedistribueerde query's die verwijzen naar huidige instantie enige tijd opnieuw uit. Dit is een tijdelijke fout.

**Tijdelijke oplossing**: context-verbindingen indien mogelijk in CLR-module gebruiken.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Managed Instance [wat is een beheerd exemplaar?](sql-database-managed-instance.md)
- Voor een functies en van de vergelijkingslijst, Zie [algemene SQL-functies](sql-database-features.md).
- Zie voor een snelstart u voor het maken van een nieuwe Managed Instance, [het maken van een beheerd exemplaar](sql-database-managed-instance-get-started.md).
