# Apostila — Programação Orientada a Objetos com C#
## Construindo um Sistema Bancário do zero até uma API com banco de dados

> **Projeto de apoio:** [meu-projeto-dotnet](https://github.com/fabioBaraDev/meu-projeto-dotnet)
> **Público:** aluno que já conhece variáveis, contexto/escopo, classes e métodos.
> **Objetivo final:** evoluir o sistema bancário de console até uma **Web API em ASP.NET Core** com **PostgreSQL rodando em Docker**, praticando no caminho **herança, classes abstratas, polimorfismo e interfaces**.

---

## Como usar esta apostila

Cada capítulo segue o mesmo formato:

1. **Conceito** — a teoria explicada com analogias do mundo bancário.
2. **Mão na massa** — o refactor ou a funcionalidade nova aplicada no projeto real.
3. **Exercícios** — para o aluno fazer sozinho (e abrir PR! Ver Capítulo 1).
4. **💡 Dica para o professor** — sugestões de condução da aula.

A regra de ouro do curso: **todo capítulo termina com um Pull Request aberto pelo aluno**. Assim ele pratica Git/GitHub continuamente, e você revisa o código como aconteceria numa empresa de verdade.

### Roteiro do curso

| Capítulo | Tema | Conceitos |
|---|---|---|
| 1 | Git e GitHub | commit, branch, fork, Pull Request, boas práticas |
| 2 | Onde estamos | revisão do projeto e das camadas (controller → service → repository) |
| 3 | Herança e classes abstratas | `abstract`, `: base()`, especialização, `protected` |
| 4 | Polimorfismo | `virtual`, `override`, `abstract`, lista de `Conta` |
| 5 | Interfaces | contratos, acoplamento entre camadas, injeção de dependência |
| 6 | Banco de dados | Docker, PostgreSQL, SQL básico, repositório com Npgsql |
| 7 | Web API | ASP.NET Core, REST, controllers, DI nativa, Swagger |
| 8 | Próximos passos | testes, EF Core, validações, deploy |

---

# Capítulo 1 — Introdução ao Git e GitHub

## 1.1 Por que começar por aqui?

Antes de escrever mais uma linha de C#, o aluno precisa aprender a ferramenta que **todo** desenvolvedor usa todos os dias: o controle de versão. O Git é como o "salvar jogo" de um videogame: você grava pontos de progresso (commits) e pode voltar a qualquer um deles se algo der errado. O GitHub é o lugar na nuvem onde esses "saves" ficam guardados e compartilhados com outras pessoas.

**Git** = a ferramenta (roda na sua máquina).
**GitHub** = o site que hospeda repositórios Git e adiciona colaboração (Pull Requests, issues, code review).

## 1.2 Repositório e clone

Um **repositório** (repo) é a pasta do projeto com todo o histórico de mudanças. Para trazer um repo do GitHub para a sua máquina:

```bash
git clone https://github.com/fabioBaraDev/meu-projeto-dotnet.git
cd meu-projeto-dotnet
```

## 1.3 Commit — o "save" do projeto

Um **commit** é uma fotografia do projeto num momento, com uma mensagem explicando o que mudou. O fluxo tem três áreas:

```
arquivos modificados  --git add-->  staging (área de preparação)  --git commit-->  histórico
```

```bash
git status                          # o que mudou?
git add src/domain/conta.cs         # prepara um arquivo específico
git add .                           # ou prepara tudo
git commit -m "adiciona validacao de CPF na criacao de conta"
git log --oneline                   # ver o histórico
```

**Boas práticas de mensagem de commit:**

- Escreva no imperativo e diga **o que** a mudança faz: `adiciona conta poupanca`, `corrige calculo de saldo`.
- Mensagens como `ajustes`, `wip`, `final2` não contam nada para quem lê o histórico.
- Commits pequenos e frequentes > um commit gigante no fim do dia. Se a mensagem precisa da palavra "e" três vezes, provavelmente eram três commits.

## 1.4 Branch — trabalhando em paralelo

Uma **branch** (ramo) é uma linha do tempo paralela. A branch principal (`main`) deve estar sempre funcionando; o trabalho novo acontece em branches separadas.

```bash
git branch                              # em qual branch estou?
git checkout -b feature/conta-poupanca  # cria e muda para a nova branch
# ... trabalha, faz commits ...
git checkout main                       # volta para a main
```

Convenção comum de nomes: `feature/nome-da-funcionalidade`, `fix/nome-do-bug`, `docs/o-que-documenta`.

Analogia bancária: a `main` é o saldo oficial da conta; a branch é um rascunho de extrato onde você simula operações antes de efetivá-las.

## 1.5 Enviando para o GitHub

```bash
git push origin feature/conta-poupanca   # envia a branch para o GitHub
git pull origin main                     # traz novidades da main para a máquina
```

## 1.6 Fork — a sua cópia de um projeto de outra pessoa

Um **fork** é uma cópia completa de um repositório de outra pessoa para a **sua** conta do GitHub. É assim que se contribui com projetos open source dos quais você não é dono: você não tem permissão para mexer no repo original, então cria a sua cópia, trabalha nela, e depois propõe suas mudanças de volta.

Fluxo do aluno neste curso:

1. Aluno acessa `github.com/fabioBaraDev/meu-projeto-dotnet` e clica em **Fork**.
2. Faz `git clone` **do fork dele**, não do original.
3. Cria branch, faz commits, dá push **para o fork**.
4. Abre um **Pull Request** do fork para o repositório original.

## 1.7 Pull Request (PR) — propondo uma mudança

Um **Pull Request** é um pedido formal: *"olha as mudanças que fiz nesta branch — pode trazer para a main?"*. É onde acontece o **code review**: outra pessoa (no nosso caso, o professor) lê o código, comenta linha por linha, pede ajustes e, quando estiver bom, aprova e faz o **merge**.

No GitHub: aba **Pull requests → New pull request →** escolha a branch (ou o fork) de origem e a `main` de destino.

**Boas práticas de PR:**

- **PR pequeno.** Um PR de 100 linhas recebe review de verdade; um de 2000 recebe um "LGTM" desanimado. Uma funcionalidade por PR.
- **Título e descrição claros:** o que foi feito, por que, e como testar.
- **Nunca trabalhe direto na `main`.** Mesmo sozinho no projeto — o hábito é o que importa.
- Responda os comentários do review e faça os ajustes na mesma branch (o PR atualiza sozinho a cada push).
- Antes de abrir o PR, rode o projeto (`dotnet run`) e confirme que nada quebrou.

## 1.8 Exercícios

1. Faça um fork do `meu-projeto-dotnet` e clone o **seu** fork.
2. Crie a branch `docs/meu-primeiro-pr` e adicione um arquivo `ALUNO.md` com seu nome e o que espera aprender no curso.
3. Faça commit (com uma boa mensagem!), push, e abra um PR para o repositório original.
4. No histórico (`git log --oneline`), identifique o commit mais antigo do projeto.

> **💡 Dica para o professor:** faça um review "de verdade" nesse primeiro PR — comente pedindo uma mudança boba (ex.: corrigir um typo) só para o aluno viver o ciclo completo: review → ajuste → push → aprovação → merge. Esse ritual será repetido em todos os capítulos. Depois do primeiro merge, mostre `git log --oneline --graph --all` para o aluno visualizar as linhas do tempo se encontrando.

---

# Capítulo 2 — Onde estamos: revisão do projeto

## 2.1 A arquitetura em camadas

O projeto já está organizado em camadas, e isso foi proposital — cada camada tem **uma responsabilidade**:

```
Program.cs (menu do console)
        │
        ▼
CommandLineController      ← recebe entrada do usuário, converte texto em tipos (double, enum)
        │
        ▼
ContaService               ← regras de negócio (o "cérebro")
        │
        ▼
ContaRepository            ← guarda e busca dados (hoje: uma List em memória)
        │
        ▼
Conta (domain)             ← o modelo: o que É uma conta
```

Pergunta para o aluno: *"por que o controller não fala direto com o repository?"* Resposta esperada: porque as regras de negócio (validar saldo, impedir duplicidade de CPF, calcular rendimento) morariam então no controller ou no repository, misturando responsabilidades. Essa separação vai ficar **muito** mais valiosa nos capítulos 5, 6 e 7 — é ela que vai permitir trocar a `List` por PostgreSQL e o console por uma API **sem reescrever as regras de negócio**.

## 2.2 O estado atual da classe Conta

```csharp
class Conta
{
    public string nome {get; private set;}
    public string CPF {get; private set;}
    public double saldo {get; private set;}
    public TipoConta tipoConta {get; private set;}
    // construtor...
}

public enum TipoConta { Pobrinho, ClassMedia, Rico }
```

Dois pontos para discutir com o aluno **antes** de evoluir:

**a) O enum como "tipo" tem um limite.** Hoje o tipo da conta é só um rótulo. Mas e quando conta poupança **render juros** e conta corrente **tiver cheque especial**? Comportamentos diferentes por tipo levariam a um monte de `if (tipoConta == ...)` espalhados pelo código. Esse é exatamente o problema que **herança e polimorfismo** resolvem (Capítulos 3 e 4).

**b) Convenções de nomenclatura do C#.** A comunidade .NET usa **PascalCase** para propriedades e métodos públicos: `Nome`, `Cpf`, `Saldo`, `AdicionarConta()`. Variáveis locais e parâmetros usam camelCase: `saldoConvertido`. A partir do Capítulo 3, todo código novo da apostila segue essa convenção — e renomear o código antigo é um ótimo exercício de refactor (o Visual Studio/VS Code faz isso com F2 / Rename Symbol).

## 2.3 Exercícios de aquecimento

1. Implemente o método `atualizarCliente()` do controller, que está vazio (atualizar apenas o nome, por enquanto).
2. Implemente a validação de CPF comentada no controller (quebrar a string `236.491.900-28` em partes e validar que são números).
3. Abra um PR com cada exercício (branches separadas!).

> **💡 Dica para o professor:** use o diagrama de camadas como pôster permanente — redesenhe-o no início de cada capítulo marcando qual camada será mexida. O aluno nunca deve perder a noção de "onde estou na arquitetura".

---

# Capítulo 3 — Herança e Classes Abstratas

## 3.1 O problema que queremos resolver

O banco vai oferecer três produtos:

- **Conta Corrente** — para o dia a dia; tem **cheque especial** (pode ficar negativa até um limite) e cobra **taxa de manutenção mensal**.
- **Conta Poupança** — não pode ficar negativa; **rende 0,5% ao mês**.
- **Conta Investimento** — rende mais (**1,5% ao mês**), mas cobra **taxa de administração a cada saque**.

Repare no que as três têm em comum: todas têm nome, CPF e saldo; todas depositam e sacam. E no que diferem: **como** rendem e **como** sacam. Com o que sabemos até agora, faríamos algo assim:

```csharp
public void Sacar(double valor)
{
    if (tipoConta == TipoConta.Corrente) { /* permite negativar até o limite */ }
    else if (tipoConta == TipoConta.Poupanca) { /* não permite negativar */ }
    else if (tipoConta == TipoConta.Investimento) { /* cobra taxa */ }
}
```

E esse mesmo `if/else` se repetiria em `AplicarRendimento()`, em relatórios, em todo lugar. Cada conta nova = mexer em N métodos. Isso é frágil e não escala. A orientação a objetos oferece um caminho melhor.

## 3.2 Herança: "é um"

**Herança** permite que uma classe (filha) receba tudo que outra classe (mãe) tem, e adicione ou modifique o que precisar. A relação se lê como **"é um(a)"**:

- Conta Poupança **é uma** Conta. ✔ herança faz sentido
- Conta **tem um** saldo. ✔ isso é composição (atributo), não herança

Sintaxe em C#:

```csharp
public class ContaPoupanca : Conta   // lê-se: ContaPoupanca herda de Conta
{
    // ganha automaticamente Nome, Cpf, Saldo, Depositar(), Sacar()...
}
```

## 3.3 Classe abstrata: um molde que não vira objeto

Pense no cadastro do banco: ninguém entra numa agência e abre "uma conta" genérica — abre uma conta **corrente**, ou **poupança**, ou **investimento**. "Conta" é um **conceito**, um molde. Em C#, isso se expressa com `abstract`:

```csharp
public abstract class Conta { ... }
```

Uma classe abstrata:

- **não pode ser instanciada**: `new Conta(...)` vira erro de compilação;
- serve de **base** para classes concretas;
- pode ter métodos implementados (herdados por todas as filhas) **e** métodos abstratos — assinaturas sem corpo que **obrigam** cada filha a implementar (veremos no Capítulo 4).

## 3.4 Mão na massa: refatorando o domínio

Vamos evoluir `src/domain/conta.cs`. Primeiro, a base:

```csharp
namespace MeuProjeto.Domain;

public abstract class Conta
{
    public string Nome { get; private set; }
    public string Cpf { get; private set; }
    public double Saldo { get; protected set; }

    protected Conta(string nome, string cpf, double saldo = 0)
    {
        Nome = nome;
        Cpf = cpf;
        Saldo = saldo;
    }

    public void Depositar(double valor)
    {
        if (valor <= 0)
        {
            throw new ArgumentException("O valor do depósito deve ser positivo.");
        }
        Saldo += valor;
    }

    // Sacar e AplicarRendimento entram no Capítulo 4 (polimorfismo)
}
```

Novidades importantes aqui:

- **`protected`** — um terceiro nível de acesso, entre `public` e `private`: só a própria classe **e as filhas** enxergam. `Saldo` tem `protected set` porque a `ContaCorrente` vai precisar alterar o saldo ao sacar, mas o mundo externo continua sem poder fazer `conta.Saldo = 1000000`.
- **Construtor `protected`** — como ninguém pode dar `new Conta(...)`, o construtor só serve para as filhas chamarem.

Agora as filhas, cada uma em seu arquivo (`src/domain/ContaCorrente.cs` etc.):

```csharp
namespace MeuProjeto.Domain;

public class ContaCorrente : Conta
{
    public double LimiteChequeEspecial { get; private set; }

    public ContaCorrente(string nome, string cpf, double saldo = 0, double limite = 500)
        : base(nome, cpf, saldo)          // repassa os dados para o construtor da mãe
    {
        LimiteChequeEspecial = limite;
    }
}

public class ContaPoupanca : Conta
{
    public ContaPoupanca(string nome, string cpf, double saldo = 0)
        : base(nome, cpf, saldo) { }
}

public class ContaInvestimento : Conta
{
    public double TaxaAdministracao { get; private set; }

    public ContaInvestimento(string nome, string cpf, double saldo = 0)
        : base(nome, cpf, saldo)
    {
        TaxaAdministracao = 5.00; // taxa fixa por saque, em reais
    }
}
```

O **`: base(...)`** é o elo entre os construtores: a filha recebe os dados e **repassa** para a mãe inicializar a parte comum. Sem isso, o compilador reclama — uma `ContaCorrente` não existe sem antes existir a parte "Conta" dela.

E o enum `TipoConta`? Ele deixa de representar o tipo (as classes fazem isso agora), mas ainda é útil na **borda** do sistema, para converter o texto digitado pelo usuário:

```csharp
public enum TipoConta { Corrente, Poupanca, Investimento }
```

O controller lê o texto, converte para o enum, e alguém decide qual classe instanciar:

```csharp
// no ContaService (por enquanto; discutiremos onde isso deve morar)
public Conta CriarConta(string nome, string cpf, double saldo, TipoConta tipo)
{
    switch (tipo)
    {
        case TipoConta.Corrente:      return new ContaCorrente(nome, cpf, saldo);
        case TipoConta.Poupanca:      return new ContaPoupanca(nome, cpf, saldo);
        case TipoConta.Investimento:  return new ContaInvestimento(nome, cpf, saldo);
        default: throw new ArgumentException("Tipo de conta inválido.");
    }
}
```

Note que esse é o **único** `switch` sobre tipo de conta que restará no sistema — o ponto de criação. Esse padrão tem nome: **Factory** (fábrica). Não precisa aprofundar agora; basta o aluno perceber que centralizamos a decisão num lugar só.

## 3.5 Exercícios

1. Faça o refactor completo: `Conta` abstrata + três filhas, atualizando `TipoConta` para os novos valores. O projeto deve compilar e o cadastro/consulta do console deve continuar funcionando.
2. Tente escrever `new Conta("a", "b")` em algum lugar e leia a mensagem de erro do compilador. Escreva no PR o que ela significa.
3. Adicione à `ContaCorrente` uma propriedade `SaldoDisponivel` (saldo + limite do cheque especial), calculada: `public double SaldoDisponivel => Saldo + LimiteChequeEspecial;`
4. **Desafio:** o cliente pode ter mais de uma conta? Hoje o CPF identifica a conta. Discuta no PR: qual seria o problema de modelagem e como resolver? (Não precisa implementar — é para pensar em `Cliente` como entidade separada, semente para o capítulo de banco de dados.)

> **💡 Dica para o professor:** desenhe a hierarquia no quadro como árvore genealógica e pergunte "o que a filha ganha de graça? o que ela adiciona?". O erro de compilação do exercício 2 é pedagógico — deixe o aluno ler e interpretar a mensagem sozinho antes de explicar. Outro momento rico: apagar o `: base(...)` e compilar, para ele ver que a mãe *exige* ser inicializada.

---

# Capítulo 4 — Polimorfismo

## 4.1 O conceito

**Polimorfismo** ("muitas formas") é a capacidade de tratar objetos de classes diferentes através de um mesmo tipo comum — e cada objeto responder **do seu jeito**.

Analogia: o gerente do banco grita "apliquem o rendimento do mês!" para a fila de contas. Ele não pergunta o tipo de cada uma — **cada conta sabe** como render: a poupança aplica 0,5%, a investimento 1,5%, a corrente não faz nada. Uma ordem, muitas formas de execução.

Em código, é isto que se torna possível:

```csharp
List<Conta> contas = repository.ListarTodas();   // correntes, poupanças, investimentos misturadas

foreach (Conta conta in contas)
{
    conta.AplicarRendimento();   // cada uma executa a SUA versão. Zero ifs.
}
```

## 4.2 As ferramentas: `virtual`, `override` e `abstract`

**`virtual` + `override`** — a mãe oferece uma implementação **padrão** e a filha pode substituí-la:

```csharp
// na Conta (mãe)
public virtual void Sacar(double valor)   // virtual = "filhas, podem sobrescrever"
{
    if (valor <= 0)
        throw new ArgumentException("O valor do saque deve ser positivo.");
    if (valor > Saldo)
        throw new InvalidOperationException("Saldo insuficiente.");
    Saldo -= valor;
}
```

```csharp
// na ContaCorrente: pode negativar até o limite
public override void Sacar(double valor)
{
    if (valor <= 0)
        throw new ArgumentException("O valor do saque deve ser positivo.");
    if (valor > Saldo + LimiteChequeEspecial)
        throw new InvalidOperationException("Valor excede o limite do cheque especial.");
    Saldo -= valor;
}
```

```csharp
// na ContaInvestimento: cobra taxa de administração por saque
public override void Sacar(double valor)
{
    base.Sacar(valor + TaxaAdministracao);   // reaproveita a regra da mãe!
}
```

A `ContaPoupanca` não sobrescreve nada: a regra padrão da mãe (não negativar) já serve. E repare no `base.Sacar(...)` da investimento: **sobrescrever não é sempre reescrever do zero** — dá para decorar a lógica da mãe.

**`abstract` (método)** — a mãe **não tem** implementação padrão e **obriga** cada filha a fornecer a sua:

```csharp
// na Conta (mãe)
public abstract void AplicarRendimento();
```

```csharp
// ContaCorrente — não rende
public override void AplicarRendimento() { /* conta corrente não rende */ }

// ContaPoupanca — 0,5% ao mês
public override void AplicarRendimento() => Saldo += Saldo * 0.005;

// ContaInvestimento — 1,5% ao mês
public override void AplicarRendimento() => Saldo += Saldo * 0.015;
```

Se uma filha esquecer de implementar um método abstrato, **não compila**. O compilador vira seu fiscal de contrato.

Resumo para colar no caderno:

| Palavra-chave | Na mãe significa | A filha... |
|---|---|---|
| (nada) | "é assim e pronto" | herda e não pode mudar |
| `virtual` | "tenho um padrão, mas aceito variações" | **pode** sobrescrever com `override` |
| `abstract` | "cada uma faz do seu jeito, não tenho padrão" | **é obrigada** a implementar |

## 4.3 O teste de mesa do polimorfismo

Este trecho merece uma execução passo a passo com o aluno:

```csharp
Conta c1 = new ContaPoupanca("Ana", "111.111.111-11", 1000);
Conta c2 = new ContaCorrente("Beto", "222.222.222-22", 1000);

c1.Sacar(1200);   // ??? 
c2.Sacar(1200);   // ???
```

A variável é do tipo `Conta` nos dois casos, mas `c1.Sacar` explode com "Saldo insuficiente" e `c2.Sacar` funciona (usa o cheque especial). **O método executado é decidido pelo objeto real, não pelo tipo da variável.** Essa frase é o coração do polimorfismo.

## 4.4 Mão na massa

1. Adicione `Sacar` (virtual + overrides) e `AplicarRendimento` (abstract + overrides) conforme acima.
2. No menu do console, crie as opções **5 - Depositar**, **6 - Sacar** e **7 - Virada do mês** (aplica rendimento em todas as contas — o loop polimórfico do item 4.1 precisará de um método `ListarTodas()` no repository).
3. Sobrescreva `ToString()` na `Conta` para a consulta imprimir bonito — e note que `ToString`, `Equals` etc. existem porque **toda classe em C# já herda de `object`**. O aluno usa herança desde a primeira aula sem saber:

```csharp
public override string ToString()
    => $"{GetType().Name} | {Nome} | CPF {Cpf} | Saldo R$ {Saldo:F2}";
```

## 4.5 Exercícios

1. Implemente tudo do item 4.4 e abra o PR.
2. Sem rodar, preveja a saída do teste de mesa do item 4.3; depois rode e confira.
3. O banco lançou a **Conta Salário**: não rende, só permite um saque por mês (dica: um atributo contador, zerado na virada do mês — `AplicarRendimento` é um bom gancho... ou seria melhor renomeá-lo para `VirarMes()`? Discuta no PR!). Implemente-a **sem alterar nenhuma linha das outras classes** e observe que o restante do sistema já funciona com ela.
4. **Desafio:** por que `double` é uma escolha ruim para dinheiro? Rode `Console.WriteLine(0.1 + 0.2);` e investigue. Refatore `Saldo` para `decimal` (o tipo correto para valores monetários em C#) e veja o efeito dominó no código.

> **💡 Dica para o professor:** o exercício 3 é o clímax do curso até aqui — quando o aluno perceber que adicionou um tipo novo de conta sem tocar no service, no repository nem nas outras contas, ele entendeu OO. Dê nome ao momento: isso é o princípio **Aberto/Fechado** (aberto para extensão, fechado para modificação), o "O" do SOLID. O exercício 4 (decimal) rende uma aula inteira sobre tipos e é uma dor real do mercado financeiro.

---

# Capítulo 5 — Interfaces: contratos entre as camadas

## 5.1 O conceito

Uma **interface** é um contrato puro: ela declara **o que** deve ser feito, sem dizer **como**. Quem "assina" o contrato (implementa a interface) é obrigado a fornecer todos os métodos declarados.

```csharp
public interface IContaRepository
{
    Conta? BuscarPorCpf(string cpf);
    List<Conta> ListarTodas();
    void Adicionar(Conta conta);
    bool Deletar(string cpf);
}
```

Convenção C#: nome de interface começa com `I`.

"Mas isso parece classe abstrata!" — a comparação é inevitável, então vamos direto a ela:

| | Classe abstrata | Interface |
|---|---|---|
| Relação | **é um** (ContaPoupanca *é uma* Conta) | **sabe fazer / se comporta como** |
| Estado (atributos) | tem (Nome, Saldo...) | não tem |
| Implementação de métodos | pode ter (Depositar) | não tem (só o contrato)* |
| Quantas posso herdar/implementar | herda de **uma** só | implementa **várias** |
| Quando usar | compartilhar código e dados entre parentes próximos | conectar partes independentes do sistema |

*\*C# moderno permite "default interface methods", mas ignore isso no curso — trate interface como contrato puro.*

## 5.2 O problema real do nosso projeto

Olhe esta linha do `ContaService`:

```csharp
ContaRepository repository = new ContaRepository();
```

O service conhece a classe **concreta** e a **constrói** ele mesmo. Consequências:

1. Quando o repositório virar PostgreSQL (Capítulo 6), teremos que **editar o service** — que não deveria nem saber que banco existe.
2. É impossível testar o service isoladamente — ele sempre arrasta o repositório real junto.
3. O acoplamento é como um eletrodoméstico **soldado** na fiação da parede: funciona, mas trocar dá trabalho. Interface é a **tomada**: qualquer aparelho que siga o padrão do plugue encaixa.

## 5.3 Mão na massa: repositório atrás de interface

**Passo 1** — crie `src/repository/IContaRepository.cs` com a interface do item 5.1.

**Passo 2** — a classe atual assina o contrato (e ganha um nome honesto):

```csharp
public class ContaRepositoryEmMemoria : IContaRepository
{
    private static List<Conta> contas = new List<Conta>();

    public Conta? BuscarPorCpf(string cpf)
        => contas.FirstOrDefault(c => c.Cpf == cpf);

    public List<Conta> ListarTodas() => contas;

    public void Adicionar(Conta conta) => contas.Add(conta);

    public bool Deletar(string cpf)
    {
        Conta? conta = BuscarPorCpf(cpf);
        if (conta == null) return false;
        contas.Remove(conta);
        return true;
    }
}
```

**Passo 3** — o service passa a depender **do contrato**, e a dependência entra pelo construtor:

```csharp
public class ContaService
{
    private readonly IContaRepository _repository;

    public ContaService(IContaRepository repository)   // quem cria o service ESCOLHE o repositório
    {
        _repository = repository;
    }

    public Conta? BuscarConta(string cpf) => _repository.BuscarPorCpf(cpf);
    // ... demais métodos usando _repository ...
}
```

Isso tem nome: **injeção de dependência** (a dependência é "injetada" de fora, em vez de construída dentro). E quem decide qual implementação usar? O ponto de entrada do programa — a **composition root**:

```csharp
// Program.cs — o único lugar que conhece as classes concretas
IContaRepository repository = new ContaRepositoryEmMemoria();
ContaService service = new ContaService(repository);
CommandLineController controller = new CommandLineController(service);
```

**Passo 4** — repita o padrão entre controller e service: extraia `IContaService` e faça o controller depender dela.

A arquitetura fica assim — as camadas agora se tocam **apenas através de contratos**:

```
CommandLineController ──► IContaService ◄── ContaService
ContaService ──► IContaRepository ◄── ContaRepositoryEmMemoria   (cap. 6: ContaRepositoryPostgres)
```

## 5.4 A prova de que valeu a pena

No Capítulo 6, esta única linha do `Program.cs`:

```csharp
IContaRepository repository = new ContaRepositoryEmMemoria();
```

virará:

```csharp
IContaRepository repository = new ContaRepositoryPostgres(connectionString);
```

E **nada mais** muda no sistema. Service, controller, domínio: intocados. No Capítulo 7, o framework da API vai fazer essa injeção automaticamente por nós.

## 5.5 Exercícios

1. Faça o refactor completo dos passos 1–4 e abra o PR. O console deve funcionar exatamente como antes (refactor bom não muda comportamento).
2. Crie um `ContaRepositoryFake : IContaRepository` que devolve sempre uma conta fixa "João Teste". Troque **uma linha** do `Program.cs` para usá-lo e mostre o sistema inteiro rodando com dados falsos. (Você acabou de descobrir a base dos testes automatizados.)
3. **Desafio:** a interface `IContaRepository` deveria ter um método `Atualizar(Conta conta)`? Com a lista em memória, alterar o objeto já "salva" (referência!). Com banco de dados, não. Discuta no PR — a resposta aparece no Capítulo 6.

> **💡 Dica para o professor:** a analogia tomada/plugue funciona muito bem visualmente. Outra que ajuda: a interface é a *vaga de emprego* (lista de habilidades exigidas) e a classe é o *candidato contratado* — o service trabalha com "quem quer que ocupe a vaga". O exercício 2 costuma provocar o "clique" da injeção de dependência: uma linha trocada, sistema inteiro mudou de comportamento.

---

# Capítulo 6 — Banco de dados: PostgreSQL com Docker

## 6.1 O problema

Feche o programa e abra de novo: todas as contas sumiram. A `List<Conta>` vive na memória RAM do processo — morreu o processo, morreram os dados. Um banco de dados guarda os dados **em disco**, de forma organizada, consultável e compartilhável entre programas. Usaremos o **PostgreSQL**, um dos bancos relacionais mais usados do mundo (e gratuito/open source).

## 6.2 Docker em 10 minutos

Instalar um banco "na mão" é chato e cada máquina fica diferente. O **Docker** resolve isso: ele roda programas em **containers** — caixas isoladas e descartáveis com tudo que o programa precisa dentro.

- **Imagem** = a receita do bolo (ex.: `postgres:16`).
- **Container** = o bolo assado, rodando.

Crie um `docker-compose.yml` na raiz do projeto:

```yaml
services:
  postgres:
    image: postgres:16
    container_name: banco-postgres
    environment:
      POSTGRES_USER: banco
      POSTGRES_PASSWORD: banco123
      POSTGRES_DB: sistema_bancario
    ports:
      - "5432:5432"          # porta_da_sua_maquina : porta_do_container
    volumes:
      - dados_postgres:/var/lib/postgresql/data   # os dados sobrevivem ao container

volumes:
  dados_postgres:
```

Comandos do dia a dia:

```bash
docker compose up -d      # sobe o banco em segundo plano
docker compose ps         # está rodando?
docker compose logs postgres
docker compose down       # derruba (os dados ficam, por causa do volume)
```

## 6.3 SQL essencial e a nossa tabela

Entre no banco pelo terminal do container:

```bash
docker exec -it banco-postgres psql -U banco -d sistema_bancario
```

Crie a tabela (guarde este script em `scripts/criar-tabelas.sql` no repo):

```sql
CREATE TABLE conta (
    id          SERIAL PRIMARY KEY,
    cpf         VARCHAR(14) NOT NULL UNIQUE,
    nome        VARCHAR(100) NOT NULL,
    saldo       NUMERIC(15, 2) NOT NULL DEFAULT 0,
    tipo_conta  VARCHAR(20) NOT NULL
);
```

Ponto de discussão importante com o aluno: **o banco não conhece herança**. No C# temos três classes; na tabela, uma coluna `tipo_conta` com o nome do tipo. Guardar objetos de uma hierarquia em tabelas se chama *mapeamento objeto-relacional*, e a nossa estratégia (uma tabela única + coluna de tipo) é a mais simples. Na leitura, usaremos essa coluna para decidir qual classe instanciar — a **Factory** do Capítulo 3 finalmente mostra todo o seu valor.

O básico de SQL para brincar no `psql`:

```sql
INSERT INTO conta (cpf, nome, saldo, tipo_conta) VALUES ('111.111.111-11', 'Ana', 1000, 'Poupanca');
SELECT * FROM conta;
SELECT * FROM conta WHERE cpf = '111.111.111-11';
UPDATE conta SET saldo = 1500 WHERE cpf = '111.111.111-11';
DELETE FROM conta WHERE cpf = '111.111.111-11';
```

## 6.4 Mão na massa: ContaRepositoryPostgres

O C# conversa com o Postgres através do pacote **Npgsql**:

```bash
dotnet add package Npgsql
```

E agora a recompensa do Capítulo 5 — uma implementação **nova** do **mesmo contrato**:

```csharp
using Npgsql;

public class ContaRepositoryPostgres : IContaRepository
{
    private readonly string _connectionString;

    public ContaRepositoryPostgres(string connectionString)
    {
        _connectionString = connectionString;
    }

    public Conta? BuscarPorCpf(string cpf)
    {
        using var conexao = new NpgsqlConnection(_connectionString);
        conexao.Open();

        using var comando = new NpgsqlCommand(
            "SELECT cpf, nome, saldo, tipo_conta FROM conta WHERE cpf = @cpf", conexao);
        comando.Parameters.AddWithValue("cpf", cpf);

        using var leitor = comando.ExecuteReader();
        if (!leitor.Read()) return null;

        return MapearConta(leitor);
    }

    public List<Conta> ListarTodas()
    {
        var contas = new List<Conta>();
        using var conexao = new NpgsqlConnection(_connectionString);
        conexao.Open();

        using var comando = new NpgsqlCommand(
            "SELECT cpf, nome, saldo, tipo_conta FROM conta", conexao);
        using var leitor = comando.ExecuteReader();

        while (leitor.Read())
        {
            contas.Add(MapearConta(leitor));
        }
        return contas;
    }

    public void Adicionar(Conta conta)
    {
        using var conexao = new NpgsqlConnection(_connectionString);
        conexao.Open();

        using var comando = new NpgsqlCommand(
            @"INSERT INTO conta (cpf, nome, saldo, tipo_conta)
              VALUES (@cpf, @nome, @saldo, @tipo)", conexao);
        comando.Parameters.AddWithValue("cpf", conta.Cpf);
        comando.Parameters.AddWithValue("nome", conta.Nome);
        comando.Parameters.AddWithValue("saldo", conta.Saldo);
        comando.Parameters.AddWithValue("tipo", conta.GetType().Name.Replace("Conta", ""));

        comando.ExecuteNonQuery();
    }

    public bool Deletar(string cpf)
    {
        using var conexao = new NpgsqlConnection(_connectionString);
        conexao.Open();

        using var comando = new NpgsqlCommand(
            "DELETE FROM conta WHERE cpf = @cpf", conexao);
        comando.Parameters.AddWithValue("cpf", cpf);

        return comando.ExecuteNonQuery() > 0;   // quantas linhas foram afetadas?
    }

    // a Factory em ação: a coluna tipo_conta decide qual classe instanciar
    private static Conta MapearConta(NpgsqlDataReader leitor)
    {
        string cpf   = leitor.GetString(0);
        string nome  = leitor.GetString(1);
        double saldo = (double)leitor.GetDecimal(2);
        string tipo  = leitor.GetString(3);

        return tipo switch
        {
            "Corrente"     => new ContaCorrente(nome, cpf, saldo),
            "Poupanca"     => new ContaPoupanca(nome, cpf, saldo),
            "Investimento" => new ContaInvestimento(nome, cpf, saldo),
            _ => throw new InvalidOperationException($"Tipo desconhecido: {tipo}")
        };
    }
}
```

Conceitos novos para destacar:

- **Connection string** — o "endereço + credenciais" do banco: `Host=localhost;Port=5432;Database=sistema_bancario;Username=banco;Password=banco123`. Em projeto real ela **nunca** vai no código-fonte (vai em variável de ambiente / arquivo de configuração fora do Git!).
- **`using var`** — garante que a conexão será fechada mesmo se der erro (é o `try/finally` disfarçado).
- **Parâmetros (`@cpf`)** — *jamais* monte SQL concatenando strings (`"WHERE cpf = '" + cpf + "'"`). Além de feio, abre a porta para **SQL Injection**, o ataque mais clássico da web. Mostre o quadrinho do "Bobby Tables" (xkcd 327) para o aluno nunca mais esquecer.

E a troca prometida no `Program.cs`:

```csharp
string connectionString = "Host=localhost;Port=5432;Database=sistema_bancario;Username=banco;Password=banco123";
IContaRepository repository = new ContaRepositoryPostgres(connectionString);
```

Service, controller, domínio: **nenhuma linha alterada.**

## 6.5 O método Atualizar (resposta ao desafio do Capítulo 5)

Com a lista em memória, mexer no objeto bastava. Com o banco, o objeto em C# e a linha na tabela são cópias independentes — é preciso salvar de volta. Adicione ao contrato e às duas implementações:

```csharp
// na interface
void Atualizar(Conta conta);

// no Postgres
public void Atualizar(Conta conta)
{
    using var conexao = new NpgsqlConnection(_connectionString);
    conexao.Open();
    using var comando = new NpgsqlCommand(
        "UPDATE conta SET nome = @nome, saldo = @saldo WHERE cpf = @cpf", conexao);
    comando.Parameters.AddWithValue("nome", conta.Nome);
    comando.Parameters.AddWithValue("saldo", conta.Saldo);
    comando.Parameters.AddWithValue("cpf", conta.Cpf);
    comando.ExecuteNonQuery();
}
```

Agora o service chama `_repository.Atualizar(conta)` depois de `Depositar`/`Sacar`/`AplicarRendimento`.

## 6.6 Exercícios

1. Suba o Postgres com Docker, crie a tabela, implemente o `ContaRepositoryPostgres` e troque a linha do `Program.cs`. Cadastre contas, **feche o programa, abra de novo** e consulte-as: elas sobreviveram! Abra o PR (com o `docker-compose.yml` e o script SQL inclusos).
2. Derrube o container com `docker compose down` e suba de novo. Os dados sumiram? Por quê? E se rodar `docker compose down -v`?
3. No `psql`, escreva uma consulta que some o saldo de todas as contas por tipo (`GROUP BY tipo_conta`).
4. **Desafio:** cadastre dois clientes com o mesmo CPF. O banco reage (graças ao `UNIQUE`)! Trate esse erro no service para exibir uma mensagem amigável em vez de deixar o programa quebrar (dica: `try/catch` de `PostgresException`).

> **💡 Dica para o professor:** reserve uma aula só para Docker + SQL puro no `psql`, sem C# nenhum — o aluno precisa "ver" a tabela existindo fora do programa antes de conectar as pontas. O momento "fechei o programa e os dados continuam lá" merece celebração: é a primeira vez que o software dele tem memória de verdade. Sobre EF Core: mencione que existe (e que é o caminho mais comum no mercado), mas fazer o SQL na mão uma vez na vida ensina o que o ORM esconde.

---

# Capítulo 7 — Virando uma API com ASP.NET Core

## 7.1 Por que uma API?

O nosso sistema só funciona para quem está com o teclado na frente do console. Um banco de verdade tem app no celular, site, caixa eletrônico — **vários clientes, um mesmo cérebro**. A solução é expor as operações como uma **API web**: um serviço que recebe requisições HTTP e responde dados (em JSON).

O framework para isso no mundo .NET é o **ASP.NET Core** — é o framework web oficial e de longe o mais utilizado pela comunidade .NET, mantido pela Microsoft, open source e multiplataforma.

## 7.2 REST em cinco linhas

Uma API REST organiza as operações em **recursos** (substantivos, no plural) e **verbos HTTP**:

| Verbo + rota | Significado | No nosso sistema |
|---|---|---|
| `GET /contas` | listar | listar todas as contas |
| `GET /contas/{cpf}` | buscar um | consultar conta por CPF |
| `POST /contas` | criar | cadastrar conta |
| `PUT /contas/{cpf}` | atualizar | atualizar dados da conta |
| `DELETE /contas/{cpf}` | remover | excluir conta |
| `POST /contas/{cpf}/depositos` | ação sobre o recurso | depositar |

E cada resposta traz um **status code**: `200 OK`, `201 Created`, `404 Not Found`, `400 Bad Request`.

## 7.3 Criando o projeto Web API

A forma mais didática é criar o projeto de API ao lado e mover as camadas para dentro (elas já estão prontas — foi para isso que trabalhamos!):

```bash
dotnet new webapi -o MeuBanco.Api --use-controllers
cd MeuBanco.Api
dotnet add package Npgsql
```

Copie as pastas `domain`, `service` e `repository` para o novo projeto (o `Program.cs` do console e o `CommandLineController` ficam para trás — o controller de console será substituído por um controller HTTP).

## 7.4 A injeção de dependência agora é nativa

Lembra da nossa *composition root* artesanal? O ASP.NET Core tem um **container de injeção de dependência** embutido. No `Program.cs` da API:

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

// registra os contratos e suas implementações — a "tomada e o aparelho"
string connectionString = builder.Configuration.GetConnectionString("Postgres")!;
builder.Services.AddScoped<IContaRepository>(_ => new ContaRepositoryPostgres(connectionString));
builder.Services.AddScoped<IContaService, ContaService>();

var app = builder.Build();

app.MapControllers();
app.Run();
```

E a connection string sai do código e vai para o `appsettings.json` (configuração!):

```json
{
  "ConnectionStrings": {
    "Postgres": "Host=localhost;Port=5432;Database=sistema_bancario;Username=banco;Password=banco123"
  }
}
```

Quando alguém pedir um `ContasController`, o framework enxerga que ele precisa de um `IContaService`, que precisa de um `IContaRepository`, e **monta a cadeia inteira sozinho**. Todo o Capítulo 5 foi a preparação para esta mágica fazer sentido.

## 7.5 O controller HTTP

`Controllers/ContasController.cs` — compare-o mentalmente com o `CommandLineController`: mesmo papel (borda do sistema, traduz o mundo externo para o service), outro "idioma" (HTTP/JSON em vez de Console.ReadLine):

```csharp
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("contas")]
public class ContasController : ControllerBase
{
    private readonly IContaService _service;

    public ContasController(IContaService service)   // injetado pelo framework!
    {
        _service = service;
    }

    [HttpGet]
    public ActionResult<List<Conta>> ListarTodas()
        => Ok(_service.ListarTodas());

    [HttpGet("{cpf}")]
    public ActionResult<Conta> BuscarPorCpf(string cpf)
    {
        Conta? conta = _service.BuscarConta(cpf);
        if (conta == null) return NotFound();   // 404
        return Ok(conta);                       // 200
    }

    [HttpPost]
    public ActionResult Criar(CriarContaRequest request)
    {
        Conta conta = _service.CriarConta(
            request.Nome, request.Cpf, request.SaldoInicial, request.Tipo);

        return CreatedAtAction(nameof(BuscarPorCpf), new { cpf = conta.Cpf }, conta);  // 201
    }

    [HttpPost("{cpf}/depositos")]
    public ActionResult Depositar(string cpf, OperacaoRequest request)
    {
        try
        {
            _service.Depositar(cpf, request.Valor);
            return NoContent();                       // 204
        }
        catch (ArgumentException e)
        {
            return BadRequest(new { erro = e.Message });   // 400
        }
    }

    [HttpDelete("{cpf}")]
    public ActionResult Deletar(string cpf)
        => _service.DeletarConta(cpf) ? NoContent() : NotFound();
}
```

E os **DTOs** (Data Transfer Objects) — as "fichas de formulário" que o cliente da API preenche. Note que o mundo externo **não** cria objetos `Conta` diretamente:

```csharp
public record CriarContaRequest(string Nome, string Cpf, double SaldoInicial, TipoConta Tipo);
public record OperacaoRequest(double Valor);
```

(`record` é uma forma curta de declarar classes que só carregam dados — apresente como "classe de uma linha".)

Um detalhe prático: para o JSON aceitar `"tipo": "Poupanca"` como texto (em vez de exigir o número do enum), registre o conversor de enums no `Program.cs`:

```csharp
builder.Services.AddControllers().AddJsonOptions(o =>
    o.JsonSerializerOptions.Converters.Add(new System.Text.Json.Serialization.JsonStringEnumConverter()));
```

## 7.6 Rodando e testando

```bash
docker compose up -d    # o banco precisa estar de pé
dotnet run
```

O template já vem com **OpenAPI/Swagger** habilitado em desenvolvimento — abra `http://localhost:5xxx/swagger` no navegador: uma página que lista os endpoints e permite testá-los com cliques. Para testar via terminal:

```bash
curl -X POST http://localhost:5xxx/contas \
     -H "Content-Type: application/json" \
     -d '{"nome": "Ana", "cpf": "111.111.111-11", "saldoInicial": 1000, "tipo": "Poupanca"}'

curl http://localhost:5xxx/contas/111.111.111-11
```

Um dos momentos didáticos mais fortes do curso: mostrar que **o navegador, o curl e o Swagger** são três "clientes" diferentes falando com o mesmo cérebro que antes só o console acessava.

## 7.7 Exercícios

1. Crie o projeto da API, mova as camadas, configure a DI e implemente `GET /contas`, `GET /contas/{cpf}`, `POST /contas` e `DELETE /contas/{cpf}`. Teste tudo pelo Swagger e abra o PR (inclua no PR os comandos curl que você usou para testar).
2. Adicione `POST /contas/{cpf}/saques` — e trate o `InvalidOperationException` de saldo insuficiente com resposta `400` e mensagem amigável em JSON.
3. Adicione `POST /contas/virada-do-mes` que aplica o rendimento em todas as contas (o loop polimórfico agora acionado por HTTP!).
4. **Desafio:** o `GET /contas/{cpf}` devolve o objeto `Conta` inteiro serializado. Crie um `ContaResponse` (DTO de saída) que expõe também o `SaldoDisponivel` quando a conta for corrente. Pesquise: por que APIs reais não expõem a entidade de domínio diretamente?

> **💡 Dica para o professor:** antes de escrever qualquer código, gaste 20 minutos com o aluno usando uma API pública real no navegador (ex.: `https://viacep.com.br/ws/01001000/json/`) para ele *sentir* o que é requisição/resposta JSON. Ao apresentar o ContasController, projete lado a lado o CommandLineController antigo — a percepção de que "só trocamos a borda, o miolo é o mesmo" é a síntese de todo o curso.

---

# Capítulo 8 — Próximos passos

O aluno que chegou aqui tem um sistema com domínio orientado a objetos, camadas desacopladas por interfaces, persistência real e uma API HTTP. Caminhos naturais de continuação, em ordem sugerida:

1. **Testes automatizados (xUnit)** — o `ContaRepositoryFake` do Capítulo 5 já plantou a semente: testar o `ContaService` injetando um repositório falso. É o próximo capítulo natural e consolida o valor das interfaces.
2. **Entity Framework Core** — o ORM padrão do mercado .NET: substitui o SQL manual do repositório por `DbContext` e LINQ, com *migrations* para versionar o schema. Excelente exercício: criar `ContaRepositoryEfCore : IContaRepository` — a terceira implementação do mesmo contrato!
3. **Validações mais ricas** — validação de CPF real (dígitos verificadores), FluentValidation nos DTOs.
4. **Entidade Cliente** — separar cliente de conta (1 cliente → N contas), com chave estrangeira no banco: a porta de entrada para modelagem relacional.
5. **Autenticação** — JWT: quem pode ver e movimentar cada conta?
6. **Deploy** — colocar a própria API num container Docker (Dockerfile) ao lado do Postgres no compose; depois, publicar em um serviço de nuvem.

---

## Apêndice A — Glossário rápido

| Termo | Definição de bolso |
|---|---|
| Herança | Classe filha recebe atributos e métodos da classe mãe (relação "é um") |
| Classe abstrata | Molde que não pode ser instanciado; base para classes concretas |
| Método abstrato | Assinatura sem corpo; as filhas são obrigadas a implementar |
| `virtual` / `override` | A mãe oferece um padrão; a filha pode substituir |
| Polimorfismo | Tratar objetos diferentes pelo tipo comum; cada um responde do seu jeito |
| Interface | Contrato puro: o "o quê" sem o "como" |
| Injeção de dependência | Receber as dependências de fora (construtor) em vez de criá-las dentro |
| Composition root | O único lugar do sistema que conhece as classes concretas e monta tudo |
| DTO | Objeto simples que carrega dados entre a borda (API) e o sistema |
| ORM | Biblioteca que mapeia objetos ↔ tabelas (ex.: EF Core) |
| REST | Estilo de API: recursos + verbos HTTP + status codes |

## Apêndice B — Comandos que mais usaremos

```bash
# .NET
dotnet run                  # roda o projeto
dotnet build                # compila
dotnet add package X        # adiciona uma biblioteca

# Git
git checkout -b feature/x   # nova branch
git add . && git commit -m "mensagem"
git push origin feature/x

# Docker
docker compose up -d        # sobe os serviços
docker compose down         # derruba
docker exec -it banco-postgres psql -U banco -d sistema_bancario   # terminal SQL
```

---

*Apostila gerada para apoiar as aulas particulares de C#/OO baseadas no projeto open source [meu-projeto-dotnet](https://github.com/fabioBaraDev/meu-projeto-dotnet). Sugestões e correções: abra uma issue — ou melhor, um Pull Request.*
