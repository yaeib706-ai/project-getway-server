## Gateway.Api – Ocelot-based API Gateway

`Gateway.Api` is an API Gateway service built on .NET 9 and Ocelot, serving as a single entry point to a set of backend services.  
The gateway routes external client requests to internal services according to the configuration defined in `ocelot.json`.

### Main Technologies

- **Platform**: .NET 9 (`net9.0`)
- **Project type**: ASP.NET Core Web API
- **Libraries**:
  - **Ocelot** – API Gateway library for .NET
  - **Microsoft.AspNetCore.OpenApi** – OpenAPI support for the API

### Project Structure

- **Program.cs** – Application entry point and middleware pipeline configuration (including HTTPS and OpenAPI in development).
- **ocelot.json** – Gateway routing configuration:
  - Example route from `/gateway/service-a/{everything}` to a downstream service at `/api/service-a/{everything}`.
  - Downstream server definition (Host and Port), which should be updated per environment.
- **appsettings.json** – Logging configuration and host access settings (`AllowedHosts`).
- **Gateway.Api.csproj** – Project file, including `TargetFramework` and NuGet dependencies.

### Routing Configuration in Ocelot

The `ocelot.json` file currently defines a single example route:

- **Upstream**:
  - **Path**: `/gateway/service-a/{everything}`
  - **Methods**: `GET`, `POST`, `PUT`, `DELETE`
- **Downstream**:
  - **Path**: `/api/service-a/{everything}`
  - **Scheme**: `http`
  - **Host**: `REPLACE_DOWNSTREAM_HOST` – must be replaced with the actual server host.
  - **Port**: `5000`
- **GlobalConfiguration**:
  - **BaseUrl**: `https://localhost:7000` – default base URL for the gateway in development.

### Prerequisites

- **SDK**: .NET SDK version 9.0 or higher installed.
- **Active downstream service** (for example, Service A) running at an address and port that match the configuration in `ocelot.json`.

### Local Setup and Run Instructions

1. **Restore dependencies** (if needed):
   - From the `Gateway.Api` directory:

```bash
dotnet restore
```

2. **Configure downstream service address**:
   - Open the `ocelot.json` file.
   - Replace the `REPLACE_DOWNSTREAM_HOST` value with the hostname or IP address of the backend service (for example: `localhost`).

3. **Run the project**:
   - From the `Gateway.Api` directory:

```bash
dotnet run
```

4. **Access the gateway**:
   - By default, the gateway is available at `https://localhost:7000`.
   - Example requests to Service A:
     - `https://localhost:7000/gateway/service-a/...`

### OpenAPI and API Documentation

In the `Development` environment, the project exposes OpenAPI services.  
You can extend the endpoints in `Program.cs` and add OpenAPI documentation as needed.

### Environment-specific Configuration

- **Production / Staging**:
  - Update the `BaseUrl` under `GlobalConfiguration` in `ocelot.json`.
  - Adjust logging settings and `AllowedHosts` in `appsettings.json` according to your organization policies.

