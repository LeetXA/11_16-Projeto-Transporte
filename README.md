
## Como executar
No terminal, dentro da pasta do projeto:

```bash
dotnet restore
dotnet run
```

## Dados iniciais
O programa inicia com:
- 8 veículos numerados de 1 a 8;
- garagem Congonhas;
- garagem Guarulhos;
- capacidade inicial de 15 passageiros por van.


## Regras implementadas
- Cada garagem possui uma pilha de veículos (`Stack<Veiculo>`).
- O último veículo estacionado é o primeiro a sair (LIFO).
- A jornada precisa ser iniciada antes de liberar viagens.
- No início da jornada, os veículos são distribuídos alternadamente entre as garagens.
- Cadastros de veículos e garagens só são permitidos com a jornada encerrada.
- A opção de liberar viagem registra passageiros na van do topo da pilha. A viagem só é realizada quando a lotação fica completa.
- Ao viajar, o veículo sai da pilha de origem e entra no topo da pilha de destino.
- Ao encerrar a jornada, é exibido o total transportado por veículo e os registros da jornada são limpos.

## Diagrama de classes

```mermaid
classDiagram
    class Veiculo {
        +int Numero
        +int Capacidade
        +int PassageirosNaViagem
        +int TotalPassageirosJornada
        +AdicionarPassageiros(quantidade) bool
        +ConcluirViagem() int
        +EncerrarJornada()
    }

    class Garagem {
        +string Nome
        -Stack~Veiculo~ veiculos
        +QuantidadeVeiculos int
        +PotencialTransporte int
        +Estacionar(veiculo)
        +RetirarUltimo() Veiculo
        +ConsultarTopo() Veiculo
        +ListarVeiculos()
    }

    class Viagem {
        +int NumeroVeiculo
        +string Origem
        +string Destino
        +int Passageiros
        +DateTime DataHora
    }

    class ControleTransporte {
        -List~Veiculo~ veiculos
        -List~Garagem~ garagens
        -List~Viagem~ viagens
        -bool jornadaIniciada
        +CarregarDadosIniciais()
        +Executar()
        +IniciarJornada()
        +EncerrarJornada()
        +LiberarViagem()
    }

    Garagem "1" o-- "0..*" Veiculo : pilha de veículos
    ControleTransporte "1" o-- "*" Garagem
    ControleTransporte "1" o-- "*" Veiculo
    ControleTransporte "1" o-- "*" Viagem
    Viagem --> Veiculo : identifica
```