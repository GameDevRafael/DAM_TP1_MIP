# Assignment 4 — MIP: VaultGuard

**Course:** Desenvolvimento de Aplicações Móveis (DAM)  
**Student:** A51394 Rafael Faustino  
**Date:** 10/03/2026  
**Repository URL:** https://github.com/GameDevRafael/DAM_TP1_MIP

---

## 1. Introdução

A aplicação VaultGuard surge no contexto da secção 8 — MIP (Mission Impossible Possible) do Tutorial 1 da unidade curricular de Desenvolvimento de Aplicações Móveis. Esta secção foca-se na colaboração estratégica entre o desenvolvedor e ferramentas de Inteligência Artificial para a criação de uma solução robusta e funcional.

O projeto consiste num gestor de passwords offline, motivado pela crescente necessidade de segurança digital e privacidade de dados. Os objetivos principais incluem:

- Implementação de criptografia de nível militar (AES-GCM)
- Verificação de integridade de credenciais através de APIs externas
- Exploração de fluxos de autenticação segura no ecossistema Android

A escolha desta aplicação justifica-se pela complexidade técnica exigida pela gestão de dados sensíveis, que permite testar os limites das capacidades de geração de código e assistência arquitetural da IA.

## 2. Visão Geral do Sistema

O VaultGuard é uma solução de gestão de credenciais que prioriza a segurança local. O sistema implementa um cofre digital protegido por um PIN mestre, cifrado através de SHA-256 e armazenado em *EncryptedSharedPreferences*.

### Funcionalidades Principais

| Funcionalidade | Descrição |
|---|---|
| Armazenamento cifrado | Guarda título, utilizador, password e notas com cifragem AES-GCM |
| Gerador de passwords | Geração segura com parâmetros configuráveis |
| Auditoria de segurança | Deteção de passwords comprometidas via API *HaveIBeenPwned* |

A aplicação utiliza o modelo de K-Anonimato para consultas externas, garantindo que a password real nunca abandona o dispositivo. Os serviços externos limitam-se à integração com a API *HaveIBeenPwned* para auditoria de segurança das credenciais guardadas.

## 3. Arquitetura e Design

A solução adota a arquitetura MVVM (Model-View-ViewModel), recomendada pela Google para projetos Android modernos. Esta decisão facilita a separação de responsabilidades e a testabilidade do código.

### Estrutura do Projeto

```
app/src/main/
├── data/
│   ├── local/          # Room + EncryptedSharedPreferences
│   ├── remote/         # Retrofit + HaveIBeenPwned API
│   ├── repository/     # VaultRepository
│   └── crypto/         # CryptoManager (AndroidKeyStore + AES-GCM)
├── ui/
│   ├── login/          # Autenticação por PIN mestre
│   ├── vault/          # Listagem de entradas
│   ├── entry/          # Criação e edição de credenciais
│   └── generator/      # Gerador de passwords
└── di/                 # Dependency Injection com Hilt
```

### Padrões de Design Utilizados

| Padrão | Aplicação |
|---|---|
| Repository Pattern | Abstração da fonte de dados |
| Singleton | Gestores globais como `CryptoManager` |
| Observer Pattern | Kotlin Flows para atualização dinâmica da UI |

A arquitetura foi proposta pelo agente de IA, aprovada pelo aluno após revisão do plano de implementação, e validada para garantir a conformidade com as melhores práticas de desenvolvimento Android.

## 4. Implementação

### Tecnologias e Bibliotecas

| Biblioteca | Função |
|---|---|
| Room (SQLite) | Persistência local com campos cifrados em Base64/AES-GCM |
| Retrofit | Integração com a API HaveIBeenPwned |
| Dagger-Hilt | Dependency Injection |
| Jetpack Navigation | Navegação entre fragmentos |
| Coroutines | Operações assíncronas |
| Jetpack Security | EncryptedSharedPreferences |

### Detalhe de Implementação

A camada de persistência utiliza a biblioteca *Room* sobre uma base de dados SQLite, onde os campos sensíveis são guardados em formato Base64 após cifragem AES-GCM (256-bit). A lógica de negócio reside nos *ViewModels*, que interagem com o `VaultRepository`.

A integração com a API externa é feita através de *Retrofit*, processando apenas os primeiros 5 caracteres do hash SHA-1 das passwords para manter o anonimato. O `CryptoManager.kt` demonstra a correta utilização do *AndroidKeyStore* para a gestão de chaves criptográficas de hardware.

## 5. Testes e Validação

A estratégia de testes centrou-se em validações manuais. Foram executados os seguintes cenários:

| Cenário | Resultado |
|---|---|
| Persistência de dados após encerramento | ✓ Passou |
| Decifragem correta de campos sensíveis | ✓ Passou |
| Password sem fugas detetadas | ✓ Passou |
| Password comprometida ("123456") | ✓ "Pwned 1439 times! Change it." |

O sistema demonstrou estabilidade em emuladores com API level 36 e dispositivo físico com Android 13.

## 6. Instruções de Utilização

### Pré-requisitos

- Android Studio (Jellyfish ou superior)
- Android SDK para API 34
- Dispositivo ou emulador com Android 8.0+

### Clonar e Configurar

```bash
git clone https://github.com/GameDevRafael/DAM_TP1_MIP.git
cd VaultGuard
```

1. Abrir o Android Studio e selecionar **File → Open**, apontando para a pasta `VaultGuard`
2. Aguardar a sincronização do Gradle (`Build → Sync Project with Gradle Files`)
3. Não são necessárias chaves de API — a *HaveIBeenPwned* é acedida através de um endpoint público
4. Executar a aplicação com **Run → Run 'app'** (ou `Shift+F10`) num dispositivo com Android 8.0+

### Resolução de Problemas Comuns

```bash
./gradlew clean build
```

Em caso de problemas com o *KeyStore*, reiniciar o emulador garante a correta inicialização.

---

# Autonomous Software Engineering Sections

## 7. Prompting Strategy

O desenvolvimento utilizou o Google Antigravity (com modelo Gemini 1.5 Pro) e o Claude (Anthropic) via claude.ai. A estratégia de prompting foi definida com base no template fornecido pelo enunciado, estruturado em seis componentes:

| Componente | Descrição |
|---|---|
| Context | Contexto do projeto e requisitos de segurança |
| Goal | Objetivos funcionais da aplicação |
| Constraints | Restrições arquiteturais explícitas (MVVM, Room, Retrofit, Hilt, Material Design 3, AES-GCM, HaveIBeenPwned) |
| Plan | Plano de implementação para aprovação prévia |
| Verification | Critérios de validação do código gerado |
| Deliverables | Ficheiros e artefactos esperados |

O prompt inicial foi elaborado com apoio do Claude, que ajudou a formular os requisitos técnicos de forma precisa antes de os submeter ao Antigravity. O agente foi configurado em modo Planning, o que o forçou a gerar um plano de implementação detalhado para aprovação antes de escrever qualquer código. Erros de compilação subsequentes foram reportados ao Antigravity em prompts de correção, colando o output de erro completo e solicitando a fix.

## 8. Autonomous Agent Workflow

O Antigravity contribuiu em todas as fases do ciclo de desenvolvimento:

**Planeamento:**  
Gerou um documento `implementation_plan.md` com arquitetura, estrutura de pastas, dependências e estratégia de encriptação, revisto e aprovado pelo aluno antes de qualquer geração de código.

**Implementação:**  
Gerou os 37 ficheiros do projeto, incluindo toda a lógica de cifragem, os ViewModels, os Fragments, os layouts XML e a configuração do Hilt.

**Debugging:**  
Identificou e corrigiu autonomamente três problemas:

| Problema | Solução |
|---|---|
| Incompatibilidade do Android Gradle Plugin com Data Binding | Atualização do AGP para 8.3.0 |
| Dependências Hilt em falta | Adição das dependências em falta no Gradle |
| Erros de sintaxe Kotlin no `VaultAdapter` e `VaultFragment` | Correção da sintaxe gerada |

**Intervenção humana:**
- Definir a ideia e os requisitos da app
- Elaborar o prompt inicial com apoio do Claude
- Aprovar o plano de implementação
- Aceitar os ficheiros gerados
- Reportar os erros de compilação ao agente
- Solicitar correções visuais na interface (contraste dos labels dos switches e cor dos títulos das páginas)

## 9. Verification of AI-Generated Artifacts

O código gerado foi verificado através de testes manuais na app em execução e através de sessões de análise com o Claude. A verificação funcional incluiu:

- Configuração do PIN mestre na primeira execução
- Autenticação com PIN correto e rejeição de PIN errado
- Criação e listagem de entradas no vault
- Verificação de breach com password conhecidamente comprometida
- Geração de passwords com diferentes configurações de caracteres

A análise do código foi feita ficheiro a ficheiro com apoio do Claude para garantir compreensão da lógica de encriptação AES-GCM, do fluxo de k-anonimato no HaveIBeenPwned, e da arquitetura MVVM implementada.

## 10. Human vs AI Contribution

| Área | Responsável |
|---|---|
| Geração de código, encriptação, modelos de dados, ViewModels, Fragments, layouts XML | IA (Antigravity) |
| Definição da ideia e requisitos | Humano |
| Estruturação do prompt inicial | Humano (com apoio do Claude) |
| Aprovação do plano de arquitetura | Humano |
| Reporte de erros de compilação | Humano |
| Melhorias visuais da interface | Humano |
| Estudo e compreensão do código gerado | Humano (com apoio do Claude) |

## 11. Ethical and Responsible Use

O uso de ferramentas de IA levanta questões importantes sobre responsabilidade e compreensão do código produzido. Os principais riscos identificados foram:

| Risco | Mitigação |
|---|---|
| Código gerado que o aluno não consegue explicar | Sessões de estudo do código com apoio do Claude |
| Incompatibilidades de versões de bibliotecas | Materializou-se e foi resolvido pelo próprio agente |

O aluno assume a responsabilidade total pelo código produzido, reconhecendo que a IA serviu como agente de desenvolvimento, mas que a validação funcional, a aprovação arquitetural e a compreensão do produto final são da responsabilidade humana.

---

# Development Process

## 12. Version Control and Commit History

O repositório utiliza uma única branch `main`, adequada para um projeto de dimensão individual com âmbito académico. Os commits foram realizados de forma incremental, refletindo a progressão do trabalho: primeiro a submissão do código do projeto MIP, seguida de iterações sucessivas sobre o relatório README.md à medida que o documento foi sendo refinado e completado.

Cada commit de documentação correspondeu a uma melhoria concreta — atualização de informação do estudante, correção do URL do repositório, e revisão de linguagem. Esta abordagem, embora simples, garante rastreabilidade das alterações e separação entre a entrega do código e a evolução da documentação.

## 13. Difficulties and Lessons Learned

O principal desafio técnico foi a resolução de erros de compilação causados por incompatibilidades entre o Android Gradle Plugin e o sistema de Data Binding, que exigiu a atualização da versão do AGP para 8.3.0. Outra dificuldade residiu na configuração inicial do Android Studio, que não reconhecia o projeto Gradle por ter sido aberta a pasta errada.

A aprendizagem mais relevante foi perceber que o valor do agente de IA não está apenas na geração de código, mas na capacidade de iterar rapidamente sobre erros — o que exige que o utilizador saiba identificar e comunicar os problemas corretamente.

### Conceitos Técnicos Esclarecidos

**AndroidKeyStore e ALIAS:**  
A dúvida inicial era se cada entrada guardada no cofre tinha o seu próprio ALIAS. O Claude esclareceu que todas as entradas partilham a mesma chave criptográfica e que o ALIAS é apenas o nome pelo qual o KeyStore identifica essa chave. O que torna cada cifragem única não é a chave, mas o IV aleatório gerado em cada operação — dois campos cifrados com a mesma chave produzem resultados completamente diferentes precisamente por causa disso.

**Base64:**  
Não era claro porque é que os dados cifrados eram convertidos para Base64 antes de serem guardados. O Claude explicou que a cifragem produz bytes arbitrários que não são texto válido — alguns podem corromper uma string em SQLite. O Base64 converte qualquer sequência de bytes em caracteres seguros (A-Z, a-z, 0-9, +, /), tornando o resultado guardável em qualquer campo de texto, ao custo de ocupar cerca de 33% mais espaço.

**`id` vs `title`:**  
Havia a ideia errada de que o `id` no método `getEntryById` correspondia ao título da entrada. O `id` é um número inteiro gerado automaticamente pelo Room para cada linha da tabela (1, 2, 3...) e não tem qualquer relação com o título. O título é o campo `title` — texto separado. O `id` serve apenas para identificar unicamente cada linha na base de dados.

### Aprendizagens sobre Trabalhar com IA

Usar dois modelos com papéis distintos — Antigravity para gerar, Claude para explicar — revelou-se uma abordagem eficaz. O Antigravity gera rapidamente mas não explica; o Claude foi essencial para compreender o código gerado e formular prompts mais precisos. A combinação das duas ferramentas foi mais produtiva do que usar apenas uma.

## 14. Future Improvements

- **Autenticação biométrica:** Implementar via *BiometricPrompt* para substituição ou complemento do PIN
- **Performance da RecyclerView:** Otimização com *DiffUtil* para atualizações incrementais da lista
- **Testes unitários:** Implementação via *JUnit* para cobertura da lógica de negócio
- **UI/UX:** Modo escuro dinâmico e animações nas transições entre fragmentos
- **Cópias de segurança:** Exportação cifrada como objetivo a médio prazo

---

## 15. AI Usage Disclosure (Mandatory)

**Código: [AC YES, AI YES]**  
Este projeto foi desenvolvido com assistência obrigatória de ferramentas de inteligência artificial, conforme requerido pela secção 8 (MIP) do Tutorial 1. Ferramentas utilizadas:

| Ferramenta | Utilização |
|---|---|
| **Google Antigravity** (Gemini 1.5 Pro) | Geração do código, arquitetura e correção de erros de compilação |
| **Claude (Anthropic)** via claude.ai | Apoio na formulação do prompt inicial, explicação dos ficheiros gerados e estudo do código |

O aluno compreende o código gerado, é capaz de o explicar, e assume total responsabilidade pelo produto final. O código foi verificado e testado manualmente.

**Relatório: [AC YES, AI YES]**  
A redação e estruturação deste relatório foi assistida pelo modelo **Claude (Anthropic)**. O aluno é totalmente responsável pelo conteúdo apresentado e confirma que o mesmo reflete com rigor o trabalho desenvolvido.
