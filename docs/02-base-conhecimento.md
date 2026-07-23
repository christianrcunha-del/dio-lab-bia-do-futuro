# Base de Conhecimento

## Dados Utilizados

O agente **Bia do Futuro** utiliza arquivos da pasta `data` para contextualizar interações e personalizar recomendações financeiras:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores e manter histórico de conversas |
| `perfil_investidor.json` | JSON | Personalizar recomendações e definir nível de risco adequado (conservador, moderado, agressivo) |
| `produtos_financeiros.json` | JSON | Sugerir produtos financeiros adequados ao perfil e objetivos do cliente |
| `transacoes.csv` | CSV | Analisar padrão de gastos, categorizar despesas e identificar oportunidades de economia |
| `categorias_gastos.json` | JSON | Classificar automaticamente as transações do usuário em categorias padronizadas |

> [!TIP]
> **Quer um conjunto de dados mais robusto?** Você pode utilizar conjuntos de dados públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

Os dados foram expandidos e adaptados para melhor servir ao contexto do agente financeiro:

- **Enriquecimento de Transações**: Cada transação foi associada a uma categoria de gasto e a um motivo (necessidade, impulso, planejado)
- **Perfil Investidor Expandido**: Incluídos campos como tolerância ao risco, objetivos financeiros de curto/médio/longo prazo, e preferências de investimento
- **Histórico de Recomendações**: Adicionado registro das recomendações anteriores aceitas ou rejeitadas para melhorar personalização
- **Dados Demográficos**: Idade, profissão, renda mensal e localização para contextualizar melhor o perfil do usuário
- **Métricas de Saúde Financeira**: Taxa de poupança, índice de endividamento, escore de educação financeira

---

## Estratégia de Integração

### Como os dados são carregados?

Os dados são carregados através de uma abordagem em camadas:

1. **Inicialização da Sessão**: Ao iniciar a conversa, o sistema carrega o perfil do cliente (JSON) e o histórico de transações (CSV)
2. **Carregamento Dinâmico**: Conforme a conversa progride, dados adicionais são consultados sob demanda (produtos financeiros, histórico de atendimento anterior)
3. **Cache em Memória**: Os dados carregados são mantidos em memória durante a sessão para resposta rápida
4. **Processamento de Entrada**: O Processador de Entrada valida e estrutura os dados antes de encaminhá-los aos motores de análise

### Como os dados são usados no prompt?

Os dados seguem uma estratégia híbrida:

- **System Prompt Estático**: Inclui contexto sobre a persona "Bia do Futuro", tom de voz, valores e limitações do agente
- **Contexto Dinâmico**: O perfil e histórico do cliente são injetados no contexto da conversa a cada turno, permitindo decisões personalizadas
- **Consulta sob Demanda**: Quando o usuário faz uma pergunta específica, a base de conhecimento é consultada para fornecer informações precisas (produtos disponíveis, análises de transações)
- **Validação com Dados Reais**: Todas as recomendações são validadas contra os dados do cliente antes de serem apresentadas

---

## Exemplo de Contexto Montado

Aqui está um exemplo de como os dados são formatados para o agente processar:

```json
{
  "usuario": {
    "id": "usr_12345",
    "nome": "João Silva",
    "idade": 28,
    "profissao": "Desenvolvedor",
    "renda_mensal": 5500,
    "localizacao": "São Paulo, SP"
  },
  "perfil_investidor": {
    "tolerancia_risco": "moderado",
    "objetivos": [
      "Emergência (3-6 meses)",
      "Aposentadoria (30 anos)",
      "Comprar casa (5 anos)"
    ],
    "capital_inicial": 15000,
    "aporte_mensal": 1000,
    "experiencia": "iniciante"
  },
  "transacoes_recentes": [
    {
      "data": "2024-01-15",
      "descricao": "Supermercado",
      "valor": 250,
      "categoria": "Alimentação",
      "tipo_gasto": "necessidade"
    },
    {
      "data": "2024-01-14",
      "descricao": "Cinema",
      "valor": 60,
      "categoria": "Entretenimento",
      "tipo_gasto": "impulso"
    }
  ],
  "saude_financeira": {
    "taxa_poupanca": 0.18,
    "indice_endividamento": 0.12,
    "escore_educacao": 6.5,
    "fundo_emergencia": "incompleto"
  }
}
```

Com essa estrutura, o agente pode personalizar suas recomendações e oferecer orientação contextualizada a cada interação.
