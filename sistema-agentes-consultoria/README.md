# Sistema de Agentes de IA para Consultoria Empresarial

## Visão Geral

Sistema automatizado de consultoria que utiliza 4 agentes especializados de IA para analisar empresas, identificar pendências críticas, priorizar ações e gerar planos de ação estruturados automaticamente.

## 🎯 Objetivo

Automatizar o processo de diagnóstico empresarial e planejamento estratégico, transformando informações básicas da empresa em um plano de ação priorizado e organizado em formato Kanban.

## 🤖 Arquitetura dos Agentes

### Fluxo de Trabalho

```
Empresa fornece dados → Agente 1: Consultor → Agente 2: Analista → Agente 3: Estrategista → Agente 4: Executor → Kanban Pronto
```

### Agentes Especializados

#### 1️⃣ Agente Consultor (Intake Specialist)
**Função**: Coletar informações estruturadas da empresa
- Aplica questionário baseado no "Contrato de Intake"
- Faz perguntas de follow-up quando necessário
- Organiza dados em formato estruturado
- Identifica gaps de informação críticos

**Output**: Relatório estruturado com dados da empresa

#### 2️⃣ Agente Analista (Problem Identifier)
**Função**: Identificar 10-20 pendências/oportunidades críticas
- Analisa dados coletados pelo Consultor
- Identifica gaps, riscos e oportunidades
- Lista pendências com descrição detalhada
- Estima impacto e esforço para cada item

**Output**: Lista de 10-20 pendências priorizadas

#### 3️⃣ Agente Estrategista (Priority Manager)
**Função**: Priorizar usando Matriz de Eisenhower
- Classifica cada pendência em 4 quadrantes
- Define nível de urgência e importância
- Sugere ordem de execução
- Identifica dependências entre tarefas

**Output**: Matriz de Eisenhower completa com classificação

#### 4️⃣ Agente Executor (Kanban Builder)
**Função**: Criar Kanban estruturado para execução
- Organiza tarefas em colunas (Backlog, To Do, In Progress, Done)
- Define responsáveis sugeridos
- Estima prazos realistas
- Cria cards detalhados com critérios de aceitação

**Output**: Kanban completo em formato Markdown/JSON

## 📋 Contrato de Intake

O sistema utiliza um questionário estruturado para coletar informações essenciais:

Ver arquivo: [`contrato-intake.md`](./contrato-intake.md)

## 📦 Estrutura de Arquivos

```
sistema-agentes-consultoria/
├── README.md                    # Este arquivo
├── contrato-intake.md           # Template de coleta de dados
├── prompts/
│   ├── agente-1-consultor.md    # Prompt do Agente Consultor
│   ├── agente-2-analista.md     # Prompt do Agente Analista
│   ├── agente-3-estrategista.md # Prompt do Agente Estrategista
│   └── agente-4-executor.md     # Prompt do Agente Executor
├── implementacao/
│   ├── crew_consultoria.py      # Código CrewAI completo
│   ├── requirements.txt         # Dependências Python
│   └── config.yaml              # Configurações dos agentes
└── exemplos/
    ├── exemplo-empresa-tech.md  # Caso de uso: Startup Tech
    └── exemplo-empresa-varejo.md # Caso de uso: Varejo
```

## 🚀 Como Usar

### Opção 1: Manual (para testes)
1. Preencha o [`contrato-intake.md`](./contrato-intake.md) com dados da empresa
2. Execute cada agente sequencialmente usando os prompts
3. Revise os outputs em cada etapa

### Opção 2: Automatizado (CrewAI)
1. Instale dependências: `pip install -r implementacao/requirements.txt`
2. Configure variáveis de ambiente (API keys)
3. Execute: `python implementacao/crew_consultoria.py`
4. Forneça dados da empresa quando solicitado
5. Aguarde o sistema gerar o Kanban completo

## 📊 Outputs Esperados

### 1. Relatório de Intake
- Dados estruturados da empresa
- Contexto de mercado
- Objetivos e desafios

### 2. Lista de Pendências (10-20 itens)
- Descrição detalhada
- Impacto estimado
- Esforço necessário

### 3. Matriz de Eisenhower
```
┌────────────────────┬────────────────────┐
│ Q1: URGENTE +      │ Q2: NÃO URGENTE + │
│     IMPORTANTE     │     IMPORTANTE     │
│ Fazer Agora        │ Agendar            │
├────────────────────┼────────────────────┤
│ Q3: URGENTE +      │ Q4: NÃO URGENTE + │
│     NÃO IMPORTANTE │     NÃO IMPORTANTE │
│ Delegar            │ Eliminar           │
└────────────────────┴────────────────────┘
```

### 4. Kanban Estruturado
```markdown
## Backlog
- [ ] Tarefa X (Q2 - Importante, não urgente)
- [ ] Tarefa Y (Q3 - Urgente, não importante)

## To Do (Próximas 2 semanas)
- [ ] Tarefa A (Q1 - Urgente e importante) - Prazo: 3 dias
- [ ] Tarefa B (Q1 - Urgente e importante) - Prazo: 5 dias

## In Progress
- [ ] (vazio inicialmente)

## Done
- [ ] (vazio inicialmente)
```

## 🛠️ Tecnologias Utilizadas

- **CrewAI**: Framework para orquestração de agentes
- **LangChain**: Gestão de prompts e memória
- **OpenAI GPT-4** ou **Google Gemini**: LLMs principais
- **Python 3.10+**: Linguagem de implementação

## 🎯 Casos de Uso

### 1. Startups em Estação Inicial
- Identificação de gaps em estruturação
- Priorização de processos críticos
- Roadmap de crescimento

### 2. Empresas em Crescimento
- Otimização de processos
- Escala de operações
- Gestão de equipes

### 3. Empresas em Reestruturação
- Diagnóstico de problemas estruturais
- Plano de recuperação
- Otimização de custos

## ✅ Benefícios

- ⚡ **Velocidade**: Geração de plano completo em minutos vs dias
- 🎯 **Precisão**: Análise baseada em metodologias comprovadas
- 📊 **Consistencia**: Todos os clientes recebem o mesmo nível de análise
- 💰 **Escalabilidade**: Atenda mais clientes sem aumentar proporcionalmente o time
- 🔄 **Atualizável**: Fácil adaptar para novos setores e contextos

## 📝 Próximos Passos

1. Revise o [`contrato-intake.md`](./contrato-intake.md)
2. Leia os prompts na pasta `prompts/`
3. Teste o sistema com um caso real
4. Ajuste conforme necessidade do seu negócio

## 🔗 Links Úteis

- [Gestão do Tempo](../desenvolvimento-pessoal/gestao-do-tempo.md)
- [CrewAI Documentação](https://docs.crewai.com/)
- [Matriz de Eisenhower](https://pt.wikipedia.org/wiki/Matriz_de_Eisenhower)

## 💬 Suporte

Para dúvidas ou sugestões sobre este sistema, entre em contato ou abra uma issue no repositório.

---

**Desenvolvido para DetectaBI** | Consultoria Empresarial Automatizada com IA
