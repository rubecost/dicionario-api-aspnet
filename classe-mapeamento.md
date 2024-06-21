# Classes de Mapeamento para Campos do Model

Essa classe de mapemento interage diretamente com os valores recebidos nas requisições a API, 
ela é responsavel por repassar os valores aos campos das classe do Model.

``` CSharp
using YourNamespace.Models;
using YourNamespace.DTOs;

namespace YourNamespace.Mappings
{
    public static class EmployeeMapping
    {

      // Read (GET):
        public static EmployeeDto ToDto(Employee employee)
        {
            return new EmployeeDto
            {
                Id = employee.Id,
                Name = employee.Name,
                // Adicionar mais informações conforme precisar
            };
        }

       // Create (POST):
        public static Employee ToModel(EmployeeDto employeeDto)
        {
            return new Employee
            {
                Id = employeeDto.Id,
                Name = employeeDto.Name,
                 // Adicionar mais informações conforme precisar
            };
        }

       // Update (PUT):
        public static void UpdateFromDto(Employee employee, EmployeeDto employeeDto)
        {
            employee.Name = employeeDto.Name;
             // Adicionar mais informações conforme precisar
        }

    }
}
```
# Create (POST)

A Controller usando o mapeamento para Criar:

``` CSharp
[HttpPost]
public async Task<IActionResult> CreateEmployee(EmployeeDto employeeDto)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    // Mapeamento com método "ToModel" para Criar.
    var employee = EmployeeMapping.ToModel(employeeDto); 

    await _employeeService.CreateEmployeeAsync(employee);

    // Mapeamento com método "ToDto" para Ler.
    var createdDto = EmployeeMapping.ToDto(employee);

    return CreatedAtAction(nameof(GetEmployee), new { id = createdDto.Id }, createdDto);
}
```
# Read (GET)

A Controller usando o mapeamento para Ler:

``` CSharp
[HttpGet("{id}")]
    public async Task<IActionResult> GetEmployee(int id)
    {
        var employee = await _employeeService.GetEmployeeAsync(id);

        if (employee == null)
        {
            return NotFound();
        }

       // Mapeando com o método "ToDto" para Ler.
        var employeeDto = EmployeeMapping.ToDto(employee);

        return Ok(employeeDto);
    }
```
# Update (PUT)

A Controller usando o mapeamento para Atualizar:

``` CSharp
[HttpPut("{id}")]
public async Task<IActionResult> UpdateEmployee(int id, EmployeeDto employeeDto)
{
    if (id != employeeDto.Id)
    {
        return BadRequest();
    }

    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    var existingEmployee = await _employeeService.GetEmployeeAsync(id);

    if (existingEmployee == null)
    {
        return NotFound();
    }

  // Mapeando com o método "UpdateFromDto" para Atualizar.
    EmployeeMapping.UpdateFromDto(existingEmployee, employeeDto);

    await _employeeService.UpdateEmployeeAsync(existingEmployee);

    return NoContent();
}
```
# Delete (DELETE)

Não necessario.
