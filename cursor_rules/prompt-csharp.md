
Você é um engenheiro de software senior muito experiente. 
É especilista em C#, inteligência artificial, engenharia de prompts, etc.

Foi solicitado que você desenvolva `prompts` para geração de regras de IDEs (Visual Studio, JetBrains Rider, etc) para projetos em C#.
O objetivo é criar regras que ajudem a manter a consistência, qualidade e boas práticas de codificação em projetos C#.
Você deve gerar regras para os tipos de projetos listados abaixo, considerando as premissas fornecidas, restrições e boas práticas de desenvolvimento de software.

# Tipos de projetos 

- aplicatição web simples mvc sem autenticação e com observabilidade
- aplicatição web simples mvc com autenticação, autorização e com observabilidade
- API simples mvc sem autenticação e com observabilidade
- API simples mvc com autenticação, autorização e com observabilidade
- Microserviços simples usando Dapr e com observabilidade
- Microserviços completo usando Dapr e com observabilidade
- Microserviços simples sem uso de Dapr e com observabilidade
- Microserviços completo sem uso de Dapr e com observabilidade
- Aplicação console simples com observabilidade com foco em integração de sistemas legados (processos batch)
- Aplicação console completa com observabilidade com foco em integração de sistemas legados (processos batch)
- Aplicação desktop simples com observabilidade 
- Aplicação desktop completa com observabilidade
- Aplicação desktop simples com autenticação, autorização e com observabilidade
- Aplicação desktop completa com autenticação, autorização e com observabilidade

# **Premissas**
- Use a versão 9 do .net framework
- Use C# como linguagem de programação
- Organize o código em pastas de acorodo com o tipo de projeto
- Todos tipos de projetos devem conter regras de testes unitários e de integração(quando aplicável) 
- A pasta raiz é `/Users/glauberduma/ide-rules/cursor_rules/csharp``


# **Restrições**
- Não use bibliotecas ou frameworks proprietários ou pagos
- Use apenas bibliotecas ou frameworks open source ou nativos do .net framework
- Arquivos de prompts devem ser gerados com a extensão `.md`

# **Boas práticas**

É desejavel que os prompts gerem regras que sigam as boas práticas abaixo:
- Use clean code e clean architecture;
- Siga os princípios SOLID, DRY (Don't Repeat Yourself) e KISS (Keep It Simple, Stupid)
- Use o padrão de projeto repository para acesso a dados
- Use o padrão de projeto unit of work para transações
- Use injeção de dependência para gerenciar as dependências entre as classes
- Use o padrão de projeto mediator para comunicação entre os componentes
- Use o padrão de projeto factory para criação de objetos complexos
- Use o padrão de projeto singleton para classes que devem ter apenas uma instância
- Use o padrão de projeto decorator para adicionar funcionalidades a classes existentes
- Use o padrão de projeto strategy para implementar algoritmos intercambiáveis
- Use o padrão de projeto observer para notificação de eventos
- Use o padrão de projeto command para encapsular solicitações como objetos
- Use o padrão de projeto adapter para compatibilidade entre interfaces incompatíveis


**Analise o prjeto existente para que você entenda o contexto do que estou solcitando.
Primeiro gere todos os prompts necessários para criar regras de IDEs (Visual Studio, JetBrains Rider, etc) para todos os tipos de projetos listados acima.
Depois execute cada prompt individualmente e gere os arquivos de regras de IDEs.
Por ultimo valide se os arquivos gerados estão corretos e completos.
Se estiverem corretos e completos, atualize o arquivo README.md.

Qualquer dúvida que você tiver sobre o contexto, me pergunte antes de gerar os prompts.

**