<p align="center">
  <img src="https://i.imgur.com/669nvEh.png"/>
</p>

![Release](https://img.shields.io/github/v/release/GregoBHM/CodacyDemoApp)
![.NET](https://img.shields.io/badge/.NET-8.0-blue)


# 🔐 Codacy .NET SAST Demo

This is a simple .NET 8 project that demonstrates how to use **Codacy** as a Static Application Security Testing (SAST) tool.

## 📦 What’s Inside

- `CodacyDemoApp/` → App with basic logic.
- `CodacyDemoApp.Tests/` → Unit tests using `xUnit`.
- `.github/workflows/codacy.yml` → GitHub Actions Codacy configuration file.

## 🛠️ Step-by-step Setup

### Clone the Repository
```bash
git clone https://github.com/GregoBHM/CodacyDemoApp.git
cd CodacyDemoApp
```

### 🔹 Create the base projects
```bash
dotnet new sln -n CodacyDemo 
dotnet new mvc -n CodacyDemoApp
dotnet new xunit -n CodacyDemoApp.Tests
dotnet sln CodacyDemo.sln add CodacyDemoApp/CodacyDemoApp.csproj
dotnet sln CodacyDemo.sln add CodacyDemoApp.Tests/CodacyDemoApp.Tests.csproj
dotnet add CodacyDemoApp.Tests/CodacyDemoApp.Tests.csproj reference CodacyDemoApp/CodacyDemoApp.csproj
```

---

### 🔹 Add a vulnerable class for Codacy

`CodacyDemoApp/Models/User.cs`

```csharp
namespace CodacyDemoApp.Models
{
    public class User
    {
        public string Username { get; set; }
        public string Password { get; set; } // insecure on purpose
    }
}
```

### 🔹 Create a unit test

`CodacyDemoApp.Tests/UnitTest1.cs`

```csharp
using CodacyDemoApp.Models;
using Xunit;

namespace CodacyDemoApp.Tests
{
    public class UserTests
    {
        [Fact]
        public void Should_Create_User_With_Username_And_Password()
        {
            var user = new User { Username = "admin", Password = "123" };
            Assert.Equal("admin", user.Username);
            Assert.Equal("123", user.Password);
        }
    }
}
```

### 🔹 Run tests with coverage

```bash
dotnet test CodacyDemoApp.Tests/CodacyDemoApp.Tests.csproj --collect:"XPlat Code Coverage"
```

### 🔹 Generate coverage report for Codacy

```bash
dotnet tool install --global dotnet-reportgenerator-globaltool
reportgenerator -reports:**/coverage.cobertura.xml -targetdir:coverage-report -reporttypes:Cobertura
```


## 🤖 GitHub Actions Workflow

This repository includes a pre-configured GitHub Actions workflow for Codacy:  
📂 `.github/workflows/codacy.yml`

---

## 🔐 Codacy Setup

1. Go to [https://app.codacy.com](https://app.codacy.com)
2. Import your GitHub repository
3. Go to **Settings > Integrations**
4. Create a new **Project API Token**
5. Add it to GitHub:  
   `Settings > Secrets > Actions > New repository secret`
   - Name: `CODACY_PROJECT_TOKEN`
   - Value: (your token)

---
