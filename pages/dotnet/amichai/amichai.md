---
title: Amichai
layout: page
---


## Создать проекты, добавить пакеты
- Blank Solution

- Создать проекты
  - src/
  - src/**Web** (Empty .Net)
  - src/**Presentation** (Class Library)
  - src/**Infrastructure** (Class Library)
  - src/**Application** (Class Library)
  - src/**Domain** (Class Library)
  - **requests** (Shared Project)
  - **docs** (Shared Project)

- Удалить везде Class1.cs 


- Добавить везде референсы


| Web | Presentation | Infrastructure | Application |
|-----|-----------|----------------|-------------|
| Presentation | Web | Application | Domain|
| Infrastructure | Infrastructure |||
| Application | Application |||

{% include_relative _includes/1.md %}
{% include_relative _includes/2.md %}
{% include_relative _includes/code.md %}
  
## Тесты
  
- Создать проекты
  - tests/
  - tests/**Architecture.Tests**

## Другое
  
- <details>
      <summary>В корне создать dockerfile</summary>
      
      ```dockerfile
      FROM httpd:alpine
      COPY ./html/ /usr/local/apache2/htdocs/
      ```
  </details>
      
- <details>
    <summary>Докер bash-команды</summary>

    `docker images`

    `docker build -t hello-docker:1.0.0 .`
  </details>
  
- <details>
  <summary>http-запросы VS Code</summary>  
  
  **requests**/Authentication/Register.http
  
  ```http
  @host=https://localhost:7056

  POST {{host}}/auth/register
  Content-type: application/json

  {
      "firstName": "Anton",
      "lastName": "K",
      "email": "ak@example.com",
      "password": "P@ssword123!"
  }  
  ```
  
  **requests**/Authentication/Login.http
  
  ```http
  @host=https://localhost:7056

  POST {{host}}/auth/login
  Content-type: application/json

  {
      "email": "ak@example.com",
      "password": "P@ssword123!"
  }
  ```
  </details>

- <details>
  <summary>Установка сервисов ASP.NET через Assembly</summary>  
  
  IInstaller.cs
  
  ```csharp
    public interface IInstaller
    {
        void InstallServices(IServiceCollection services, IConfiguration configuration);
    }
  ```
  
  InstallerExtensions.cs
  
  ```csharp
    public static class InstallerExtensions
    {
        //Вызвать в services Program.cs
        public static void InstallServicesInAssembly(this IServiceCollection services, IConfiguration configuration)
        {
            var installers = typeof(Program).Assembly.ExportedTypes.Where(x => typeof(IInstaller).IsAssignableFrom(x)
            && !x.IsInterface && !x.IsAbstract) //Найти все классы реализующие IInstaller, которые не интерфейсы и не абстрактные
            .Select(Activator.CreateInstance) //Создать экземпляр каждого
            .Cast<IInstaller>() //Привести к типу IInstaller
            .ToList(); //Сделать List

            installers.ForEach(installer => installer.InstallServices(services, configuration));
        }
    }
  ```
  </details>
