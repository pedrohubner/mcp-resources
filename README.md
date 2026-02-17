# MCP Resources Playground

Este projeto tem como objetivo servir como uma fonte de **MCP Resources** para um **MCP Server** (ainda não implementado), disponibilizando documentos de assuntos variados para validar e testar o consumo de recursos via protocolo MCP.

## Objetivo

Centralizar conteúdos de referência em um único repositório para:

- testar leitura e listagem de resources;
- validar busca e seleção de contexto;
- simular cenários reais de uso com domínios distintos;
- apoiar a evolução de um MCP Server que consumirá estes recursos.

## Escopo Atual

No momento, este repositório concentra-se exclusivamente na preparação e organização dos documentos que serão expostos como resources.

O MCP Server será criado em etapa posterior. Assim que estiver disponível, este `README` receberá instruções adicionais de integração, execução e validação ponta a ponta.

## Estrutura Esperada (Inicial)

Uma organização recomendada para os conteúdos:

```text
.
├── resources/
│   ├── tecnologia/
│   ├── produto/
│   ├── processos/
│   └── geral/
└── README.md
```

> A estrutura acima é sugestiva e pode ser ajustada conforme as necessidades de teste.

## Diretrizes de Conteúdo

Para manter consistência e facilitar testes:

- use nomes de arquivos descritivos;
- prefira documentos curtos e objetivos por tema;
- mantenha versionamento via Git para rastrear mudanças;
- evite dados sensíveis ou informações restritas.

## Próximos Passos

1. Popular o diretório de resources com documentos de temas diferentes.
2. Definir metadados e convenções de organização dos arquivos.
3. Implementar o MCP Server consumidor destes resources.
4. Atualizar este `README` com instruções de setup, execução e troubleshooting.

## Status

Projeto em fase inicial de estruturação de conteúdo para testes de MCP Resources.
