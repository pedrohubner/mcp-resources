# Guia de Atualização de Versão Java

## Versionamento do Documento

- Documento: `ATUALIZAÇÃO_VERSÃO_JAVA.md`
- Versão: `1.0.0`
- Data da última atualização: `2026-02-17`
- Responsável: `Agente Codex`
- Modelo utilizado: `MICC` (Modelo de Migração por Compatibilidade Cruzada)

### Histórico de versões

| Versão | Data       | Alterações |
|--------|------------|------------|
| 1.0.0  | 2026-02-17 | Versão inicial com fluxo completo de migração, validação e modernização pós-upgrade |

## Objetivo

Padronizar a atualização de versões do Java com segurança, garantindo:

- compatibilidade entre Java, framework, build tool e dependências;
- adoção das versões mais recentes **e compatíveis entre si**;
- continuidade do funcionamento da aplicação sem quebra de comportamento.

## Entradas Obrigatórias (informadas pelo usuário)

Antes de iniciar, o usuário deve informar:

- versão atual do Java;
- versão alvo do Java;
- framework principal (ex.: Spring, Spring Boot, Quarkus, Micronaut);
- gerenciador de dependências/build (Gradle ou Maven);
- versão atual do build tool (ex.: Gradle Wrapper, Maven);
- contexto de execução (local/CI/CD, Docker, runtime em produção).

## Regras Mandatórias

1. Sempre avaliar compatibilidade cruzada entre:
   - versão do Java;
   - framework e versão do framework;
   - dependency management (BOM, parent, plugins, platform);
   - versões das dependências diretas e transitivas;
   - versão do build tool (Gradle Wrapper/Maven + plugins).
2. Se houver incompatibilidade, **sempre** selecionar as versões **mais recentes e compatíveis entre si**.
3. É permitido consultar documentação na web (e é recomendado quando necessário), priorizando fontes oficiais.
4. Nenhuma atualização deve ser considerada concluída se houver quebra funcional, testes falhando ou regressão.
5. Toda decisão de versão deve ser rastreável e registrada em um documento ADR (ARCHITECTURE_DECISION_RECORD.md).

## Fontes de Consulta Recomendadas

- documentação oficial do Java/OpenJDK;
- matriz de compatibilidade do framework (ex.: Spring Boot x Java);
- release notes de framework, plugins e bibliotecas críticas;
- documentação do Gradle/Maven e plugins usados no projeto.

## Fluxo Padrão de Migração (MICC)

### Etapa 1: Descoberta e Inventário

- mapear versões atuais de Java, framework, build tool e dependências;
- identificar uso de BOMs, parent poms, version catalogs, plugin management;
- levantar cobertura de testes existente (unitário, integração, e2e/smoke).

### Etapa 2: Matriz de Compatibilidade

- validar se a versão alvo do Java é suportada pelo framework atual;
- validar se a versão atual do framework suporta a versão alvo do Java;
- validar compatibilidade do build tool com a versão alvo do Java;
- validar compatibilidade das dependências críticas com novo conjunto.

### Etapa 3: Estratégia de Atualização

- definir ordem de upgrade para minimizar risco:
  1. build tool/plugins;
  2. framework/dependency management;
  3. dependências incompatíveis;
  4. versão do Java;
- priorizar sempre versões mais recentes estáveis compatíveis entre si.

### Etapa 4: Execução Técnica

- atualizar configuração de Java (toolchain/sourceCompatibility/targetCompatibility);
- atualizar Gradle Wrapper ou Maven, quando necessário;
- atualizar framework/BOM/dependency management;
- ajustar dependências e plugins incompatíveis;
- revisar configurações que mudaram entre versões (propriedades, flags, defaults).

### Etapa 5: Validação Obrigatória

- build completo sem erros;
- suíte de testes passando;
- aplicação sobe com sucesso;
- smoke tests dos fluxos críticos passando;
- validação de logs/observabilidade sem erros novos.

### Etapa 6: Critério de Conclusão

A migração só é concluída quando:

- todos os checks técnicos estiverem verdes;
- não houver regressão funcional;
- documentação de versões e decisões estiver atualizada.

## Template de Execução por Migração

Use este modelo para cada atualização:

```md
## Migração Java: <VERSÃO_ORIGEM> -> <VERSÃO_DESTINO>

### Contexto
- Projeto:
- Framework (versão):
- Build tool (versão):
- Dependency management:

### Matriz de compatibilidade
- Java x Framework:
- Framework x Dependências:
- Java x Build tool:
- Plugins críticos:

### Decisões de versão
- Java:
- Framework:
- Build tool:
- Dependências críticas:
- Justificativa (versões mais recentes compatíveis):

### Execução
- Alterações realizadas:
- Arquivos impactados:

### Validação
- Build:
- Testes:
- Smoke:
- Resultado final:

### Status
- [ ] Concluído sem regressão
- [ ] Necessita ajustes
```

## Pós-Migração (obrigatório após sucesso)

Depois de concluir a migração com sucesso:

1. destacar as principais mudanças da versão Java de destino (linguagem, runtime, GC, desempenho, deprecações);
2. propor ao usuário uma varredura no código para adoção segura de melhorias;
3. aplicar apenas mudanças que não quebrem comportamento e mantenham tudo funcionando;
4. validar novamente com testes e smoke após qualquer modernização de código.

## Checklist Final

- [ ] Versões mais recentes e compatíveis entre si foram aplicadas
- [ ] Compatibilidade Java/framework/build/dependências foi comprovada
- [ ] Build e testes passaram
- [ ] Aplicação validada em execução
- [ ] Decisões e versões registradas com versionamento
- [ ] Propostas de melhorias pós-migração documentadas
