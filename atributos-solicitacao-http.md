# Dicionário de Atributos de Métodos de Solicitação HTTP em ASP.NET Core 

Este dicionário contém informações sobre os principais atributos de métodos de solicitação HTTP em ASP.NET Core.

Esses atributos são classes que derivam de System.Attribute e são usados para adicionar metadados a um elemento de código. 
Eles são colocados diretamente antes do elemento ao qual se aplicam, entre colchetes ([]).

```csharp

[HttpGet] // Indica que um método de ação deve responder a solicitações HTTP GET.
public IActionResult GetAllUsers()
{
    return Ok(Users); // Exemplo de retorno
}

[HttpPost] // Indica que um método de ação deve responder a solicitações HTTP POST.
public IActionResult CreateUser(User user)
{
    // Lógica para criar um usuário
}

[HttpPut("{id}")] // Indica que um método de ação deve responder a solicitações HTTP PUT.
public IActionResult UpdateUser(int id, User user)
{
    // Lógica para atualizar um usuário com o ID fornecido
}

[HttpDelete("{id}")] // Indica que um método de ação deve responder a solicitações HTTP DELETE.
public IActionResult DeleteUser(int id)
{
    // Lógica para excluir um usuário com o ID fornecido
}

[HttpPatch("{id}")] // Indica que um método de ação deve responder a solicitações HTTP PATCH, usadas para atualizações parciais de recursos.
public IActionResult PartialUpdateUser(int id, JsonPatchDocument<User> patchDocument)
{
    // Lógica para atualizar parcialmente um usuário com o ID fornecido
}

[HttpOptions] // Indica que um método de ação deve responder a solicitações HTTP OPTIONS, usadas para determinar as opções de comunicação disponíveis para um recurso.
public IActionResult GetOptions()
{
    // Lógica para fornecer as opções disponíveis para um recurso
}

[HttpHead("{id}")] // Indica que um método de ação deve responder a solicitações HTTP HEAD, usadas para obter apenas informações de cabeçalho de resposta sem o corpo da resposta.
public IActionResult GetHeaderInformation(int id)
{
    // Lógica para obter informações de cabeçalho para um recurso com o ID fornecido
}

```
