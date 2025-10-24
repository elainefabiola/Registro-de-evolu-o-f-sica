# Diagrama de Classes - Módulo Registro de evolucao fisica (Arquitetura MVC)

## Diagrama de Classes - Arquitetura MVC


```mermaid
classDiagram
    %% ===== DADOS (MODEL) =====
    %% Aqui ficam as informações que o sistema guarda
    
    %% Estilo para classe Aluno (cor vermelha)
    classDef alunoClass fill:#ff6b6b,stroke:#d63031,stroke-width:2px,color:#fff
    
    class AvaliacaoFisica {
        -int id
        -Aluno Aluno
        -Date dataAvaliacao
        -String observacoes
        -boolean completa
        +getId()
        +getNomeAluno()
        +getDataAvaliacao()
        +getObservacoes()
        +isCompleta()
        +setId(id)
        +setNomeAluno(nome)
        +setDataAvaliacao(data)
        +setObservacoes(obs)
        +setCompleta(completa)
    }

    class Aluno {
        -int id
        -String nome
        -String email
        -Date dataNascimento
        -String telefone
        -boolean ativo
        +getId()
        +getNome()
        +getEmail()
        +getDataNascimento()
        +getTelefone()
        +isAtivo()
        +setId(id)
        +setNome(nome)
        +setEmail(email)
        +setDataNascimento(data)
        +setTelefone(telefone)
        +setAtivo(ativo)
    }
    
    %% Aplicar estilo vermelho à classe Aluno
    class Aluno alunoClass
 

    class MedidasCorporais {
        -int id
        -float peso
        -float altura
        -float cintura
        -float quadril
        -float braco
        -float coxa
        +getId()
        +getPeso()
        +getAltura()
        +getCintura()
        +getQuadril()
        +getBraco()
        +getCoxa()
        +setId(id)
        +setPeso(peso)
        +setAltura(altura)
        +setCintura(cintura)
        +setQuadril(quadril)
        +setBraco(braco)
        +setCoxa(coxa)
    }

    class Relatorio {
        -int id
        -Aluno nomeAluno
        -Date dataGeracao
        +getId()
        +getNomeAluno()
        +getDataGeracao()
        +setId(id)
        +setNomeAluno(nome)
        +setDataGeracao(data)
    }

    %% ===== LÓGICA DE NEGÓCIO (SERVICES) =====
    %% Aqui ficam os cálculos e validações
    
    class CalculadoraIMC {
        +calcularIMC(peso, altura)
        +classificarIMC(imc)
        +getFaixasIMC()
        +validarParametros(peso, altura)
    }

    class ValidadorDados {
        +validarPeso(peso)
        +validarAltura(altura)
        +validarMedidas(medidas)
        +validarFormatoNumerico(valor)
        +validarFaixaValores(valor, min, max)
    }

    %% ===== CONTROLE (CONTROLLER) =====
    %% Aqui ficam as operações principais
    
    class SistemaController {
        -CalculadoraIMC calculadoraIMC
        -ValidadorDados validadorDados
        -boolean sistemaInicializado
        -String usuarioLogado
        +criarAvaliacao()
        +buscarAvaliacao()
        +atualizarAvaliacao()
        +excluirAvaliacao()
        +gerarRelatorio()
        +exportarPDF()
        +inicializarSistema()
        +autenticarUsuario()
        +validarPermissoes()
        +coordenarOperacoes()
        +getCalculadoraIMC()
        +getValidadorDados()
        +isSistemaInicializado()
        +getUsuarioLogado()
    }

    %% ===== INTERFACE (VIEW) =====
    %% Aqui ficam as telas que o usuário vê
    
    class TelaAvaliacao {
        -SistemaController controller
        -boolean formularioVisivel
        -String mensagemAtual
        +exibirFormulario()
        +exibirDados()
        +exibirMensagem()
        +ocultarFormulario()
        +limparCampos()
        +getController()
        +isFormularioVisivel()
        +getMensagemAtual()
    }

    class TelaRelatorio {
        -SistemaController controller
        -boolean relatorioCarregado
        -String formatoAtual
        +exibirRelatorio()
        +exportarRelatorio()
        +getController()
        +isRelatorioCarregado()
        +getFormatoAtual()
    }

    class TelaPrincipal {
        -SistemaController controller
        -boolean menuAberto
        -String telaAtual
        +exibirMenu()
        +exibirDashboard()
        +exibirLogin()
        +exibirNavegacao()
        +fecharMenu()
        +trocarTela(novaTela)
        +getController()
        +isMenuAberto()
        +getTelaAtual()
    }

    %% ===== RELACIONAMENTOS MVC COM CARDINALIDADES =====
    
    %% Relacionamentos de COMPOSIÇÃO (Composição - forte dependência)
    %% Uma AvaliacaoFisica É COMPOSTA POR MedidasCorporais
    %% CORREÇÃO: MedidasCorporais não pode existir sem AvaliacaoFisica
    AvaliacaoFisica "1" *-- "1" MedidasCorporais : "composição<br/>(tem-um)"
    
    %% Relacionamentos de AGREGAÇÃO (Agregação - dependência média)
    %% CORREÇÃO: Services podem existir independentemente do Controller
    SistemaController "1" o-- "1" CalculadoraIMC : "agregação<br/>(usa)"
    SistemaController "1" o-- "1" ValidadorDados : "agregação<br/>(usa)"
    
    %% Relacionamentos de ASSOCIAÇÃO (Associação - relacionamento fraco)
    %% CORREÇÃO: Relatorio pode existir independentemente de AvaliacaoFisica
    AvaliacaoFisica "1" --> "0..*" Relatorio : "associação<br/>(gera)"
    
    %% Relacionamentos Aluno (Agregação)
    %% CORREÇÃO: AvaliacaoFisica CONTÉM uma instância de Aluno
    AvaliacaoFisica "1" o-- "1" Aluno : "agregação<br/>(contém)"
    %% CORREÇÃO: Relatorio CONTÉM uma instância de Aluno
    Relatorio "1" o-- "1" Aluno : "agregação<br/>(contém)"
    
    %% Relacionamentos Controller -> Model (Associações)
    %% CORREÇÃO: Controller gerencia mas não possui os modelos
    SistemaController "1" --> "0..*" AvaliacaoFisica : "associação<br/>(gerencia)"
    SistemaController "1" --> "0..*" MedidasCorporais : "associação<br/>(manipula)"
    SistemaController "1" --> "0..*" Relatorio : "associação<br/>(gerencia)"
    SistemaController "1" --> "0..*" Aluno : "associação<br/>(gerencia)"
    
    %% Relacionamentos View -> Controller (Associações)
    %% CORREÇÃO: Views dependem do Controller para funcionar
    TelaAvaliacao "1" --> "1" SistemaController : "associação<br/>(comunica)"
    TelaRelatorio "1" --> "1" SistemaController : "associação<br/>(comunica)"
    TelaPrincipal "1" --> "1" SistemaController : "associação<br/>(comunica)"
    
    %% Relacionamentos Service -> Model (Dependências)
    %% CORREÇÃO: Services dependem temporariamente dos modelos
    CalculadoraIMC "1" ..> "0..*" MedidasCorporais : "dependência<br/>(calcula)"
    ValidadorDados "1" ..> "0..*" MedidasCorporais : "dependência<br/>(valida)"

    %% Notas explicativas
    note for Aluno "Representa um aluno<br/>do sistema (Não será implemtada)"
    
    note for AvaliacaoFisica "Armazena informações<br/>básicas da avaliação"
    
    note for MedidasCorporais "Guarda todas as<br/>medidas do corpo"
    
    note for CalculadoraIMC "Faz os cálculos<br/>matemáticos"
    
    note for SistemaController "Controla todo o<br/>sistema MVC"
```

## Arquitetura MVC - Descrição das Camadas

### **CAMADA MODEL (MODELOS + SERVIÇOS + UTILITÁRIOS)**

### 🗂️ **DADOS (Model)**
São como "gavetas" onde guardamos as informações:
- **AvaliacaoFisica**: Guarda informações básicas (aluno, data, observações)
- **MedidasCorporais**: Guarda todas as medidas do corpo (peso, altura, cintura, etc.)
- **Relatorio**: Guarda informações dos relatórios gerados

### ⚙️ **LÓGICA DE NEGÓCIO (Services)**
São como "calculadoras inteligentes" que fazem os cálculos:

- **CalculadoraIMC**: Calcula o IMC e diz se está normal, acima do peso, etc.
- **ValidadorDados**: Verifica se os dados estão corretos (peso não pode ser negativo, etc.)

### 🎮 **CONTROLE (Controller)**
É como um "gerente geral" que coordena tudo:

- **SistemaController**: Controla todo o sistema - avaliações, relatórios, autenticação e coordenação geral

### 🖥️ **INTERFACE (View)**
São as telas que o usuário vê e usa:

- **TelaAvaliacao**: Tela para preencher dados da avaliação
- **TelaRelatorio**: Tela para ver relatórios e gráficos
- **TelaPrincipal**: Tela principal com menu, dashboard e navegação

## Como Funciona na Prática?

1. **Usuário** acessa o sistema através da **TelaPrincipal**
2. **TelaPrincipal** comunica com o **SistemaController** para autenticação
3. **SistemaController** valida permissões e coordena acesso
4. **SistemaController** busca dados do **Aluno** (classe vermelha) no sistema
5. **Usuário** preenche dados na **TelaAvaliacao**
6. **TelaAvaliacao** envia dados para o **SistemaController**
7. **SistemaController** pede para o **ValidadorDados** verificar se está tudo certo
8. **SistemaController** pede para o **CalculadoraIMC** calcular o IMC
9. **SistemaController** salva os dados na **AvaliacaoFisica** e **MedidasCorporais**
10. **SistemaController** avisa a **TelaAvaliacao** que deu tudo certo
11. **TelaRelatorio** solicita relatório ao **SistemaController**
12. **SistemaController** gera relatório baseado nos dados salvos do **Aluno**

## 📊 Tabela de Relacionamentos de Classes e Cardinalidades

### **Resumo Completo dos Relacionamentos**

| Classe Origem | Relacionamento | Classe Destino | Cardinalidade | Tipo | Descrição |
|---------------|----------------|----------------|---------------|------|-----------|
| **AvaliacaoFisica** | `*--` | **MedidasCorporais** | 1:1 | COMPOSIÇÃO | Uma avaliação É COMPOSTA POR uma medida corporal |
| **AvaliacaoFisica** | `o--` | **Aluno** | 1:1 | AGREGAÇÃO | Uma avaliação CONTÉM um aluno |
| **AvaliacaoFisica** | `-->` | **Relatorio** | 1:N | ASSOCIAÇÃO | Uma avaliação GERA zero ou muitos relatórios |
| **Relatorio** | `o--` | **Aluno** | 1:1 | AGREGAÇÃO | Um relatório CONTÉM um aluno |
| **SistemaController** | `o--` | **CalculadoraIMC** | 1:1 | AGREGAÇÃO | Um controller USA uma calculadora |
| **SistemaController** | `o--` | **ValidadorDados** | 1:1 | AGREGAÇÃO | Um controller USA um validador |
| **SistemaController** | `-->` | **AvaliacaoFisica** | 1:N | ASSOCIAÇÃO | Um controller GERENCIA zero ou muitas avaliações |
| **SistemaController** | `-->` | **MedidasCorporais** | 1:N | ASSOCIAÇÃO | Um controller MANIPULA zero ou muitas medidas |
| **SistemaController** | `-->` | **Relatorio** | 1:N | ASSOCIAÇÃO | Um controller GERENCIA zero ou muitos relatórios |
| **SistemaController** | `-->` | **Aluno** | 1:N | ASSOCIAÇÃO | Um controller GERENCIA zero ou muitos alunos |
| **TelaAvaliacao** | `-->` | **SistemaController** | 1:1 | ASSOCIAÇÃO | Uma tela COMUNICA com um controller |
| **TelaRelatorio** | `-->` | **SistemaController** | 1:1 | ASSOCIAÇÃO | Uma tela COMUNICA com um controller |
| **TelaPrincipal** | `-->` | **SistemaController** | 1:1 | ASSOCIAÇÃO | Uma tela COMUNICA com um controller |
| **CalculadoraIMC** | `..>` | **MedidasCorporais** | 1:N | DEPENDÊNCIA | Uma calculadora CALCULA zero ou muitas medidas |
| **ValidadorDados** | `..>` | **MedidasCorporais** | 1:N | DEPENDÊNCIA | Um validador VALIDA zero ou muitas medidas |

### **Legenda dos Símbolos UML**

| Símbolo | Nome | Descrição | Força da Dependência |
|---------|------|-----------|---------------------|
| `*--` | **Composição** | "É composto por" - dependência forte | 🔴 **Forte** |
| `o--` | **Agregação** | "Contém" ou "Usa" - dependência média | 🟡 **Média** |
| `-->` | **Associação** | "Relaciona-se com" - dependência fraca | 🟢 **Fraca** |
| `..>` | **Dependência** | "Usa temporariamente" - dependência muito fraca | ⚪ **Muito Fraca** |

### **Tipos de Cardinalidades**

| Cardinalidade | Símbolo | Descrição | Exemplo |
|---------------|---------|-----------|---------|
| **Um para Um** | 1:1 | Exatamente um objeto | AvaliacaoFisica ↔ MedidasCorporais |
| **Um para Muitos** | 1:N | Um objeto para zero ou muitos | Aluno ↔ AvaliacaoFisica |
| **Muitos para Muitos** | N:M | Muitos objetos para muitos | (Não aplicável no sistema atual) |

### **Resumo por Tipo de Relacionamento**

#### **🔴 COMPOSIÇÃO (1 relacionamento)**
- `AvaliacaoFisica *-- MedidasCorporais` (1:1)

#### **🟡 AGREGAÇÃO (4 relacionamentos)**
- `AvaliacaoFisica o-- Aluno` (1:1)
- `Relatorio o-- Aluno` (1:1)
- `SistemaController o-- CalculadoraIMC` (1:1)
- `SistemaController o-- ValidadorDados` (1:1)

#### **🟢 ASSOCIAÇÃO (8 relacionamentos)**
- `AvaliacaoFisica --> Relatorio` (1:N)
- `SistemaController --> AvaliacaoFisica` (1:N)
- `SistemaController --> MedidasCorporais` (1:N)
- `SistemaController --> Relatorio` (1:N)
- `SistemaController --> Aluno` (1:N)
- `TelaAvaliacao --> SistemaController` (1:1)
- `TelaRelatorio --> SistemaController` (1:1)
- `TelaPrincipal --> SistemaController` (1:1)

#### **⚪ DEPENDÊNCIA (2 relacionamentos)**
- `CalculadoraIMC ..> MedidasCorporais` (1:N)
- `ValidadorDados ..> MedidasCorporais` (1:N)

### **📈 Estatísticas do Sistema**
- **Total de Relacionamentos**: 15
- **Classes Envolvidas**: 8
- **Composições**: 1 (6.7%)
- **Agregações**: 4 (26.7%)
- **Associações**: 8 (53.3%)
- **Dependências**: 2 (13.3%)

