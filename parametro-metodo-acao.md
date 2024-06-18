# Dicionário de Uso de [FromBody] e Outros Tipos de Parâmetros em ASP.NET Core

Este dicionário contém informações sobre o uso do atributo `[FromBody]` e outros tipos de parâmetros em ASP.NET Core.

```csharp
[HttpPost]
public IActionResult Create([FromBody] User newUser)
{   
    // [FromBody]
    // Lógica para criar um novo usuário com base nos dados do corpo da solicitação
}

[HttpGet]
public IActionResult GetUsers([FromQuery] string searchTerm)
{
    // [FromQuery]
    // Lógica para obter usuários com base no termo de pesquisa fornecido na consulta da URL
}

[HttpGet("{id}")]
public IActionResult GetUserById([FromRoute] int id)
{
    // [FromRoute]
    // Lógica para obter um usuário pelo ID da rota
}

[HttpGet]
public IActionResult GetHeaderValue([FromHeader] string userAgent)
{
    // [FromHeader]
    // Lógica para usar o valor do cabeçalho User-Agent
}

[HttpPost]
public IActionResult ProcessForm([FromForm] FormData formData)
{
    // [FromForm]
    // Lógica para processar os dados do formulário enviado
}

```


