# Realizando análise de código estático com sonarqube
## SonarQube
SonarQube é uma plataforma de código aberto para avaliação contínua da qualidade do código fonte. É projetada para ajudar desenvolvedores a identificar e corrigir problemas de código, melhorar a segurança e manter boas práticas de programação. O SonarQube suporta diversas linguagens de programação, incluindo Java, C#, JavaScript, TypeScript, Python, entre outras.

O SonarQube é uma ferramenta valiosa para equipes de desenvolvimento que desejam garantir a qualidade do código, detectar e corrigir problemas cedo no ciclo de desenvolvimento, e melhorar a manutenibilidade e a segurança do software.

## SonarScanner
É uma ferramenta de linha de comando ou um plugin para diversas ferramentas de build (como Maven, Gradle, MSBuild) que é usada para analisar o código-fonte de um projeto e enviar os resultados para o SonarQube. O SonarScanner coleta dados do código e os envia para o SonarQube para que a plataforma possa processar essas informações, gerar relatórios e fornecer feedback sobre a qualidade do código.

## Controllers
```using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Logging;

namespace App.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class SampleController : ControllerBase
    {

        private static readonly string[] Summaries = new[]
        {
            "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
        };

        public int publicVariable;
        private readonly ILogger<SampleController> _logger;

        public SampleController(ILogger<SampleController> logger)
        {
            _logger = logger;
        }

        [HttpGet("/SecurityIssueOnUseRandom")]
        public IEnumerable<SampleModel> GetSecurityIssueOnUseRandom()
        {
            var rng = new Random();
            return Enumerable.Range(1, 5).Select(index => new SampleModel
            {
                Date = DateTime.Now.AddDays(index),
                Summary = Summaries[rng.Next(Summaries.Length)]
            })
            .ToArray();
        }

        [HttpGet("/BugOnNullPath")]
        public SampleModel GetBugOnNullPath()
        {
            SampleModel sampleModel = null;
            sampleModel.Date = DateTime.Now;
            return sampleModel;
        }

        [HttpGet("/BugOnAlwaysEvaluateToFalse")]
        public IActionResult GetBugOnAlwaysEvaluateToFalse()
        {
            var alwaysFalse = false;

            if (alwaysFalse)
                alwaysFalse = true;

            return Ok();
        }
    }
}
``` 

### Namespaces e Usings:

O código inclui alguns namespaces necessários para o funcionamento do ASP.NET Core, como `Microsoft.AspNetCore.Mvc` e `Microsoft.Extensions.Logging`.

### Classe SampleController:

É uma classe de controleador ASP.NET Core. Controladores são responsáveis por gerenciar as requisições HTTP e retornar respostas adequadas.

### Campos e Propriedades:

- `publicVariable`: Uma variável pública da classe.
- `_logger`: Um campo privado para o logger, que é uma instância da classe `ILogger<SampleController>`.

### Construtor:

`SampleController`: O construtor da classe `SampleController` recebe uma instância de `ILogger<SampleController>`, que é usado para realizar o logging.

### Métodos de Ação (Action Methods):

- `GetSecurityIssueOnUseRandom`: Retorna uma lista de objetos `SampleModel`, onde as datas são incrementadas a partir da data atual e os Summaries são escolhidos aleatoriamente de uma lista predefinida.
- `GetBugOnNullPath`: Tentativa de acessar a propriedade `Date` de um objeto `SampleModel` que foi inicializado como `null`. Isso causará uma exceção. A correção sugerida é inicializar o objeto antes de acessar suas propriedades.
- `GetBugOnAlwaysEvaluateToFalse`: Declara uma variável booleana `alwaysFalse`, mas não a utiliza de forma significativa. A lógica dentro do bloco `if` não afeta o fluxo do programa.


## Resultados 
![image](https://github.com/sophiatosarr/dotnet-sonarqube-example/assets/99216420/3d03161b-ba60-4c36-9182-c0b467069d65)

![image](https://github.com/sophiatosarr/dotnet-sonarqube-example/assets/99216420/0219bc23-b177-4053-a8f6-e9c556b4b473)
