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
Para que o ModelState.IsValid funcione corretamente, o EmployeeDto deve usar anotações de dados para definir regras de validação:
Como [Required], [StringLength(100)] etc...

``` CSharp
public class EmployeeDto
{
    [Required]
    public int EmployeeID { get; set; }

    [Required]
    [StringLength(100)]
    public string Name { get; set; }

    [Required]
    [StringLength(50)]
    public string Position { get; set; }
}
```
