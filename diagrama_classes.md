# Diagrama de Classes - Módulo Registro de evolucao fisica (Arquitetura MVC)

## Diagrama de Classes - Arquitetura MVC


```mermaid
classDiagram
    %% ===== INTERFACE CRUD =====
    %% Interface genérica para operações CRUD
    
    class ICrud {
        <<interface>>
        +create(objeto)
        +read(id)
        +update(objeto)
        +delete(id)
        +list()
        +exists(id)
        +validate(objeto)
    }

    %% ===== REPOSITÓRIOS (Implementam ICrud) =====
    %% Classes que implementam operações CRUD
    
    class AvaliacaoRepository {
        -List~AvaliacaoFisica~ avaliacoes
        +create(avaliacao)
        +read(id)
        +update(avaliacao)
        +delete(id)
        +list()
        +exists(id)
        +validate(avaliacao)
        +findByAluno(nomeAluno)
        +findByData(data)
    }

    class MedidasRepository {
        -List~MedidasCorporais~ medidas
        +create(medidas)
        +read(id)
        +update(medidas)
        +delete(id)
        +list()
        +exists(id)
        +validate(medidas)
        +findByAvaliacao(avaliacaoId)
    }

    class RelatorioRepository {
        -List~Relatorio~ relatorios
        +create(relatorio)
        +read(id)
        +update(relatorio)
        +delete(id)
        +list()
        +exists(id)
        +validate(relatorio)
        +findByAluno(nomeAluno)
        +findByData(data)
    }
    
    class AvaliacaoFisica {
        -int id
        -Aluno nomeAluno
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
        -AvaliacaoRepository avaliacaoRepo
        -MedidasRepository medidasRepo
        -RelatorioRepository relatorioRepo
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
        +getAvaliacaoRepo()
        +getMedidasRepo()
        +getRelatorioRepo()
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

    %% ===== RELACIONAMENTOS MVC =====
    
    %% Relacionamentos de IMPLEMENTAÇÃO (Interface)
    AvaliacaoRepository ..|> ICrud : "implementa"
    MedidasRepository ..|> ICrud : "implementa"
    RelatorioRepository ..|> ICrud : "implementa"
    
    %% Relacionamentos de COMPOSIÇÃO (Composição - forte dependência)
    %% Uma AvaliacaoFisica É COMPOSTA POR MedidasCorporais
    AvaliacaoFisica *-- MedidasCorporais : "composição<br/>(tem-um)"
    
    %% Relacionamentos de AGREGAÇÃO (Agregação - dependência média)
    %% Um SistemaController É AGREGADO POR Services e Repositories
    SistemaController o-- CalculadoraIMC : "agregação<br/>(contém)"
    SistemaController o-- ValidadorDados : "agregação<br/>(contém)"
    SistemaController o-- AvaliacaoRepository : "agregação<br/>(contém)"
    SistemaController o-- MedidasRepository : "agregação<br/>(contém)"
    SistemaController o-- RelatorioRepository : "agregação<br/>(contém)"
    
    %% Relacionamentos de ASSOCIAÇÃO (Associação - relacionamento fraco)
    %% Uma AvaliacaoFisica está ASSOCIADA a Relatorios
    AvaliacaoFisica --> Relatorio : "associação<br/>(gera)"
    
    %% Relacionamentos Repository -> Model (Associações)
    AvaliacaoRepository --> AvaliacaoFisica : "associação<br/>(gerencia)"
    MedidasRepository --> MedidasCorporais : "associação<br/>(gerencia)"
    RelatorioRepository --> Relatorio : "associação<br/>(gerencia)"
    
    %% Relacionamentos Controller -> Repository (Associações)
    SistemaController --> AvaliacaoRepository : "associação<br/>(usa)"
    SistemaController --> MedidasRepository : "associação<br/>(usa)"
    SistemaController --> RelatorioRepository : "associação<br/>(usa)"
    
    %% Relacionamentos View -> Controller (Associações)
    TelaAvaliacao --> SistemaController : "associação<br/>(comunica)"
    TelaRelatorio --> SistemaController : "associação<br/>(comunica)"
    TelaPrincipal --> SistemaController : "associação<br/>(comunica)"
    
    %% Relacionamentos Service -> Model (Dependências)
    CalculadoraIMC ..> MedidasCorporais : "dependência<br/>(calcula)"
    ValidadorDados ..> MedidasCorporais : "dependência<br/>(valida)"

    %% Notas explicativas
    note for ICrud "Interface padrão<br/>para operações CRUD"
    
    note for AvaliacaoRepository "Gerencia dados<br/>de avaliações"
    
    note for MedidasRepository "Gerencia dados<br/>de medidas corporais"
    
    note for RelatorioRepository "Gerencia dados<br/>de relatórios"
    
    note for AvaliacaoFisica "Armazena informações<br/>básicas da avaliação"
    
    note for MedidasCorporais "Guarda todas as<br/>medidas do corpo"
    
    note for CalculadoraIMC "Faz os cálculos<br/>matemáticos"
    
    note for SistemaController "Controla todo o<br/>sistema MVC"
```

## Arquitetura MVC - Descrição das Camadas

### **CAMADA MODEL (MODELOS + SERVIÇOS + UTILITÁRIOS + REPOSITÓRIOS)**

### 🔧 **INTERFACE CRUD**
Define o padrão para operações de banco de dados:

- **ICrud**: Interface genérica com métodos padrão (create, read, update, delete, list, exists, validate)

### 🗂️ **REPOSITÓRIOS (Implementam ICrud)**
São como "bibliotecários" que organizam e gerenciam os dados:

- **AvaliacaoRepository**: Gerencia dados de avaliações físicas
- **MedidasRepository**: Gerencia dados de medidas corporais  
- **RelatorioRepository**: Gerencia dados de relatórios

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

- **SistemaController**: Controla todo o sistema - usa repositórios para gerenciar dados, coordena operações entre Model e View

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
8. **SistemaController** usa **AvaliacaoRepository** para salvar dados na **AvaliacaoFisica**
9. **SistemaController** usa **MedidasRepository** para salvar dados na **MedidasCorporais**
10. **SistemaController** avisa a **TelaAvaliacao** que deu tudo certo
11. **TelaRelatorio** solicita relatório ao **SistemaController**
12. **SistemaController** usa **RelatorioRepository** para gerar relatório baseado nos dados salvos

## 🔧 Interface CRUD - Padrão de Design

### **📋 O que é a Interface ICrud?**
A interface `ICrud` define um contrato padrão para operações de banco de dados:

```java
public interface ICrud<T> {
    T create(T objeto);           // Criar novo registro
    T read(int id);              // Ler registro por ID
    T update(T objeto);          // Atualizar registro existente
    boolean delete(int id);      // Excluir registro por ID
    List<T> list();             // Listar todos os registros
    boolean exists(int id);     // Verificar se registro existe
    boolean validate(T objeto);  // Validar dados antes de salvar
}
```

### **🎯 Benefícios da Interface CRUD:**

#### **✅ Padronização**
- Todos os repositórios seguem o mesmo padrão
- Métodos com nomes consistentes
- Comportamento previsível

#### **✅ Reutilização**
- Interface genérica (`<T>`) funciona com qualquer tipo
- Código comum pode ser compartilhado
- Fácil manutenção

#### **✅ Flexibilidade**
- Fácil trocar implementação (ex: de arquivo para banco)
- Testes unitários simplificados
- Injeção de dependência facilitada

### **🏗️ Implementação nos Repositórios:**

#### **AvaliacaoRepository**
```java
public class AvaliacaoRepository implements ICrud<AvaliacaoFisica> {
    // Implementa todos os métodos da interface ICrud
    // + métodos específicos: findByAluno(), findByData()
}
```

#### **MedidasRepository**
```java
public class MedidasRepository implements ICrud<MedidasCorporais> {
    // Implementa todos os métodos da interface ICrud
    // + métodos específicos: findByAvaliacao()
}
```

#### **RelatorioRepository**
```java
public class RelatorioRepository implements ICrud<Relatorio> {
    // Implementa todos os métodos da interface ICrud
    // + métodos específicos: findByAluno(), findByData()
}
```

### **💡 Vantagens desta Arquitetura:**
- ✅ **Consistência**: Todos os repositórios seguem o mesmo padrão
- ✅ **Manutenibilidade**: Mudanças na interface afetam todos os repositórios
- ✅ **Testabilidade**: Fácil criar mocks para testes
- ✅ **Escalabilidade**: Fácil adicionar novos repositórios

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
