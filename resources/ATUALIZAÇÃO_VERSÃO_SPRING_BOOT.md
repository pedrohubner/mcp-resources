# Guia de Atualização de Versão Spring Boot

## Versionamento do Documento

- Documento: `ATUALIZAÇÃO_VERSÃO_SPRING_BOOT.md`
- Versão: `1.0.0`
- Data da última atualização: `2026-02-18`
- Responsável: `Agente Codex`
- Modelo utilizado: `MICC-SB` (Modelo de Migração por Compatibilidade Cruzada - Spring Boot)

### Histórico de versões

| Versão | Data       | Alterações |
|--------|------------|------------|
| 1.0.0  | 2026-02-18 | Versão inicial com fluxo completo de migração Spring Boot, agnóstico de versão |

## Objetivo

Padronizar a atualização de versões do Spring Boot com segurança, garantindo:

- compatibilidade entre Spring Boot, Java, Spring Cloud, dependências e tecnologias integradas;
- adoção das versões mais recentes **e compatíveis entre si**;
- continuidade do funcionamento da aplicação sem quebra de comportamento;
- migração segura de breaking changes estruturais entre major versions.

## Entradas Obrigatórias (informadas pelo usuário)

Antes de iniciar, o usuário deve informar:

- versão atual do Spring Boot;
- versão alvo do Spring Boot;
- versão atual e alvo do Java;
- tipo de aplicação (WebFlux, MVC, Batch, Cloud Function, etc.);
- stack de tecnologias utilizadas:
  - Spring Cloud (e versão atual);
  - observabilidade (Sleuth, Micrometer Tracing, Actuator, etc.);
  - persistência (JPA/Hibernate, MongoDB, Redis, R2DBC, etc.);
  - mensageria (Kafka, RabbitMQ, etc.);
  - segurança (Spring Security, OAuth2, etc.);
  - outras dependências críticas;
- gerenciador de dependências/build (Gradle ou Maven);
- versão atual do build tool;
- contexto de execução (local/CI/CD, Docker, K8s, runtime em produção).

## Regras Mandatórias

1. Sempre avaliar compatibilidade cruzada entre:
   - versão do Spring Boot;
   - versão do Java;
   - versão do Spring Cloud (se aplicável);
   - versão do Spring Framework subjacente;
   - dependency management (BOM, parent, plugins);
   - versões das dependências do stack (Kafka, Redis, MongoDB, etc.);
   - versão do build tool e plugins.
2. Se houver incompatibilidade, **sempre** selecionar as versões **mais recentes e compatíveis entre si**.
3. Identificar e documentar **breaking changes** entre versões (major releases requerem atenção especial).
4. Consultar documentação oficial e release notes é **mandatório** para major upgrades.
5. Nenhuma atualização deve ser considerada concluída se houver quebra funcional, testes falhando ou regressão.
6. Toda decisão de versão deve ser rastreável e registrada em ADR (ARCHITECTURE_DECISION_RECORD.md).

## Fontes de Consulta Recomendadas

- [Spring Boot Release Notes](https://github.com/spring-projects/spring-boot/wiki);
- [Spring Boot Migration Guides](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Migration-Guide) (adaptar para versão alvo);
- [Spring Cloud Release Train](https://spring.io/projects/spring-cloud#learn);
- matriz de compatibilidade Spring Boot x Java x Spring Cloud;
- release notes de bibliotecas críticas (Micrometer, Reactor, drivers de banco, etc.);
- documentação do Gradle/Maven e plugins usados no projeto.

## Categorias de Breaking Changes por Major Version

### Spring Boot 2.x → 3.x (exemplo de referência para estruturar análise)

- **Jakarta EE Migration**: `javax.*` → `jakarta.*` (impacta TODAS as imports);
- **Observabilidade**: Spring Cloud Sleuth descontinuado → Micrometer Tracing + Brave/OTEL;
- **Properties**: renomeações, deprecações, mudanças de formato;
- **Spring Security**: configurações deprecadas removidas;
- **Spring Data**: mudanças em repositories, queries, auditing;
- **WebFlux/MVC**: mudanças de comportamento em roteamento, CORS, error handling;
- **Actuator**: endpoints movidos/renomeados;
- **Kafka/Redis/MongoDB**: drivers e configurações atualizadas.

### Próximas Versions (4.x, 5.x...) - Template de Análise

Para cada major version futura, seguir estrutura:

- **Core Changes**: mudanças no Spring Framework base;
- **Dependency Upgrades**: versões obrigatórias de Java, bibliotecas core;
- **Deprecations Removed**: APIs removidas da versão anterior;
- **Configuration Changes**: properties, auto-configuration, starters;
- **Technology Stack Updates**: mudanças em integração com tecnologias (DB, mensageria, etc.);
- **Observability & Monitoring**: mudanças em métricas, tracing, logging;
- **Security**: atualizações de práticas, algoritmos, configurações.

## Fluxo Padrão de Migração (MICC-SB)

### Etapa 1: Descoberta e Inventário

- mapear versões atuais de:
  - Spring Boot e Spring Framework;
  - Java;
  - Spring Cloud (se aplicável);
  - dependências críticas do stack;
  - build tool e plugins;
- identificar uso de BOMs, parent poms, version catalogs;
- mapear propriedades customizadas (application.yml/properties);
- levantar cobertura de testes (unitário, integração, e2e);
- identificar uso de APIs deprecadas (static analysis recomendado).

### Etapa 2: Análise de Breaking Changes

Para major upgrades (ex.: 2.x → 3.x, 3.x → 4.x):

- consultar migration guide oficial da versão alvo;
- mapear breaking changes aplicáveis ao projeto:
  - mudanças de pacotes/imports;
  - propriedades removidas/renomeadas;
  - APIs descontinuadas;
  - mudanças de comportamento padrão;
  - dependências obrigatoriamente atualizadas;
- criar checklist específico de refatoração.

### Etapa 3: Matriz de Compatibilidade

- validar se versão alvo do Spring Boot suporta versão alvo do Java;
- validar versão compatível do Spring Cloud (se aplicável);
- validar compatibilidade de cada tecnologia do stack:
  - drivers de banco (MongoDB, Redis, etc.);
  - clients de mensageria (Kafka, RabbitMQ, etc.);
  - bibliotecas de observabilidade (Micrometer, OpenTelemetry, etc.);
  - bibliotecas de segurança (OAuth2, JWT, etc.);
- validar compatibilidade do build tool com versão alvo do Java e Spring Boot;
- documentar versões mais recentes compatíveis para cada dependência.

### Etapa 4: Estratégia de Atualização

Definir ordem de upgrade para minimizar risco:

1. **Pre-flight**: atualizar build tool/plugins se necessário;
2. **Java**: migrar versão do Java (seguindo ATUALIZAÇÃO_VERSÃO_JAVA.md);
3. **Spring Boot Parent/BOM**: atualizar para versão alvo;
4. **Spring Cloud**: atualizar para release train compatível;
5. **Refatoração de Breaking Changes**:
   - imports (ex.: javax → jakarta);
   - properties renomeadas;
   - APIs descontinuadas;
   - configurações obsoletas;
6. **Dependências do Stack**: atualizar para versões compatíveis;
7. **Validação Incremental**: build e testes a cada etapa crítica.

### Etapa 5: Execução Técnica

#### 5.1. Atualização de Versões

- atualizar `spring-boot-starter-parent` ou BOM no Gradle/Maven;
- atualizar Spring Cloud release train (se aplicável);
- atualizar versão do Java (toolchain/sourceCompatibility/targetCompatibility);
- atualizar dependências do stack para versões compatíveis.

#### 5.2. Refatoração de Breaking Changes

**Para cada breaking change identificado**:

- aplicar refatoração necessária (imports, properties, configurações);
- validar build após cada conjunto de mudanças;
- executar testes impactados.

**Exemplos comuns** (adaptar para versão específica):

- **Jakarta EE**: substituir `javax.*` por `jakarta.*`;
- **Observabilidade**: migrar de Sleuth para Micrometer Tracing;
- **Properties**: atualizar nomes/formatos no application.yml/properties;
- **Security**: atualizar configurações SecurityFilterChain;
- **Data**: ajustar queries/repositories com mudanças de API.

#### 5.3. Atualização de Configurações

- revisar application.yml/properties para propriedades:
  - deprecadas/removidas;
  - renomeadas;
  - com mudança de formato;
  - com novo comportamento padrão;
- ajustar configurações de infraestrutura (health checks, actuator, métricas);
- validar configurações de logging e tracing.

### Etapa 6: Validação Obrigatória

- [ ] build completo sem erros de compilação;
- [ ] nenhuma dependência com conflito não resolvido;
- [ ] suíte de testes unitários passando (100%);
- [ ] testes de integração passando;
- [ ] aplicação sobe com sucesso (startup sem erros);
- [ ] smoke tests dos fluxos críticos passando:
  - endpoints REST (WebFlux/MVC);
  - health checks;
  - conexão com bancos de dados;
  - mensageria (produção/consumo);
  - cache (Redis);
  - autenticação/autorização;
  - observabilidade (métricas, traces);
- [ ] validação de logs sem erros/warnings novos;
- [ ] validação de métricas/traces sendo coletadas corretamente;
- [ ] testes de carga/performance (se aplicável).

### Etapa 7: Critério de Conclusão

A migração só é concluída quando:

- todos os checks técnicos da Etapa 6 estiverem verdes;
- não houver regressão funcional;
- não houver degradação de performance;
- documentação de versões e decisões estiver atualizada (ADR);
- equipe estiver ciente de mudanças de comportamento (se houver).

## Template de Execução por Migração

Use este modelo para cada atualização:

```md
## Migração Spring Boot: <VERSÃO_ORIGEM> -> <VERSÃO_DESTINO>

### Contexto
- Projeto:
- Spring Boot (origem → destino):
- Java (origem → destino):
- Spring Cloud (origem → destino):
- Tipo de aplicação (WebFlux/MVC/Batch/etc.):
- Build tool (versão):

### Stack de Tecnologias
- Observabilidade:
- Persistência:
- Mensageria:
- Segurança:
- Outras dependências críticas:

### Breaking Changes Identificados
- [ ] Mudança 1: descrição + impacto + ação
- [ ] Mudança 2: descrição + impacto + ação
- [ ] ...

### Matriz de Compatibilidade
- Spring Boot x Java:
- Spring Boot x Spring Cloud:
- Spring Boot x Stack (Kafka, Redis, MongoDB, etc.):
- Java x Build tool:
- Plugins críticos:

### Decisões de Versão
- Spring Boot:
- Spring Framework:
- Spring Cloud:
- Java:
- Build tool:
- Dependências críticas:
  - Micrometer/Tracing:
  - Kafka:
  - Redis:
  - MongoDB:
  - Outras:
- Justificativa (versões mais recentes compatíveis):

### Estratégia de Execução
1. Pre-flight: [ações]
2. Java: [ações]
3. Spring Boot/Cloud: [ações]
4. Refatoração: [ações]
5. Stack: [ações]

### Execução
- Alterações realizadas:
  - Dependências:
  - Imports/Pacotes:
  - Configurações:
  - Código:
- Arquivos impactados:

### Validação
- [ ] Build: ✅/❌
- [ ] Testes unitários: ✅/❌
- [ ] Testes integração: ✅/❌
- [ ] Startup: ✅/❌
- [ ] Smoke tests: ✅/❌
- [ ] Observabilidade: ✅/❌
- Resultado final:

### Status
- [ ] Concluído sem regressão
- [ ] Necessita ajustes
- [ ] Bloqueado (motivo)
```

## Pós-Migração (obrigatório após sucesso)

Depois de concluir a migração com sucesso:

1. **Documentar mudanças principais** da versão Spring Boot de destino:
   - novos recursos disponíveis;
   - deprecações introduzidas;
   - mudanças de comportamento padrão;
   - melhorias de performance/segurança;
2. **Propor modernização segura**:
   - adoção de novas APIs recomendadas;
   - substituição de código deprecado;
   - otimizações habilitadas pela nova versão;
   - melhorias em observabilidade/segurança;
3. **Aplicar apenas mudanças seguras**:
   - que não quebrem comportamento;
   - que mantenham testes passando;
   - que sejam validadas por smoke tests;
4. **Atualizar documentação do projeto**:
   - requisitos mínimos (Java, Docker, etc.);
   - mudanças em configuração;
   - novos recursos habilitados.

## Checklist Final

- [ ] Versões mais recentes e compatíveis entre si foram aplicadas
- [ ] Compatibilidade Spring Boot/Java/Spring Cloud/Stack foi comprovada
- [ ] Breaking changes foram identificados e resolvidos
- [ ] Build e testes passaram (100%)
- [ ] Aplicação validada em execução com smoke tests
- [ ] Observabilidade funcionando corretamente
- [ ] Decisões e versões registradas com versionamento (ADR)
- [ ] Propostas de melhorias pós-migração documentadas
- [ ] Equipe ciente de mudanças relevantes

## Observações Importantes

### Major Upgrades (ex.: 2.x → 3.x, 3.x → 4.x)

- **Exigem análise detalhada** de migration guides oficiais;
- **Podem envolver refatoração massiva** (ex.: javax → jakarta);
- **Requerem validação extensiva** (testes + smoke + carga);
- **Considerar feature flags** para rollback seguro em produção.

### Minor/Patch Upgrades (ex.: 3.2.x → 3.3.x)

- **Geralmente mais seguros**, mas ainda requerem validação;
- **Focar em release notes** para mudanças de comportamento;
- **Validar dependências transitivas** que possam ter sido atualizadas.

### Ambientes Multi-Módulo/Microserviços

- **Planejar migração incremental** por módulo/serviço;
- **Validar compatibilidade** entre serviços com versões diferentes;
- **Atenção especial** a contratos de API e mensageria.
