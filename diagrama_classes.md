# Diagrama de Classes - Sistema de Registro de Circunferências Corporais

## Diagrama de Classes

```mermaid
classDiagram
    %% Entidades Principais
    class Usuario {
       
    }

    class Aluno {
      
    }

    class Profissional {
        
    }

    class AvaliacaoFisica {
        +int id
        +int alunoId
        +int profissionalId
        +Date dataAvaliacao
        +String observacoes
        +boolean completa
        +Date dataCriacao
        +Date dataAtualizacao
        +salvar()
        +calcularIMC()
        +classificarPercentualGordura()
    }

    %% Medidas Corporais
    class Circunferencias {
        +int id
        +int avaliacaoId
        +float torax
        +float bracoDireitoContraido
        +float bracoEsquerdoContraido
        +float bracoDireitoRelaxado
        +float bracoEsquerdoRelaxado
        +float abdomen
        +float cintura
        +float quadril
        +float antebracoDireito
        +float antebracoEsquerdo
        +float coxaDireita
        +float coxaEsquerda
        +float panturrilhaDireita
        +float panturrilhaEsquerda
        +float escapular
        +validarMedidas()
        +calcularVariacao()
    }

    class ComposicaoCorporal {
        +int id
        +int avaliacaoId
        +float peso
        +float altura
        +float percentualGordura
        +float pesoOsseo
        +float pesoResidual
        +float pesoMuscular
        +float pesoGordura
        +float imc
        +String classificacaoIMC
        +String classificacaoGordura
        +calcularPesoGordura()
        +calcularIMC()
        +classificarIMC()
        +classificarPercentualGordura()
        +validarDados()
    }

    %% Relatórios e Histórico
    class RelatorioEvolucao {
        +int id
        +int alunoId
        +int profissionalId
        +Date dataGeracao
        +String formato
        +String caminhoArquivo
        +gerarPDF()
        +compararAvaliacoes()
        +calcularEstatisticas()
    }

    class ComparacaoMedidas {
        +int id
        +int avaliacaoAtualId
        +int avaliacaoAnteriorId
        +Map~String, Float~ variacoesAbsolutas
        +Map~String, Float~ variacoesPercentuais
        +calcularVariacoes()
        +gerarTabelaComparativa()
    }

    %% Validações e Utilitários
    class ValidadorMedidas {
        +validarCircunferencias()
        +validarComposicaoCorporal()
        +validarPeso()
        +validarAltura()
        +validarPercentualGordura()
        +validarFormatoNumerico()
    }

    class CalculadoraIMC {
        +calcularIMC(peso, altura)
        +classificarIMC(imc)
        +getFaixasIMC()
    }

    class ClassificadorGordura {
        +classificarPorGeneroIdade(percentual, genero, idade)
        +getFaixasGordura()
        +validarFaixaPercentual()
    }

    %% Relacionamentos
    Usuario <|-- Aluno
    Usuario <|-- Profissional
    
    Aluno "1" --> "*" AvaliacaoFisica : possui
    Profissional "1" --> "*" AvaliacaoFisica : realiza
    
    AvaliacaoFisica "1" --> "1" Circunferencias : contém
    AvaliacaoFisica "1" --> "1" ComposicaoCorporal : contém
    
    AvaliacaoFisica "1" --> "*" RelatorioEvolucao : gera
    Profissional "1" --> "*" RelatorioEvolucao : cria
    
    ComparacaoMedidas --> AvaliacaoFisica : compara atual
    ComparacaoMedidas --> AvaliacaoFisica : compara anterior
    
    ValidadorMedidas ..> Circunferencias : valida
    ValidadorMedidas ..> ComposicaoCorporal : valida
    
    CalculadoraIMC ..> ComposicaoCorporal : calcula
    ClassificadorGordura ..> ComposicaoCorporal : classifica

    %% Notas e Observações
    note for AvaliacaoFisica "Campos opcionais permitidos<br/>Observações até 1000 caracteres<br/>Status: completa/parcial"
    
    note for Circunferencias "Valores positivos<br/>Até 2 casas decimais<br/>Campos opcionais"
    
    note for ComposicaoCorporal "Peso: 20-180kg<br/>Altura: 1.00-2.50m<br/>Gordura: 3-70%<br/>Cálculos automáticos"
    
    note for RelatorioEvolucao "Mínimo 2 avaliações<br/>Exportação PDF<br/>Layout profissional"
```

## Descrição das Classes Principais

### **Usuario (Classe Base)**
- Classe abstrata que representa qualquer usuário do sistema
- Contém dados básicos de autenticação e perfil
- Implementa métodos de autenticação e validação de permissões

### **Aluno**
- Herda de Usuario
- Representa os alunos da academia
- Possui histórico de avaliações físicas
- Pode solicitar novas avaliações

### **Profissional**
- Herda de Usuario
- Representa instrutores e profissionais da academia
- Pode registrar avaliações físicas
- Pode gerar relatórios de evolução

### **AvaliacaoFisica**
- Entidade central do sistema
- Contém todas as medidas de uma avaliação
- Suporta campos opcionais
- Calcula automaticamente IMC e classificações

### **Circunferencias**
- Armazena todas as medidas de circunferência corporal
- Valida formato numérico e faixas de valores
- Calcula variações entre avaliações

### **ComposicaoCorporal**
- Armazena dados de peso, altura e composição corporal
- Calcula automaticamente IMC e peso de gordura
- Classifica percentual de gordura por faixas etárias

### **RelatorioEvolucao**
- Gera relatórios em PDF
- Compara avaliações temporais
- Requer mínimo de 2 avaliações

### **ValidadorMedidas**
- Classe utilitária para validações
- Valida formatos numéricos e faixas de valores
- Garante integridade dos dados

### **CalculadoraIMC**
- Calcula IMC usando fórmula padrão
- Classifica IMC em faixas da OMS
- Retorna classificações padronizadas

### **ClassificadorGordura**
- Classifica percentual de gordura por gênero e idade
- Implementa tabelas de referência
- Valida faixas aceitáveis

## Funcionalidades Implementadas

✅ **US10**: Registro de circunferências corporais (AC1-AC3)
✅ **US13**: Registro de peso, altura e percentual de gordura (AC4-AC9)
✅ **US17**: Adição de observações (AC10-AC15)
✅ **US15**: Visualização do histórico (AC16-AC19)
✅ **US12**: Registro múltiplas medidas (AC20-AC25)
✅ **US14**: Visualização do IMC (AC26-AC30)
✅ **US16**: Geração de relatórios (AC31-AC33)
✅ **US11**: Comparação de medidas (AC34-AC36)

## Padrões de Design Utilizados

- **Herança**: Usuario como classe base para Aluno e Profissional
- **Composição**: AvaliacaoFisica composta por Circunferencias e ComposicaoCorporal
- **Validação**: Classes utilitárias para validação e cálculo
- **Separação de Responsabilidades**: Cada classe tem uma responsabilidade específica
