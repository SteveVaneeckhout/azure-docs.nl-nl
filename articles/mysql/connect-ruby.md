---
title: Verbinding maken met Azure Database for MySQL via Ruby
description: Deze snelstartgids bevat enkele voorbeelden van Ruby-code die u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit een Azure Database voor MySQL.
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 02/28/2018
ms.openlocfilehash: cfaf6cc5b93ee80017a8fe5634c7afa7b9ce0ccf
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986794"
---
# <a name="azure-database-for-mysql-use-ruby-to-connect-and-query-data"></a>Azure Database voor MySQL: Ruby gebruiken om verbinding te maken en gegevens op te vragen
In deze snelstartgids ziet u hoe u vanuit de platformen Windows, Ubuntu Linux en Mac met behulp van een [Ruby](https://www.ruby-lang.org)-toepassing en de [mysql2](https://rubygems.org/gems/mysql2)-gem verbinding maakt met een Azure Database voor MySQL. U ziet hier hoe u SQL-instructies gebruikt om gegevens in de database op te vragen, in te voegen, bij te werken en te verwijderen. In dit artikel wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met behulp van Ruby, maar geen ervaring hebt met het werken met Azure Database voor MySQL.

## <a name="prerequisites"></a>Vereisten
In deze snelstartgids worden de resources die in een van deze handleidingen zijn gemaakt, als uitgangspunt gebruikt:
- [Een Azure-database voor een MySQL-server maken met behulp van Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Een Azure-database voor een MySQL-server maken met behulp van Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a>Ruby installeren
Installeer Ruby, Gem en de MySQL2-bibliotheek op de computer. 

### <a name="windows"></a>Windows
1. Download en installeer versie 2.3 van [Ruby](https://rubyinstaller.org/downloads/).
2. Open een nieuw opdrachtprompt (cmd) vanuit het menu Start.
3. Wijzig de map in de Ruby-map voor versie 2.3. `cd c:\Ruby23-x64\bin`
4. Test de Ruby-installatie door met de opdracht `ruby -v` te controleren welke versie er is geïnstalleerd.
5. Test de Gem-installatie door met de opdracht `gem -v` te controleren welke versie er is geïnstalleerd.
6. Bouw de MySQL2-module voor Ruby. Gebruik hiervoor Gem en voer de opdracht `gem install mysql2` uit.

### <a name="macos"></a>MacOS
1. Installeer Ruby met Homebrew. Voer daarvoor de opdracht `brew install ruby` uit. Zie de Ruby-[documentatie voor installatie](https://www.ruby-lang.org/en/documentation/installation/#homebrew) voor meer installatieopties.
2. Test de Ruby-installatie door met de opdracht `ruby -v` te controleren welke versie er is geïnstalleerd.
3. Test de Gem-installatie door met de opdracht `gem -v` te controleren welke versie er is geïnstalleerd.
4. Bouw de MySQL2-module voor Ruby. Gebruik hiervoor Gem en voer de opdracht `gem install mysql2` uit.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Installeer Ruby door de opdracht `sudo apt-get install ruby-full` uit te voeren. Zie de Ruby-[documentatie voor installatie](https://www.ruby-lang.org/en/documentation/installation/) voor meer installatieopties.
2. Test de Ruby-installatie door met de opdracht `ruby -v` te controleren welke versie er is geïnstalleerd.
3. Installeer de nieuwste updates voor Gem door de opdracht `sudo gem update --system` uit te voeren.
4. Test de Gem-installatie door met de opdracht `gem -v` te controleren welke versie er is geïnstalleerd.
5. Installeer gcc, make en andere buildhulpprogramma's door de opdracht `sudo apt-get install build-essential` uit te voeren.
6. Installeer de clientontwikkelaarsbibliotheken van MySQL door de opdracht `sudo apt-get install libmysqlclient-dev` uit te voeren.
7. Bouw de MySQL2-module voor Ruby. Gebruik hiervoor Gem en voer de opdracht `sudo gem install mysql2` uit.

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure Database voor MySQL. U hebt de volledig gekwalificeerde servernaam en aanmeldingsreferenties nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu aan de linkerkant in Azure Portal op **Alle resources** en zoek naar de server die u hebt gemaakt (bijvoorbeeld **mydemoserver**).
3. Klik op de servernaam.
4. Ga naar het venster **Overzicht** van de server en noteer de **Servernaam** en de **Aanmeldingsnaam van de serverbeheerder**. Als u uw wachtwoord vergeet, kunt u het wachtwoord in dit venster opnieuw instellen.
 ![Naam van Azure Database voor MySQL-server](./media/connect-ruby/1_server-overview-name-login.png)

## <a name="run-ruby-code"></a>Ruby-code uitvoeren 
1. Plak de Ruby-code uit de onderstaande secties in tekstbestanden en sla vervolgens de bestanden op in een projectmap met de bestandsextensie .rb, zoals `C:\rubymysql\createtable.rb` of `/home/username/rubymysql/createtable.rb`.
2. Voor het uitvoeren van de code opent u de opdrachtprompt of bash-shell. Ga naar de projectmap `cd rubymysql`
3. Typ vervolgens de Ruby-opdracht, gevolgd door de bestandsnaam, zoals `ruby createtable.rb`, om de toepassing uit te voeren.
4. Als de Ruby-toepassing zich niet in uw padomgevingsvariabele bevindt, moet u in Windows mogelijk het volledige pad, zoals `"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`, gebruiken om de knooppunttoepassing te starten.

## <a name="connect-and-create-a-table"></a>Verbinding maken en een tabel maken
Gebruik de volgende code om een tabel te verbinden en te maken met behulp van de SQL-instructie **CREATE TABLE**, gevolgd door de SQL-instructie **INSERT INTO** om rijen in de tabel toe te voegen.

De code gebruikt een [mysql2::client](https://www.rubydoc.info/gems/mysql2/0.4.8) .new()-methode om verbinding te maken met Azure Database voor MySQL. Vervolgens wordt de methode [query()](https://www.rubydoc.info/gems/mysql2/0.4.8#Usage) meerdere keren aangeroepen om de opdrachten DROP, CREATE TABLE en INSERT INTO uit te voeren. Vervolgens wordt methode [close()](https://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) aangeroepen om de verbinding vóór het sluiten te verbreken.

Vervang de tekenreeksen `host`, `database`, `username` en `password` door uw eigen waarden. 
```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('mydemoserver.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@mydemoserver')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Drop previous table of same name if one exists
    client.query('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    client.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    client.query("INSERT INTO inventory VALUES(1, 'banana', 150)")
    client.query("INSERT INTO inventory VALUES(2, 'orange', 154)")
    client.query("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="read-data"></a>Gegevens lezen
Gebruik de volgende code om verbinding te maken en de gegevens te lezen met behulp van de SQL-instructie **SELECT**. 

De code gebruikt een [mysql2::client class.new()](https://www.rubydoc.info/gems/mysql2/0.4.8)-methode om verbinding te maken met Azure Database voor MySQL. Daarna wordt de methode [query()](https://www.rubydoc.info/gems/mysql2/0.4.8#Usage) aangeroepen om de SELECT-opdrachten uit te voeren. Vervolgens wordt methode [close()](https://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) aangeroepen om de verbinding vóór het sluiten te verbreken.

Vervang de tekenreeksen `host`, `database`, `username` en `password` door uw eigen waarden. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('mydemoserver.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@mydemoserver')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Read data
    resultSet = client.query('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end
    puts 'Read ' + resultSet.count.to_s + ' row(s).'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="update-data"></a>Gegevens bijwerken
Gebruik de volgende code om verbinding te maken en de gegevens bij te werken met behulp van de SQL-instructie **UPDATE**.

De code gebruikt een [mysql2::client](https://www.rubydoc.info/gems/mysql2/0.4.8) .new()-methode om verbinding te maken met Azure Database voor MySQL. Daarna wordt de methode [query()](https://www.rubydoc.info/gems/mysql2/0.4.8#Usage) aangeroepen om de UPDATE-opdrachten uit te voeren. Vervolgens wordt methode [close()](https://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) aangeroepen om de verbinding vóór het sluiten te verbreken.

Vervang de tekenreeksen `host`, `database`, `username` en `password` door uw eigen waarden. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('mydemoserver.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@mydemoserver')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Update data
   client.query('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
   puts 'Updated 1 row of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```


## <a name="delete-data"></a>Gegevens verwijderen
Gebruik de volgende code om verbinding te maken en de gegevens te lezen met behulp van de SQL-instructie **DELETE**. 

De code gebruikt een [mysql2::client](https://www.rubydoc.info/gems/mysql2/0.4.8) .new()-methode om verbinding te maken met Azure Database voor MySQL. Daarna wordt de methode [query()](https://www.rubydoc.info/gems/mysql2/0.4.8#Usage) aangeroepen om de DELETE-opdrachten uit te voeren. Vervolgens wordt methode [close()](https://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) aangeroepen om de verbinding vóór het sluiten te verbreken.

Vervang de tekenreeksen `host`, `database`, `username` en `password` door uw eigen waarden. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('mydemoserver.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@mydemoserver')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Delete data
    resultSet = client.query('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een database migreren met behulp van Exporteren en importeren](./concepts-migrate-import-export.md)
