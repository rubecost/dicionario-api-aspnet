# Como usar a propriedade ModelState

O ModelState.IsValid pertence à classe ControllerBase, que é parte do ASP.NET Core MVC framework. Ele é usado para verificar a validade dos dados enviados para o controlador.
A ModelState é uma propriedade da classe ControllerBase, que representa o estado dos dados recebidos na requisição.

``` CSharp
[HttpPost] // Exemplo de uso em POST
    public async Task<IActionResult> AddEmployee([FromBody] EmployeeDto employeeDto)
    {
        if (!ModelState.IsValid)
        {
            return BadRequest(ModelState);  // Retorna 400 Bad Request se o modelo for inválido
        }
    }

    [HttpPut("{id}")] // Exemplo de uso em PUT
    public async Task<IActionResult> UpdateEmployee(int id, [FromBody] EmployeeDto employeeDto)
    {
        if (!ModelState.IsValid)
        {
            return BadRequest(ModelState);  // Retorna 400 Bad Request se o modelo for inválido
        }
    }

```
