# Assignment 8 — VaultGuard

Course: Desenvolvimento de Aplicações Móveis
Student(s): A51394 Rafael Faustino
Date: 08/03/2026
Repository URL: https://github.com/GameDevRafael/DAM_TP1_MIP

## 1. Introduction
O objetivo deste trabalho é criar o "VaultGuard". É uma aplicação para Android que serve como um gestor de palavras-passe local. O problema que tentei resolver é a dificuldade que as pessoas têm para guardar e lembrar dezenas de palavras-passe seguras. A aplicação ajuda a guardar todas as passwords de forma muito segura no telemóvel e também ajuda a criar palavras-passe novas e fortes.

## 2. System Overview
A aplicação permite ao utilizador criar um "cofre" que é fechado e protegido por um PIN principal. Lá dentro, pode ver as suas contas, adicionar contas novas, editar ou apagar as que já não usa. Também tem um gerador de passwords. Uma parte porreira do projeto é que a app consegue ir à internet ver se uma palavra-passe sofreu uma fuga de dados, usando a API do "Have I Been Pwned".

## 3. Architecture and Design
Usei a arquitetura MVVM (Model-View-ViewModel). Escolhi esta arquitetura porque separa bem o desenho do ecrã e as regras do programa.
- **Fragments e XML**: Tratam só da imagem e de apanhar os cliques do utilizador.
- **ViewModels**: Guardam os dados enquanto o ecrã muda de forma e enviam as ordens para a base de dados.
- **Repositories**: Fazem a ponte entre a aplicação e a base de dados ou internet.
- A app está toda dividida em pastas principais (`data`, `di`, `ui`, `utils`) para ficar mais arrumada e ser fácil achar tudo o que preciso.

## 4. Implementation
Os bocados mais importantes do projeto são:
- **Base de Dados**: Usei a biblioteca Room. Criei a tabela `PasswordEntry`. Mas, antes de gravar, todas as senhas, notas e nomes de utilizador são encriptados usando a biblioteca `Security Crypto` e o Android Keystore. Assim, os dados não ficam visíveis no disco.
- **API (Rede)**: Usei o Retrofit para perguntar ao "Have I Been Pwned" se a password é segura. Para a enviar com segurança, usei o "k-Anonymity" (só envio os primeiros 5 caracteres de um tipo de código chamado SHA-1).
- **Lista de Passwords**: Para apresentar muitas passwords, usei a clássica `RecyclerView` com um `VaultAdapter` e um menu flutuante.
- **Injeção de Dependências**: Usei o Dagger Hilt para inicializar as classes e fazer a ligação do ViewModel com a base de dados de forma automática.

## 5. Testing and Validation
Para testar o programa liguei a aplicação no meu emulador e fui testando tudo passo a passo.
Os testes basearam-se em ver se:
- O registo do PIN pela primeira vez não falha.
- Se meter o PIN errado 3 vezes, não deixa entrar no cofre principal.
- Editar e apagar muda realmente os dados que aparecem na janela (CRUD completo).
- Clicar para mostrar a password escondida no campo "Password" em vez de mostrar "***" funciona.

Limitação: se o telemóvel formatar ou se esquecer do PIN, perde-se o acesso ao cofre, porque os dados não são guardados em servidores online e todo o acesso fica barrado.

## 6. Usage Instructions
Para testar e correr este projeto no computador:
- Instalar ou usar o **Android Studio** (versão recomendada Iguana ou superior).
- Fazer clone do repositório ou abrir a pasta `VaultGuard` através do IDE.
- O IDE vai pedir para fazer o "Sync Project with Gradle Files", clicar OK.
- Selecionar o emulador em cima (com Android versão 8.0 Oreo ou superior) e carregar no botão verde "Run".

---

# Autonomous Software Engineering Sections

## 7. Prompting Strategy
Pedi ajuda a uma Inteligência Artificial para fazer grande parte do código. Primeiro fiz prompts compridos a explicar a app e as tecnologias escolhidas (Kotlin, Room, MVVM e Material Design 3). Ao longo do desenvolvimento de cada ecrã fui fazendo prompts menores, tipo "cria-me o XML do gerador de passwords" ou "corrige esta cor que está errada do botão".

## 8. Autonomous Agent Workflow
O agente inteligente ajudou na organização e fez logo de seguida uma listagem (um ficheiro Markdown de plano) do que ia fazer primeiro, base de dados, UI, etc. Quando o Android Studio dava algum erro a carregar os pacotes ou falhas no Gradle, fazia copy/paste para a IA e ela ensinava os passos que tinha de modificar e escrevia a correção na hora.

## 9. Verification of AI-Generated Artifacts
Sempre que a IA criava código novo, copiava para os ficheiros originais no meu computador e construía a app. Via no emulador se o texto aparecia bem no ecrã e testava os botões para ver se não rebentava.

## 10. Human vs AI Contribution
A inteligência artificial programou grande parte dos processos Kotlin e construiu os ecrãs todos. A minha parte neste projeto foi escolher a ideia, delinear o caminho a tomar, dizer o que mudar se não gostasse de alguma parte visual e garantir que no final a app estava toda montada numa só peça. Fui principalmente o gestor das ideias da IA.

## 11. Ethical and Responsible Use
Havia sempre o risco da IA escolher formas com pouca segurança, uma vez que se trata de um gestor de passwords. Por minha iniciativa informei-a de que devia usar os mecanismos mais modernos e seguros disponíveis de criptografia (Keystore em vez de partilhas simples), e assegurei que tudo foi desenvolvido sem pôr em causa a privacidade do utilizador do VaultGuard.

---

# Development Process

## 12. Version Control and Commit History
Durante o projeto usei o Git. Quando a configuração inicial da app corria sem falhas fiz um commit. Quando um dos ecrãs (Login, Gerador de senhas) ficava pronto sem bugs também ia guardando e enviando para o GitHub para manter o registo do que foi evoluindo.

## 13. Difficulties and Lessons Learned
A maior dificuldade foi afinar o Gradle, onde ocorreram muitas versões incompatíveis no Kotlin que empancavam a compilação do Android (problemas com o DataBinding ou referências de ícones nos manifests). Aprendi bastante sobre como as rotas entre ecrãs com o Jetpack Navigation funcionam sem necessitar de mais atividades pesadas.

## 14. Future Improvements
Posso melhorar este projeto com:
- Adicionar login por biometria (impressão digital) se o aparelho permitir.
- Fazer cópias de segurança locais em texto para colocar numa Drive ou passar pelo computador, com a forma de recuperar tudo de volta.

---

## 15. AI Usage Disclosure
Neste trabalho académico declaro a utilização considerável de modelos de Inteligência Artificial no âmbito de Autonomous Software Engineering. Contudo, todo o conteúdo submetido e as verificações realizadas são da minha plena responsabilidade, conforme as regras de conduta previstas na unidade curricular.
