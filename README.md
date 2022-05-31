## **Visão Geral**
### **dontet-trace-plugin**

O **dontet-trace-plugin** foi projetado para apoiar os desenvolvedores a configurar e utilizar o Opentelemetry com exporter para Jaeger e Instrumentação AWS X-Ray.

## **Uso**

#### **Pré-requisitos**
Para utilizar esse plugin é necessário ter uma stack DotNET criada pelo `CLI` do `StackSpot` que você pode baixar [**aqui**](https://stackspot.com/).

Ter instalado:
- .NET 5 ou 6 
- O template `dotnet-api-template` ou o `dotnet-worker-template` deverá estar aplicado para você conseguir utilizar este plugin.

#### **Inputs**
Os inputs necessários para utilizar o plugin são:
| **Campo** | **Valor** | **Descrição** |
| :--- | :--- | :--- |
| Type| Padrão: "Jaeger" | Tipo do Exporter(Jaeger, OTLP, X-Ray, X-Ray - Daemon) |
| Server|  | Hostname do agent  |
| Port|  | Port do agent  |

- AppName - Nome da Aplicação - Campo Obrigatório.
- ExporterType - Tipo de Exportação (Jaeger, x-ray ou otlp).
- Host - Hostname do agent - comunicação via UDP.
- Port - Porta do agent - comunicação via UDP.
- ConsoleExporter - Exportar para o console.
- UseGrpcClientInstrumentation - Habilitar instrumentação gRPC.
- UseHttpClientInstrumentation - Habilitar instrumentação http.
- Tags - Headers que serão propagados.

Você pode configurar as variáveis no arquivo `appsettings.json`.

```json
{
  "AppName": "MyAppName",  
  "Telemetry": {
    "ExporterType": "Jaeger",
    "Host": "127.0.0.1",
    "Port": 6831,
    "ConsoleExporter": true,
    "UseHttpClientInstrumentation": true,
    "UseGrpcClientInstrumentation": true,
    "Tags": [
            "X-PTO-TraceId",
            "X-PTO-ParentSpanId",
            "X-PTO-SpanId"
     ]    
  }
}
```

#### **Configurações**

Adicione ao seu `IServiceCollection` via `services.AddOpenTelemetryTracing()` no `Startup` da aplicação ou `Program`.

```csharp
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddOpenTelemetryTracing(Configuration);
        }
```

#### Ambiente local

Esta etapa não é obrigatória. Execute o comando abaixo para disponibilizar container `jaegertracing`:

```
  docker run -d -p6831:6831/udp -p16686:16686 jaegertracing/all-in-one:latest
```

Para visualizar os traces acesse: http://localhost:16686/

### **Implementação**
- [**Nuget OpenTelemetry**](https://www.nuget.org/packages/StackSpot.Tracing/)
- [**Nuget X-Ray**](https://www.nuget.org/packages/StackSpot.Tracing.XRay/)
