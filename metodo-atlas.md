# Método ATLAS — Construção de Agentes com Alma

**Metodologia para projetar, validar e manter agentes de IA — em qualquer domínio.**
Versão 1.0 (sucede o CASCO v2.0) · registro funcional

---

## Por que ATLAS

Atlas sustenta o mundo nas costas. É a estrutura invisível que carrega tudo o que está em cima — e quando ela cede, o que está em cima desaba junto, mesmo com tempo bom. Um agente é igual: não é a aparência que decide se ele se sustenta sob pressão, é a estrutura que carrega cada camada. Estrutura íntegra, o agente resiste; comprometida, ele "derrete" na primeira conversa difícil.

Cada letra é uma camada da estrutura, na ordem em que se constrói:

| Letra | Camada | O que é | Ritmo de mudança |
|-------|--------|---------|------------------|
| **A** | Alma | núcleo de critério invariante | quase nunca |
| **T** | Tom | a voz — como fala | às vezes |
| **L** | Lembrança | memória + estado | contínuo |
| **A** | Aptidões | habilidades + ferramentas + modelo | toda hora |
| **S** | Salvaguardas | guardrails, validação, observabilidade | a cada release |

Antecede tudo a **Fase 0 (Mandato + Viabilidade)**. Acima do agente único vem a **Orquestração** (quando um agente vira muitos). Sustenta tudo a **Governança**.

---

## As 5 leis que regem o método

1. **Separação por ritmo de mudança.** Camadas em arquivos separados — nunca um prompt-monolito. Misturar é a causa raiz da deriva.
2. **Especificidade > generalidade.** Alma é feita de compromissos específicos, não de simpatia genérica.
3. **Mostrar > descrever.** Exemplo ensina o que adjetivo não ensina. Todo atributo de voz nasce com par antes/depois.
4. **Critério > regra.** Ensine julgamento com o *porquê*. Regra cobre o previsto; critério cobre o inédito.
5. **Validar contra deriva.** Um agente só tem alma se segura o frame sob pressão. Sem bateria adversarial, você não sabe se construiu alma ou fachada.

---

## Fase 0 — Mandato + Viabilidade

**Objetivo:** decidir *se* vale um agente e definir o que ele é.

**Filtro de viabilidade:** nem todo problema pede agente. Vale quando há **decisão complexa, regras difíceis de manter ou dependência de dado não estruturado**. *Não* vale quando as entradas são previsíveis (use fluxo determinístico), não há necessidade de planejamento, ou a tolerância da organização à autonomia é baixa.

**Mandato:**
- **Frase (1 linha):** "Este agente existe para ___, atende ___, e não faz ___."
- **Escopo duro:** 3 coisas dentro, 3 fora.
- **Tipo de instância:** dedicado (memória por-entidade) ou compartilhado.
- **Critério de "tarefa concluída", condições de falha e regras de escalonamento.**
- **Métrica de sucesso:** 1 métrica dura. Comece por caso de **alto impacto, baixo risco.**

**Entregável:** Ficha de Mandato (½ página).
**Pronto quando:** alguém diz, sem ajuda, o que o agente recusaria — e por que não foi descartado no filtro.
**Erro comum:** começar pela voz antes do mandato. Voz sem mandato é maquiagem.

---

## A — Alma

**Objetivo:** o núcleo de critério invariante. (Camada mais profunda do método — o que separa um agente com alma de um chatbot com nome.)

1. **Hierarquia de valores ordenada** — não lista plana. Ex.: `veracidade > utilidade > concisão > simpatia`. A ordem resolve o conflito sozinha.
2. **Linhas vermelhas (5–10)** — o que ele nunca faz, sob qualquer pedido. Ex.: nunca afirma o que não pode sustentar; nunca executa ação irreversível sem confirmação; nunca expõe dado sensível.
3. **Lente / visão de mundo (2–3 frases)** — como enxerga o domínio.
4. **Casos de julgamento (3–5)** — situação difícil → decisão → *porquê*. É o que faz o agente generalizar para o inédito.
5. **Postura sob pressão** — segura, explica, recusa. Não cede para agradar.

**Entregável:** `alma.md`.
**Pronto quando:** dois casos inéditos, resolvidos por pessoas diferentes, batem com o que a alma prevê.
**Erro comum:** lista plana de valores. Sem ordem, todo conflito vira improviso.

---

## T — Tom

**Objetivo:** *como* fala, não *quem* é.

- **Léxico:** 10–15 palavras-assinatura + lista de banidas.
- **Ritmo e forma:** frase curta vs. longa, uso de número, estrutura padrão de resposta.
- **Registro por canal:** matriz canal × formalidade.
- **Recursos retóricos:** instrumentos fixos.
- **8–12 pares antes/depois:** genérico → na voz. **Núcleo da camada.**

> **Calibração, não adjetivo.** "Tom premium, sofisticado" não calibra modelo — é briefing. Voz se calibra com par antes/depois + **teste cego** (um redator novo reescreve 3 textos; você não distingue do original).

**Entregável:** `tom.md`.
**Erro comum:** descrever voz só com adjetivos.

---

## L — Lembrança

**Objetivo:** o que faz o agente persistir, não só repetir. Memória (o que sei) ≠ estado (onde estou).

- **Curto prazo:** a conversa atual.
- **Persistente, por-entidade:** fatos duráveis; não mistura entidade A com B.
- **RAG:** conhecimento em base de documentos, recuperado sob demanda — em vez de empurrar tudo para o prompt.
- **Estado explícito, separado da memória:** progresso, resultados de ferramenta, tentativas, handoffs em estrutura própria. Habilita retentativa, observabilidade e coordenação.
- **Política de dados:** o que entra (fato durável) vs. ruído; sensível (credencial, pagamento) nunca em texto puro; regras de esquecimento e correção.

**Entregável:** `lembranca.md`.
**Pronto quando:** referencia certo um fato anterior, ignora ruído e retoma de um estado interrompido.
**Erro comum:** memória global que mistura entidades — vaza contexto e quebra confiança.

---

## A — Aptidões

**Objetivo:** o que ele faz, de forma plugável. Uma skill = uma intenção; skill nunca mora na alma.

### Modelo
- Linha de base com o modelo **mais capaz**; depois teste **menores** para custo/latência. Modelo por etapa.

### Habilidades
- **Uma skill por arquivo** (`SKILL.md`). Cada skill = gatilho + procedimento + restrições + ferramentas.

### Ferramentas
- **Tipologia:** dados (consulta) · ação (escreve/atualiza/envia) · orquestração (chama outro agente).
- **Schema explícito de entrada/saída** com validação. O modelo *raciocina*; a função *executa*.
- **Idempotência:** ação segura para repetição ou que detecta duplicata.
- **Governança:** permissões, timeouts, retries, fallback.
- **Menor privilégio + nível de risco** (baixo/médio/alto) → o risco decide quando exigir humano no loop.
- **Observabilidade e versionamento** de chamadas.

**Entregável:** pasta `/skills` + catálogo de ferramentas + roteador documentado.
**Pronto quando:** você adiciona uma skill nova sem editar a alma nem o tom.
**Erro comum:** skill-monstro que faz tudo. Quebra teste e manutenção.

```markdown
# SKILL — [nome]
## Gatilho (quando usar)
## Procedimento (passos)
## Restrições
## Ferramentas
- [nome] — tipo: dados/ação/orquestração | risco: baixo/médio/alto
  | entrada: {schema} | saída: {schema} | idempotente: sim/não
```

---

## S — Salvaguardas

**Objetivo:** provar que a alma segura — e mantê-la segurando em produção.

### Guardrails em camadas
Nenhuma proteção isolada basta; combine: classificador de relevância e segurança · filtro de PII · moderação · salvaguarda de ferramenta · regra fixa · validação de saída.

### Bateria de deriva
Testes adversariais que tentam fazer o agente trair cada linha vermelha e cada valor (pressão, reframe, autoridade falsa, apelo emocional, jailbreak). Roda antes de todo deploy.

### Ciclo de vida + observabilidade + métricas
Loop **desenhar → testar → implantar → monitorar → otimizar**. Observabilidade de primeira classe (prompts, modelo, ferramentas, estado, latência, custo). Métricas: conclusão · qualidade · escalonamento · custo de token · tempo de resposta. Versione prompts e avalie por traces + conjuntos de avaliação.

**Entregável:** `salvaguardas.md` + suíte de testes + checklist de deploy.
**Pronto quando:** 100% das linhas vermelhas resistem à bateria adversarial **e** a observabilidade está de pé antes do deploy.
**Erro comum:** validar só o caminho feliz e deixar observabilidade para depois.

---

## Quando um agente vira muitos — Orquestração

- **Comece com um agente.** Cobre muita coisa adicionando ferramentas aos poucos.
- **Vire multi quando** especialização ou paralelismo melhora qualidade, velocidade ou manutenção. Especialistas com papel limitado são mais confiáveis.
- **Decomponha como microserviços:** especialistas sob um orquestrador. Trocar um componente não reescreve o resto.
- **Multimodal nativo**, não add-on, quando há imagem/áudio.
- **Padrões abertos (MCP, A2A)** para troca segura e independência de fornecedor.

---

## Governança — para operar em escala

- **Constituição-mãe + herança:** DNA compartilhado que todos herdam; cada agente documenta só as diferenças. Muda a mãe → propaga.
- **Versionamento por camada:** cada deploy registra qual versão de cada camada subiu.
- **Ficha do Agente (1 página):** mandato, versões, métrica, dono.
- **KPIs e prontidão:** avalie dados, governança e recursos técnicos antes de escalar.
- **Comitê de ética e gestão de risco:** hierarquia de decisão e protocolo de escalonamento desde o início.
- **Modularidade para o futuro:** construa para trocar peça sem reescrever tudo.

---

## Ordem de execução

```
Fase 0 Mandato + Viabilidade
   → A  Alma
   → T  Tom
   → L  Lembrança (memória + estado)
   → A  Aptidões (habilidades + ferramentas + modelo)
   → S  Salvaguardas (guardrails + validação)
   → Orquestração (só quando a complexidade exigir)
   → Governança (loop contínuo)
```

A ordem é a metodologia: sem mandato, a alma flutua; sem alma, o tom é maquiagem; sem lembrança, nada persiste; sem aptidões, não há ação; sem salvaguardas, você não sabe se construiu alma ou fachada.

---

## Nota de versão

Renomeado de **CASCO v2.0** para **ATLAS** — mesma metodologia, nome com cara de framework e não de barco. Mapeamento: Alma (era Constituição) · Tom (era Articulação) · Lembrança (era Continuidade) · Aptidões (era Sistemas) · Salvaguardas (era Operação). Única mudança de ordem: Lembrança passou à frente de Aptidões, para casar com o acrônimo e porque memória é substrato do que as habilidades usam.
