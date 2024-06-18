# Dicionário de Atributos de Validação em C#

Este dicionário contém informações sobre os principais atributos de validação disponíveis em C#.

## [Required] 

Indica que um campo é obrigatório.

## [EmailAddress]

Valida se o valor de uma propriedade é um endereço de e-mail válido.

## [Compare]

Compara o valor da propriedade com o valor de outra propriedade.

```csharp
[Compare("OtherProperty")]