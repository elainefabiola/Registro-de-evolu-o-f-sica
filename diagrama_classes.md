# Diagrama de Classes - Sistema de Registro de evolucao fisica (Arquitetura MVC)

## Diagrama de Classes - Arquitetura MVC


```mermaid
classDiagram
    %% ===== DADOS (MODEL) =====
    %% Aqui ficam as informações que o sistema guarda
    
    class AvaliacaoFisica {
        +int id
        +String nomeAluno
        +Date dataAvaliacao
        +String observacoes
        +boolean completa
    }

    class MedidasCorporais {
        +int id
        +float peso
        +float altura
        +float cintura
        +float quadril
        +float braco
        +float coxa
    }

    class Relatorio {
        +int id
        +String nomeAluno
        +Date dataGeracao
        +String arquivoPDF
    }

    %% ===== LÓGICA DE NEGÓCIO (SERVICES) =====
    %% Aqui ficam os cálculos e validações
    
    class CalculadoraIMC {
        +calcularIMC(peso, altura)
        +classificarIMC(imc)
    }

    class ValidadorDados {
        +validarPeso(peso)
        +validarAltura(altura)
        +validarMedidas(medidas)
    }

    %% ===== CONTROLE (CONTROLLER) =====
    %% Aqui ficam as operações principais
    
    class SistemaController {
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
    }

    %% ===== INTERFACE (VIEW) =====
    %% Aqui ficam as telas que o usuário vê
    
    class TelaAvaliacao {
        +exibirFormulario()
        +exibirDados()
        +exibirMensagem()
    }

    class TelaRelatorio {
        +exibirRelatorio()
        +exibirGraficos()
    }

    class TelaPrincipal {
        +exibirMenu()
        +exibirDashboard()
        +exibirLogin()
        +exibirNavegacao()
    }

    %% ===== RELACIONAMENTOS MVC =====
    
    %% Relacionamentos de COMPOSIÇÃO (Composição - forte dependência)
    %% Uma AvaliacaoFisica É COMPOSTA POR MedidasCorporais
    AvaliacaoFisica *-- MedidasCorporais : "composição<br/>(tem-um)"
    
    %% Relacionamentos de AGREGAÇÃO (Agregação - dependência média)
    %% Um SistemaController É AGREGADO POR Services
    SistemaController o-- CalculadoraIMC : "agregação<br/>(contém)"
    SistemaController o-- ValidadorDados : "agregação<br/>(contém)"
    
    %% Relacionamentos de ASSOCIAÇÃO (Associação - relacionamento fraco)
    %% Uma AvaliacaoFisica está ASSOCIADA a Relatorios
    AvaliacaoFisica --> Relatorio : "associação<br/>(gera)"
    
    %% Relacionamentos Controller -> Model (Associações)
    SistemaController --> AvaliacaoFisica : "associação<br/>(gerencia)"
    SistemaController --> MedidasCorporais : "associação<br/>(manipula)"
    SistemaController --> Relatorio : "associação<br/>(gerencia)"
    
    %% Relacionamentos View -> Controller (Associações)
    TelaAvaliacao --> SistemaController : "associação<br/>(comunica)"
    TelaRelatorio --> SistemaController : "associação<br/>(comunica)"
    TelaPrincipal --> SistemaController : "associação<br/>(comunica)"
    
    %% Relacionamentos Service -> Model (Dependências)
    CalculadoraIMC ..> MedidasCorporais : "dependência<br/>(calcula)"
    ValidadorDados ..> MedidasCorporais : "dependência<br/>(valida)"

    %% Notas explicativas
    note for AvaliacaoFisica "Armazena informações<br/>básicas da avaliação"
    
    note for MedidasCorporais "Guarda todas as<br/>medidas do corpo"
    
    note for CalculadoraIMC "Faz os cálculos<br/>matemáticos"
    
    note for SistemaController "Controla todo o<br/>sistema MVC"
```

## Tipos de Relacionamentos no Diagrama

### 🔗 **COMPOSIÇÃO (Composição - *--)**
- **Símbolo**: `*--` (losango preenchido)
- **Significado**: Relacionamento forte - "É COMPOSTO POR" ou "TEM-UM"
- **Exemplo**: `AvaliacaoFisica *-- MedidasCorporais`
- **Características**:
  - Se a AvaliacaoFisica for excluída, as MedidasCorporais também são excluídas
  - As MedidasCorporais não podem existir sem a AvaliacaoFisica
  - Relacionamento de dependência forte

### 🔄 **AGREGAÇÃO (Agregação - o--)**
- **Símbolo**: `o--` (losango vazio)
- **Significado**: Relacionamento médio - "CONTÉM" ou "TEM-UM"
- **Exemplo**: `SistemaController o-- CalculadoraIMC`
- **Características**:
  - O SistemaController CONTÉM a CalculadoraIMC
  - Se o SistemaController for excluído, a CalculadoraIMC pode continuar existindo
  - Relacionamento de dependência média

### ↔️ **ASSOCIAÇÃO (Associação - -->)**
- **Símbolo**: `-->` (seta simples)
- **Significado**: Relacionamento fraco - "USA" ou "GERENCIA"
- **Exemplo**: `SistemaController --> AvaliacaoFisica`
- **Características**:
  - As classes podem existir independentemente
  - Relacionamento temporário ou de uso
  - Uma classe usa a outra, mas não depende dela para existir

### 📋 **DEPENDÊNCIA (Dependência - ..>)**
- **Símbolo**: `..>` (seta tracejada)
- **Significado**: Relacionamento muito fraco - "DEPENDE DE"
- **Exemplo**: `CalculadoraIMC ..> MedidasCorporais`
- **Características**:
  - Uma classe precisa da outra para funcionar
  - Relacionamento temporário durante execução
  - Não há propriedade ou posse

## Exemplos Práticos dos Relacionamentos

### 🏠 **Analogia com uma Casa**

#### **COMPOSIÇÃO** (Casa *-- Quarto)
- Uma casa É COMPOSTA POR quartos
- Se a casa for demolida, os quartos também são destruídos
- Os quartos não podem existir sem a casa

#### **AGREGAÇÃO** (Casa o-- Móveis)
- Uma casa CONTÉM móveis (mesa, cadeira, sofá)
- Se a casa for demolida, os móveis podem ser movidos para outra casa
- Os móveis podem existir independentemente da casa

#### **ASSOCIAÇÃO** (Pessoa --> Casa)
- Uma pessoa USA uma casa (morar, alugar)
- A pessoa e a casa podem existir independentemente
- A pessoa pode mudar de casa, a casa pode ter outros moradores

#### **DEPENDÊNCIA** (Eletricista ..> Casa)
- Um eletricista DEPENDE da casa para trabalhar
- É um relacionamento temporário
- O eletricista não possui a casa, apenas trabalha nela

### 💻 **No Nosso Sistema**

#### **COMPOSIÇÃO**: `AvaliacaoFisica *-- MedidasCorporais`
- Uma avaliação física É COMPOSTA POR medidas corporais
- Se excluir a avaliação, as medidas também são excluídas
- As medidas não existem sem a avaliação

#### **AGREGAÇÃO**: `SistemaController o-- CalculadoraIMC`
- O SistemaController CONTÉM a CalculadoraIMC
- Se excluir o SistemaController, a CalculadoraIMC pode ser reutilizada
- A CalculadoraIMC pode existir independentemente

#### **ASSOCIAÇÃO**: `SistemaController --> AvaliacaoFisica`
- O controller GERENCIA as avaliações
- Controller e avaliação podem existir independentemente
- O controller pode gerenciar outras coisas também

#### **DEPENDÊNCIA**: `CalculadoraIMC ..> MedidasCorporais`
- A calculadora DEPENDE das medidas para calcular o IMC
- É um relacionamento temporário durante o cálculo
- A calculadora não possui as medidas, apenas as usa

## 🔍 Diferença entre Composição e Agregação

### **COMPOSIÇÃO vs AGREGAÇÃO**

| Aspecto | Composição (*--) | Agregação (o--) |
|---------|------------------|-----------------|
| **Força** | Relacionamento forte | Relacionamento médio |
| **Propriedade** | "É COMPOSTO POR" | "CONTÉM" |
| **Vida útil** | Mesma vida útil | Vida útil independente |
| **Exclusão** | Se excluir o todo, exclui as partes | Se excluir o todo, as partes podem sobreviver |
| **Símbolo** | Losango preenchido | Losango vazio |

### **🎯 Quando Usar Cada Uma?**

#### **Use COMPOSIÇÃO quando:**
- As partes não podem existir sem o todo
- Se excluir o todo, as partes também devem ser excluídas
- Exemplo: Casa *-- Quarto (quarto não existe sem casa)

#### **Use AGREGAÇÃO quando:**
- As partes podem existir independentemente
- Se excluir o todo, as partes podem ser reutilizadas
- Exemplo: Casa o-- Móveis (móveis podem ser movidos)

### **💡 Dica para Iniciantes:**
- **Composição**: "Não posso viver sem você"
- **Agregação**: "Posso viver sem você, mas prefiro ter você"

## Arquitetura MVC - Descrição das Camadas

### **CAMADA MODEL (MODELOS + SERVIÇOS + UTILITÁRIOS)**

### 🗂️ **DADOS (Model)**
São como "gavetas" onde guardamos as informações:

- **AvaliacaoFisica**: Guarda informações básicas (nome do aluno, data, observações)
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
4. **Usuário** preenche dados na **TelaAvaliacao**
5. **TelaAvaliacao** envia dados para o **SistemaController**
6. **SistemaController** pede para o **ValidadorDados** verificar se está tudo certo
7. **SistemaController** pede para o **CalculadoraIMC** calcular o IMC
8. **SistemaController** salva os dados na **AvaliacaoFisica** e **MedidasCorporais**
9. **SistemaController** avisa a **TelaAvaliacao** que deu tudo certo
10. **TelaRelatorio** solicita relatório ao **SistemaController**
11. **SistemaController** gera relatório baseado nos dados salvos

