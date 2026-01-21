# Exemplo de Implementação com CrewAI

## 📚 Visão Geral

Este documento fornece um exemplo prático de como implementar o sistema de 4 agentes usando CrewAI em Python.

## 🛠️ Requisitos

```bash
pip install crewai
pip install crewai-tools
pip install langchain
pip install openai  # ou anthropic, google-generativeai
```

## 🔑 Configuração de Variáveis de Ambiente

Crie um arquivo `.env`:

```bash
OPENAI_API_KEY=sk-...
# ou
ANTHROPIC_API_KEY=sk-ant-...
# ou
GOOGLE_API_KEY=...
```

## 💻 Código Exemplo

### 1. Estrutura Básica (`crew_consultoria.py`)

```python
from crewai import Agent, Task, Crew, Process
from langchain_openai import ChatOpenAI
import os
from dotenv import load_dotenv

load_dotenv()

# Configurar LLM
llm = ChatOpenAI(
    model="gpt-4-turbo-preview",
    temperature=0.7
)

# ===== AGENTE 1: CONSULTOR =====

consultor = Agent(
    role="Consultor Senior de Negócios",
    goal="Coletar informações completas e estruturadas sobre a empresa cliente",
    backstory="""Você é um consultor experiente com 15 anos de atuação em diagnóstico empresarial.
    Possui MBA e especialização em consultoria organizacional. É conhecido por sua habilidade em
    fazer as perguntas certas e extrair informações críticas de forma empática e profissional.""",
    verbose=True,
    allow_delegation=False,
    llm=llm
)

# ===== AGENTE 2: ANALISTA =====

analista = Agent(
    role="Analista de Negócios Senior",
    goal="Identificar entre 10-20 pendências críticas baseado nas informações coletadas",
    backstory="""Especialista com 12 anos analisando empresas de pequeno e médio porte.
    Possui habilidade única para identificar gaps estruturais, riscos financeiros e oportunidades
    de crescimento. Seu olhar analítico transforma dados em insights acionáveis.""",
    verbose=True,
    allow_delegation=False,
    llm=llm
)

# ===== AGENTE 3: ESTRATEGISTA =====

estrategista = Agent(
    role="Estrategista de Negócios",
    goal="Classificar pendências na Matriz de Eisenhower e definir ordem de execução",
    backstory="""Estrategista com 10 anos de experiência em priorização e roadmaps estratégicos.
    Expert em Matriz de Eisenhower e metodologias de priorização. Transforma longas listas de
    tarefas em planos estratégicos executáveis e priorizados.""",
    verbose=True,
    allow_delegation=False,
    llm=llm
)

# ===== AGENTE 4: EXECUTOR =====

executor = Agent(
    role="Gerente de Projetos Ágil",
    goal="Transformar a matriz priorizada em um Kanban detalhado pronto para execução",
    backstory="""Gerente de projetos com 8 anos implementando Kanban e metodologias ágeis.
    Especialista em transformar estratégia em execução. Cria boards Kanban claros, acionáveis
    e com todas as informações necessárias para o time executar com excelência.""",
    verbose=True,
    allow_delegation=False,
    llm=llm
)

# ===== TAREFAS =====

task1_intake = Task(
    description="""Realize o intake completo da empresa cliente.
    
    Siga o Contrato de Intake (contrato-intake.md) e colete TODAS as informações necessárias:
    - Identificação da Empresa
    - Perfil do Negócio  
    - Estrutura Organizacional
    - Situação Financeira
    - Processos e Operações
    - Estratégia e Planejamento
    - Desafios e Objetivos
    
    Faça perguntas de follow-up quando necessário. Gere um relatório estruturado completo.""",
    agent=consultor,
    expected_output="Relatório de Intake completo em markdown com todas as informações coletadas"
)

task2_analise = Task(
    description="""Analise o Relatório de Intake e identifique 10-20 pendências críticas.
    
    Para cada pendência, detalhe:
    - Título descritivo
    - Categoria (Financeiro, Processos, Pessoas, Tecnologia, Estratégia, Marketing)
    - Descrição completa
    - Impacto (Alto/Médio/Baixo) com justificativa
    - Esforço (Alto/Médio/Baixo) com justificativa
    - Evidências do Intake que suportam
    
    Foque em gaps estruturais, riscos, ineficiências e oportunidades.""",
    agent=analista,
    expected_output="Lista de 10-20 pendências detalhadas em markdown",
    context=[task1_intake]
)

task3_priorizacao = Task(
    description="""Classifique todas as pendências na Matriz de Eisenhower.
    
    Para cada pendência, determine:
    - Em qual quadrante se encaixa (Q1, Q2, Q3, Q4)
    - Por que é urgente (ou não)
    - Por que é importante (ou não)
    - Prazo sugerido
    - Dependências
    
    Defina ordem de execução com marcos (semanas 1-2, mês 1, meses 2-3, etc).""",
    agent=estrategista,
    expected_output="Matriz de Eisenhower completa com classificação e ordem de execução",
    context=[task2_analise]
)

task4_kanban = Task(
    description="""Crie um Kanban executável baseado na Matriz priorizada.
    
    Estruture em 4 colunas: Backlog, To Do, In Progress, Done
    
    Para cada card, detalhe:
    - Título e categoria
    - Prioridade (quadrante)
    - Prazo e esforço estimado
    - Passos de execução detalhados
    - Responsável sugerido
    - Recursos necessários
    - Critérios de aceitação
    - Dependências
    
    Inclua métricas e alertas relevantes.""",
    agent=executor,
    expected_output="Kanban completo e pronto para execução em formato markdown",
    context=[task3_priorizacao]
)

# ===== CREW =====

crew = Crew(
    agents=[consultor, analista, estrategista, executor],
    tasks=[task1_intake, task2_analise, task3_priorizacao, task4_kanban],
    process=Process.sequential,
    verbose=2
)

# ===== EXECUÇÃO =====

if __name__ == "__main__":
    print("\n=== Sistema de Agentes de Consultoria ===")
    print("Iniciando processo automatizado...\n")
    
    # Input inicial (dados da empresa)
    dados_empresa = input("Cole os dados básicos da empresa (ou pressione Enter para exemplo):\n")
    
    if not dados_empresa:
        dados_empresa = """Empresa: TechStart Soluções
        Setor: Tecnologia/SaaS
        Porte: Pequena (8 colaboradores)
        Faturamento: R$ 80k/mês
        Desafio principal: Escalar operações e melhorar processos"""
    
    result = crew.kickoff(inputs={'empresa_info': dados_empresa})
    
    print("\n=== RESULTADO FINAL ===")
    print(result)
    
    # Salvar output
    with open('resultado_consultoria.md', 'w', encoding='utf-8') as f:
        f.write(str(result))
    
    print("\n✅ Kanban salvo em: resultado_consultoria.md")
```

---

## 📝 Arquivo de Dependências (`requirements.txt`)

```
crewai>=0.28.0
crewai-tools>=0.2.0
langchain>=0.1.0
langchain-openai>=0.0.5
python-dotenv>=1.0.0
openai>=1.12.0
```

---

## ▶️ Como Executar

### 1. Instalar dependências
```bash
pip install -r requirements.txt
```

### 2. Configurar API Key
Crie arquivo `.env`:
```
OPENAI_API_KEY=sua-chave-aqui
```

### 3. Executar
```bash
python crew_consultoria.py
```

### 4. Resultado
O sistema irá:
1. Coletar informações (Consultor)
2. Identificar pendências (Analista)
3. Priorizar com Eisenhower (Estrategista)
4. Gerar Kanban (Executor)
5. Salvar em `resultado_consultoria.md`

---

## 🎨 Personalizações

### Trocar o LLM

**Para Anthropic Claude:**
```python
from langchain_anthropic import ChatAnthropic

llm = ChatAnthropic(
    model="claude-3-opus-20240229",
    temperature=0.7
)
```

**Para Google Gemini:**
```python
from langchain_google_genai import ChatGoogleGenerativeAI

llm = ChatGoogleGenerativeAI(
    model="gemini-1.5-pro",
    temperature=0.7
)
```

### Ajustar Temperatura
- `temperature=0.3` - Mais determinístico/conservador
- `temperature=0.7` - Balanceado (recomendado)
- `temperature=1.0` - Mais criativo/variável

### Adicionar Ferramentas
```python
from crewai_tools import FileReadTool, WebScraperTool

consultor_tools = [
    FileReadTool('contrato-intake.md'),
    WebScraperTool()
]

consultor = Agent(
    ...,
    tools=consultor_tools
)
```

---

## 💡 Dicas de Uso

1. **Primeira Execução**: Teste com dados simples para entender o fluxo
2. **Custos**: GPT-4 pode custar $0.50-2.00 por execução completa
3. **Tempo**: Processo completo leva 5-15 minutos
4. **Qualidade**: Revise sempre os outputs antes de entregar ao cliente
5. **Iteração**: Ajuste os prompts baseado nos resultados

---

## 🔧 Troubleshooting

### Erro de API Key
```
Verifique se o arquivo .env existe e contém a chave correta
```

### Agente não responde
```
Aumente o timeout ou reduza a complexidade da task
```

### Output incompleto
```
Aumente o max_tokens do LLM ou divida a task em partes menores
```

---

## 📚 Recursos Adicionais

- [Documentação CrewAI](https://docs.crewai.com/)
- [Exemplos CrewAI](https://github.com/joaomdmoura/crewAI-examples)
- [LangChain Docs](https://python.langchain.com/)

---

**Desenvolvido para DetectaBI** | Sistema de Consultoria Automatizada
