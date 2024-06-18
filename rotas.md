# Dicionário de Uso de Rotas em ASP.NET Core

Este dicionário contém informações sobre o uso de rotas em ASP.NET Core.

## [Route]

O atributo `[Route]` é usado para definir a rota de um controlador ou método de ação.

```csharp
[Route("api/[controller]")] // Rota do controlador
public class LoginController : ControllerBase
{
    // Métodos de ação HTTP POST
    [HttpPost]
    public IActionResult Login()
    {
    }
}
```
O ASP.NET Core gera rotas automaticamente para controladores e métodos de ação.

Para um controlador chamado LoginController, a rota padrão seria "/login" 
então ficaria assim:

http://localhost:5000/api/login

Para uma chamada ao método de ação Login que fica dentro do controlador LoginController
ficaria assim:

http://localhost:5000/api/login/login

# Criação de Rotas Personalizadas

```csharp
[ApiController]
    [Route("api/autenticar")] // Definindo a rota base explicitamente do controller como: api/autenticar
    public class AuthController : ControllerBase
    {
        [HttpPost("login/{id}")] // A rota para esse método de ação "LoginUsuario" a partir de agora será: /api/autenticar/login/{id}
        public IActionResult LoginUsuario(int id)
        {
        }
    }
```
