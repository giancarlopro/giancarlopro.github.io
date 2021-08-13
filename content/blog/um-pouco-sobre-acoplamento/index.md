---
title: Um Pouco Sobre Acoplamento
date: "2021-08-11T11:01:03.284Z"
description: Um resumo sobre o que eu entendo por coplamento, no mundo do desenvolvimento de software.
tags: coupling,solid,designpartterns,softwarearchitecture
---

Acoplamento é a força que une os componentes do software, e se refere ao quanto um componente depende de outro para realizar sua função. Quanto mais acoplado forem os componentes, mais difícil fica para dar manutenção ao código, por isso busca-se designs de software com o mínimo de acoplamento possível.

### Exemplos

Para ficar mais claro o que é acoplamento, vamos imaginar um carro e uma pessoa, caso queiramos trocar os pneus, ou o motor de um carro, nós conseguimos fazer isso sem necessáriamente alterar a estrutura do carro, e o carro continuaria funcionando com a parte nova instalada, então dizemos que o carro possui um baixo acoplamento entre seus componentes.

Já com uma pessoa, não conseguimos, com a mesma facilidade, trocar uma perna, por exemplo, é algo complexo e que envolve o sistema circulatório, sistema nervoso e etc... Portanto podemos dizer que uma pessoa tem alto acoplamento entre seus componentes.

Vejamos um exemplo de alto acoplamento e baixo acoplamento com código:

```java
// Classe acoplada ao banco de dados MySQL
class PessoaModel {
	private MySQLDatabase database;
	PessoaModel(MySQLDatabase database) { this.database = database; }
	...
}

// Exemplo de classe que depende de uma interface genérica de banco de dados
// que para alterar o banco seria necessário somente alterar a implementação
// concreta de IDatabase.
//
// Imagine que existam as implementações MySQLDatabase, PostgreSQLDatabase
// e que elas implementam a interface IDatabase
interface IDatabase {
	IRecord find(/* params */);
	void create(/* params */);
}

class PessoaModel {
	private IDatabase database;
	PessoaModel(IDatabase database) { this.database = database; }
	...
}
```

No primeiro exemplo a classe `PessoaModel` depende exclusivamente da implementação de `MySQLDatabase` , assim, caso seja necessário mudar o banco de dados seria necessário modificar a classe `PessoaModel` para utilizar a nova implementação de banco de dados, e cada model existente no sistema deveria ser alterado também.

Já no segundo exemplo, possuímos a interface `IDatabase` que define os métodos genéricos que a classe `PessoaModel` pode utilizar. Sempre que for necessário alterar o banco de dados, basta criar uma nova implementação de `IDatabase` que a classe `PessoaModel` já vai saber utilizar essa nova implementação sem precisar ser alterada.

### Conclusão

Desenvolver buscando baixo acoplamento, vai te dar uma facilidade maior para adicionar novas funcionalidades e reduzir o risco de quebrar as funcionalidades já existentes, e vai tornar mais fácil adicionar testes automatizados, caso ainda não esteja utilizando, já que componentes separados são mais fáceis de testar.

É bom lembrar que o acoplamento máximo é difícil de ser atingido, dado que os componentes do software precisam interagir entre si de alguma forma, e que sempre irão existir demandas de tempo para entrega do software, já que reduzir o acoplamento pode trazer mais complexidade pro código, aumentando o tempo necessário para desenvolve-lo.

### Referências

- [Explicação de acoplamento no stackoverflow (inglês)](https://stackoverflow.com/questions/2832017/what-is-the-difference-between-loose-coupling-and-tight-coupling-in-the-object-o/37993102#37993102)
- [Explicação de acoplamento e coesão nos stackoverflow (inglês)](https://stackoverflow.com/questions/39946/coupling-and-cohesion/39988#39988)
- [A brief history of coupling and cohesion (artigo)](https://mrpicky.dev/a-brief-history-of-coupling-and-cohesion/)
- [SRP (Uncle Bob)](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html)
