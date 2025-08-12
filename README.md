 Task Management System - README

 Overview
A console-based task management application built with .NET 9.0 following Clean Architecture principles. The system features runtime feature management and a plugin system for extensibility.

 Architecture
The solution follows Clean Architecture with these layers:

 Domain Layer (Core Business Logic)
- Contains `TaskItem` entity and `ITaskRepository` interface
- No external dependencies

 Application Layer (Use Cases)
- Implements CQRS pattern using MediatR
- Contains commands and queries for task operations

 Infrastructure Layer (Implementations)
- Provides `InMemoryTaskRepository` implementation
- Handles data persistence (in-memory)

 Presentation Layer (Console UI)
- Thin console interface
- Dispatches commands/queries via MediatR

 Features

 Core Functionality
- Create tasks with descriptions
- View all tasks
- View task details
- Mark tasks as complete
- Delete tasks

 Feature Management
- **Task Prioritization**: 
  - Enabled/disabled via `appsettings.json`
  - When enabled:
    - Allows setting priority (Low/Medium/High)
    - Displays priority in task list

### Plugin System
- **Export Formats**:
  - JSON export via `JsonExporterPlugin`
  - Extensible system for additional formats

## Getting Started

### Prerequisites
- .NET 9.0 SDK
- Visual Studio Code (recommended) with C# extension

### Installation
1. Clone the repository
2. Restore dependencies:
   ```bash
   dotnet restore
   ```

### Running the Application
```bash
cd TaskManagementSystem.Presentation
dotnet run
```

## Configuration

### Feature Flags
Edit `appsettings.json` to enable/disable features:
```json
{
  "FeatureManagement": {
    "TaskPrioritization": true
  }
}
```

### Plugins
1. Built plugins are automatically copied to `Plugins` folder
2. To add a new plugin:
   - Create a class library implementing `IExporter`
   - Build the project
   - Copy DLL to `Plugins` folder

## Testing
Run unit tests with:
```bash
cd TaskManagementSystem.Tests
dotnet test
```

## Creating New Plugins

1. Create a new class library project
2. Reference `Exporter.Abstractions`
3. Implement `IExporter` interface
4. Build and copy DLL to Presentation's `Plugins` folder

Example exporter:
```csharp
public class CsvExporter : IExporter
{
    public string GetFormatName() => "CSV";
    public string GetFileExtension() => "csv";
    public byte[] Export(IEnumerable<TaskItem> tasks)
    {
        // Implementation
    }
}
```

## Security Notes
The application currently shows warnings for System.Text.Json vulnerabilities. These will be addressed in future updates.

## Troubleshooting
- **Plugin not loading**: Ensure DLL is in `Plugins` folder
- **Feature not working**: Verify `appsettings.json` configuration
- **Build warnings**: Most are about async methods that could be optimized

## Future Enhancements
- Add more export formats (CSV, XML)
- Implement database persistence
- Add user authentication



