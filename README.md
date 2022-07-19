## **Visão Geral**
### **dontet-trace-plugin**

O plugin **`dotnet-trace-plugin`** foi projetado para apoiar os desenvolvedores a configurar e utilizar o [**Opentelemetry**](https://opentelemetry.io/) com exporter para [**Jaeger**](https://www.jaegertracing.io/) e instrumentação [**AWS X-Ray**](https://docs.aws.amazon.com/pt_br/xray/latest/devguide/aws-xray.html).

## **Uso**

#### **Pré-requisitos**
Para utilizar esse plugin é necessário ter uma stack DotNET criada pelo `CLI` do `StackSpot` que você pode baixar [**aqui**](https://stackspot.com/).

Ter instalado:
- .NET 5 ou 6 
- O template `dotnet-api-template` ou o `dotnet-worker-template` deverá estar aplicado para você conseguir utilizar este plugin.

#### **Configuração de Inputs**  
Confira os inputs que precisam ser configurados durante a aplicação do plugin:  

| **Campo** | **Valor** | **Descrição** |
| :--- | :--- | :--- |
| Type| Padrão: "Jaeger" | Tipo do Exporter(Jaeger, OTLP, X-Ray, X-Ray - Daemon) |
| Server|  | Hostname do agent  |
| Port|  | Port do agent  |
| AppName | Nome da Aplicação | Campo Obrigatório.
| ExporterType | Tipo de Exportação (Jaeger, x-ray ou otlp). |
| Host | Hostname do agent | Comunicação via UDP.
| Port | Porta do agent | Comunicação via UDP.
| ConsoleExporter | Exportar para o console. |
| UseGrpcClientInstrumentation | Habilitar instrumentação gRPC. |
| UseHttpClientInstrumentation | Habilitar instrumentação http. |
| Tags | Headers que serão propagados. |

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

#### **Inputs configurados automaticamente**  

A configuração abaixo será feita no **`IServiceCollection`**, através do **`services.AddOpenTelemetryTracing()`** no `Startup` da aplicação ou `Program`.

```csharp
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddOpenTelemetryTracing(Configuration);
        }
```

#### **Execução em Ambiente local**  

Para executar o plugin, execute o comando abaixo para disponibilizar o contêiner **`jaegertracing`**:

```
  docker run -d -p6831:6831/udp -p16686:16686 jaegertracing/all-in-one:latest
```

Para visualizar os traces, acesse [este link](http://localhost:16686/).

### **Conteúdos complementares**
- [**Nuget OpenTelemetry**](https://www.nuget.org/packages/StackSpot.Tracing/)
- [**Nuget X-Ray**](https://www.nuget.org/packages/StackSpot.Tracing.XRay/)
