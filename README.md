# Database Structure Extractor

A comprehensive Python tool for extracting and analyzing database schemas from multiple database systems. This tool provides an interactive CLI interface to manage database connections, extract complete database structures, and export schemas in both JSON and SQL DDL formats.

## Features

- **Multi-Database Support**
  - MySQL
  - PostgreSQL
  - SQLite
  - SQL Server

- **Complete Schema Extraction**
  - Tables and table structures
  - Column definitions (name, type, nullable, default values)
  - Primary keys
  - Foreign keys with references
  - Indexes and their properties

- **Multiple Export Formats**
  - JSON format for programmatic usage
  - SQL DDL (Data Definition Language) for database recreation

- **Configuration Management**
  - Save and manage multiple database configurations
  - Secure configuration storage
  - Easy switching between different database connections

- **Interactive CLI**
  - User-friendly command-line interface
  - Menu-driven navigation
  - Real-time connection testing

## Installation

### Prerequisites
- Python 3.7 or higher
- pip (Python package manager)

### Basic Installation

1. Clone the repository:
```bash
git clone https://github.com/aniruddha-aits/Database-Extractor.git
cd Database-Extractor
```

2. Install base dependencies:
```bash
pip install -r requirements.txt
```

3. Install database-specific drivers (optional, based on your needs):

For MySQL support:
```bash
pip install pymysql
```

For PostgreSQL support:
```bash
pip install psycopg2-binary
```

For SQL Server support:
```bash
pip install pyodbc
```

Or install all optional dependencies:
```bash
pip install pymysql psycopg2-binary pyodbc
```

## Usage

### Running the Application

```bash
python main.py
```

This will launch the interactive CLI with the following main options:

### Main Menu Options

1. **Manage Database Configurations**
   - Add new database configurations
   - Edit existing configurations
   - Delete configurations
   - Configurations are securely stored in `~/.db_extractor/db_configs.json`

2. **Extract Database Schema**
   - Select a saved configuration
   - Connect to the database
   - Extract complete schema information
   - Export to JSON, SQL DDL, or both

3. **View Saved Configurations**
   - List all saved database configurations
   - View connection details
   - See configuration storage location

4. **Exit**
   - Safely exit the application

### Adding a Database Configuration

When adding a new configuration, you'll be prompted for:

**For SQLite:**
- Configuration name
- Database file path

**For MySQL, PostgreSQL, and SQL Server:**
- Configuration name
- Host address (e.g., localhost or 127.0.0.1)
- Port (uses defaults if not provided)
- Database name
- Username
- Password

### Extracting Schema

1. Select the database configuration to use
2. The tool will connect to the database
3. Schema information will be extracted
4. Choose export format(s):
   - JSON format (structured schema data)
   - SQL DDL (CREATE TABLE statements)
   - Both formats
   - View only (display in terminal)

## Configuration Storage

Database configurations are stored in JSON format at:
```
~/.db_extractor/db_configs.json
```

This location is automatically created on first use. The JSON file is not encrypted, so handle it with appropriate security measures.

### Example Configuration

```json
{
  "production-db": {
    "name": "production-db",
    "db_type": "postgres",
    "host": "prod.example.com",
    "port": 5432,
    "database": "production",
    "username": "dbuser",
    "password": "dbpassword",
    "file_path": "",
    "options": {}
  }
}
```

## Output Formats

### JSON Schema Format

The JSON output contains:
```json
{
  "database_name": "database_name",
  "db_type": "postgres",
  "tables": [
    {
      "name": "table_name",
      "columns": [
        {
          "name": "column_name",
          "type": "varchar",
          "nullable": true,
          "default": null,
          "primary_key": false
        }
      ],
      "foreign_keys": [...],
      "indexes": [...]
    }
  ]
}
```

### SQL DDL Format

The SQL output contains:
```sql
-- Database: database_name
-- Type: postgres

CREATE TABLE table_name (
    column_name varchar,
    ...
);

CREATE INDEX index_name ON table_name (column_name);
```

## Architecture

### Core Classes

- **DBConfig**: Dataclass for storing database configuration
- **ConfigManager**: Manages loading, saving, and deleting configurations
- **SchemaExtractor**: Connects to databases and extracts schema information
- **InteractiveCLI**: Provides the command-line user interface

### Supported Database Operations

#### SQLite
- Uses native sqlite3 module
- Extracts: tables, columns, foreign keys, indexes via PRAGMA statements

#### MySQL
- Uses pymysql for connections
- Extracts: tables, columns via SHOW statements and INFORMATION_SCHEMA queries

#### PostgreSQL
- Uses psycopg2 for connections
- Extracts: schema information from information_schema tables

#### SQL Server
- Uses pyodbc for connections
- Extracts: schema from INFORMATION_SCHEMA and sys catalog views

## Examples

### Example 1: Extract SQLite Database

```
1. Select "Manage Database Configurations"
2. Select "Add New Configuration"
3. Choose "3. SQLite"
4. Enter name: "my-local-db"
5. Enter path: "/path/to/database.db"
6. Back to main menu
7. Select "Extract Database Schema"
8. Choose "my-local-db"
9. Select "Save as JSON"
10. Choose output directory
```

### Example 2: Extract PostgreSQL Schema

```
1. Add configuration with:
   - Name: "prod-postgres"
   - Type: PostgreSQL
   - Host: db.example.com
   - Port: 5432
   - Database: myapp
   - Username: admin
   - Password: secure-password

2. Extract schema to both JSON and SQL DDL
3. Review generated files for database documentation
```

## Troubleshooting

### Connection Issues

- **MySQL not available**: Install with `pip install pymysql`
- **PostgreSQL not available**: Install with `pip install psycopg2-binary`
- **SQL Server not available**: Install with `pip install pyodbc`

### Permission Errors

- Ensure the configuration directory can be created in your home folder
- Verify database user has SELECT permissions on schema metadata

### No Tables Found

- Verify the database connection credentials are correct
- Check that the database user has permissions to view tables
- Some databases may have tables in specific schemas (e.g., PostgreSQL's "public" schema)

## Security Considerations

1. **Configuration Storage**: Store `~/.db_extractor/db_configs.json` securely
2. **Passwords**: Database passwords are stored in plain text in the config file
3. **Access Control**: Restrict access to the config directory and files
4. **Database Credentials**: Use database users with minimal required permissions

## Limitations

- PostgreSQL extraction limited to "public" schema
- SQL Server extraction limited to base tables
- No support for stored procedures or functions extraction
- Configuration passwords stored in plain text

## Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

## License

This project is open source and available under the MIT License.

## Support

For issues, questions, or suggestions, please create an issue on the GitHub repository.

## Roadmap

- [ ] Encryption for configuration passwords
- [ ] Support for schema-specific extraction (PostgreSQL, SQL Server)
- [ ] Stored procedure and function extraction
- [ ] CSV export format
- [ ] Database comparison tool
- [ ] Graphical user interface

## Changelog

### Version 1.0.0
- Initial release
- Support for MySQL, PostgreSQL, SQLite, and SQL Server
- JSON and SQL DDL export formats
- Configuration management system
- Interactive CLI interface
