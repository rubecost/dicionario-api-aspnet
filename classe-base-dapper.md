# Classe Base para Operações CRUD com Dapper ao Stored Procedures do MySQL 

Definir uma classe chamada Base com métodos genéricos para operações CRUD.

``` CSharp
// Nome dessa classe: BaseRepository.cs

using Dapper;
using MySql.Data.MySqlClient;
using System.Collections.Generic;
using System.Data;
using System.Threading.Tasks;

public abstract class BaseRepository<T>
{
    private readonly string _connectionString;

    protected BaseRepository(string connectionString)
    {
        _connectionString = connectionString;
    }

    protected async Task<IEnumerable<T>> ExecuteStoredProcedureAsync(string storedProcedure, object parameters = null)
    {
        using var connection = new MySqlConnection(_connectionString);
        return await connection.QueryAsync<T>(storedProcedure, parameters, commandType: CommandType.StoredProcedure);
    }

    protected async Task<int> ExecuteCommandAsync(string storedProcedure, object parameters = null)
    {
        using var connection = new MySqlConnection(_connectionString);
        return await connection.ExecuteAsync(storedProcedure, parameters, commandType: CommandType.StoredProcedure);
    }

    public async Task<IEnumerable<T>> GetAllAsync(string storedProcedure)
    {
        return await ExecuteStoredProcedureAsync(storedProcedure);
    }

    public async Task<T> GetByIdAsync(string storedProcedure, object parameters)
    {
        var result = await ExecuteStoredProcedureAsync(storedProcedure, parameters);
        return result.FirstOrDefault();
    }

    public async Task AddAsync(string storedProcedure, object parameters)
    {
        await ExecuteCommandAsync(storedProcedure, parameters);
    }

    public async Task UpdateAsync(string storedProcedure, object parameters)
    {
        await ExecuteCommandAsync(storedProcedure, parameters);
    }

    public async Task DeleteAsync(string storedProcedure, object parameters)
    {
        await ExecuteCommandAsync(storedProcedure, parameters);
    }
}
```
# Repositório Específico

Criar uma segunda classe que herda da classe Base BaseRepository.
Aqui cria-se os métodos passando os nomes dos procedimentos a serem usados do MySQL, e que informações seram atribuidas aos Registros do Banco de dados.

``` CSharp
// Nome dessa classe: EmployeeRepository.cs

public class EmployeeRepository : BaseRepository<Employee>
{
    public EmployeeRepository(string connectionString) : base(connectionString) { }

    public async Task<IEnumerable<Employee>> GetEmployeeDetailsAsync(int empId)
    {
        return await GetAllAsync("GetEmployeeDetails");
    }

    public async Task AddEmployeeAsync(EmployeeDto employeeDto)
    {
        await AddAsync("AddEmployee", new { name = employeeDto.Name, position = employeeDto.Position });
    }

    public async Task UpdateEmployeeAsync(EmployeeDto employeeDto)
    {
        await UpdateAsync("UpdateEmployee", new { emp_id = employeeDto.EmployeeID, name = employeeDto.Name, position = employeeDto.Position });
    }

    public async Task DeleteEmployeeAsync(int empId)
    {
        await DeleteAsync("DeleteEmployee", new { emp_id = empId });
    }
}
```
# Uso do EmployeeRepository no Serviço

Agora é só criar uma classe de serviço para usar o Uso EmployeeRepository criado anteriormente.

``` CSharp
// O nome dessa classe será: EmployeeService.cs

public class EmployeeService
{
    private readonly EmployeeRepository _employeeRepository;

    public EmployeeService(IConfiguration configuration)
    {
        var connectionString = configuration.GetConnectionString("DefaultConnection");
        _employeeRepository = new EmployeeRepository(connectionString);
    }

    public async Task<IEnumerable<Employee>> GetEmployeeDetailsAsync(int empId)
    {
        return await _employeeRepository.GetEmployeeDetailsAsync(empId);
    }

    public async Task AddEmployeeAsync(EmployeeDto employeeDto)
    {
        await _employeeRepository.AddEmployeeAsync(employeeDto);
    }

    public async Task UpdateEmployeeAsync(EmployeeDto employeeDto)
    {
        await _employeeRepository.UpdateEmployeeAsync(employeeDto);
    }

    public async Task DeleteEmployeeAsync(int empId)
    {
        await _employeeRepository.DeleteEmployeeAsync(empId);
    }
}
```
# Funcionamento do Controller

Esse controlador utiliza todos os métodos CRUD da classe de serviço EmployeeService.

``` CSharp
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using System.Threading.Tasks;

[ApiController]
[Route("api/[controller]")]
public class EmployeesController : ControllerBase
{
    private readonly EmployeeService _employeeService;

    public EmployeesController(EmployeeService employeeService)
    {
        _employeeService = employeeService;
    }

    // GET: api/employees
    [HttpGet]
    public async Task<IActionResult> GetAllEmployees()
    {
        var employees = await _employeeService.GetEmployeeDetailsAsync(0); // Supondo que passar 0 retorna todos os empregados
        return Ok(employees);
    }

    // GET: api/employees/{id}
    [HttpGet("{id}")]
    public async Task<IActionResult> GetEmployeeDetails(int id)
    {
        var employee = await _employeeService.GetEmployeeDetailsAsync(id);
        if (employee == null)
        {
            return NotFound();
        }
        return Ok(employee);
    }

    // POST: api/employees
    [HttpPost]
    public async Task<IActionResult> AddEmployee([FromBody] EmployeeDto employeeDto)
    {
        if (!ModelState.IsValid)
        {
            return BadRequest(ModelState);
        }

        await _employeeService.AddEmployeeAsync(employeeDto);
        return CreatedAtAction(nameof(GetEmployeeDetails), new { id = employeeDto.EmployeeID }, employeeDto);
    }

    // PUT: api/employees/{id}
    [HttpPut("{id}")]
    public async Task<IActionResult> UpdateEmployee(int id, [FromBody] EmployeeDto employeeDto)
    {
        if (id != employeeDto.EmployeeID)
        {
            return BadRequest();
        }

        if (!ModelState.IsValid)
        {
            return BadRequest(ModelState);
        }

        await _employeeService.UpdateEmployeeAsync(employeeDto);
        return NoContent();
    }

    // DELETE: api/employees/{id}
    [HttpDelete("{id}")]
    public async Task<IActionResult> DeleteEmployee(int id)
    {
        var employee = await _employeeService.GetEmployeeDetailsAsync(id);
        if (employee == null)
        {
            return NotFound();
        }

        await _employeeService.DeleteEmployeeAsync(id);
        return NoContent();
    }
}
```

