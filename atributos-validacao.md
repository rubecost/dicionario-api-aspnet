# Dicionário de Atributos de Validação em C#

Este dicionário contém informações sobre os principais atributos de validação disponíveis em C#.

```csharp

## [Required] 
Indica que um campo é obrigatório.

## [EmailAddress]
Valida se o valor de uma propriedade é um endereço de e-mail válido.

## [Compare]
Compara o valor da propriedade com o valor de outra propriedade.
[Compare("OtherProperty")]

## [Range]
Especifica os valores mínimo e máximo permitidos para uma propriedade numérica.
[Range(1, 100)]

## [StringLength]
Define o comprimento máximo (e opcionalmente o mínimo) de uma string.
[StringLength(50, MinimumLength = 5)]

## [MinLength]
Especifica o comprimento mínimo de uma string ou coleção.
[MinLength(5)]

## [MaxLength]
Especifica o comprimento máximo de uma string ou coleção.
[MaxLength(50)]

## [RegularExpression]
Valida se o valor da propriedade corresponde a uma expressão regular específica.
[RegularExpression(@"^[a-zA-Z0-9]*$")]

## [CreditCard]
Valida se o valor da propriedade é um número de cartão de crédito.

## [Phone]
Valida se o valor da propriedade é um número de telefone.

## [Url]
Valida se o valor da propriedade é uma URL.

## [DataType]
Especifica o tipo de dado associado com a propriedade (pode ser usado para formatação).
[DataType(DataType.Date)]

## [CustomValidation]
Permite a criação de validações personalizadas.
[CustomValidation(typeof(MyValidatorClass), "MyValidationMethod")]
```

Exemplo de uso:

```csharp

namespace SimpleApiExample.Models
{
    public class User
    {
        public int Id { get; set; }

        [Required]
        public string Name { get; set; }

        [Required]
        [EmailAddress]
        public string Email { get; set; }

        [Range(18, 99)]
        public int Age { get; set; }

        [StringLength(20, MinimumLength = 5)]
        public string Username { get; set; }

        [RegularExpression(@"^[a-zA-Z0-9]*$")]
        public string Password { get; set; }

        [Phone]
        public string PhoneNumber { get; set; }

        [Url]
        public string Website { get; set; }

        [DataType(DataType.Date)]
        public DateTime BirthDate { get; set; }

        [CustomValidation(typeof(UserValidator), "ValidateCustomProperty")]
        public string CustomProperty { get; set; }
    }
}
```
