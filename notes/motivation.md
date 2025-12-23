# Motivação: Por que o IAIP?

O IAIP nasceu de uma frustração técnica real acumulada em mais de um ano de uso diário de IA.

### O Ponto de Ruptura
O problema central não é o modelo, mas o contexto. Percebi que, independentemente da IA utilizada (GPT, Gemini, Claude), o comportamento é o mesmo: o modelo inicia de forma coerente, mas alucina conforme a conversa se estende e as regras se perdem no acúmulo de tokens. 

### Refém da Plataforma
A dependência de prompts manuais gera um sentimento de "refém". Ao trocar de ferramenta ou IDE, o desenvolvedor é obrigado a reexplicar guardrails e contextos, o que é ineficiente e cansativo. Documentos Markdown comuns ajudam, mas não são tratados como obrigatórios pelos agentes.

### A Necessidade do Protocolo
O "estalo" para a formalização veio ao notar que regras implícitas em prompts são ignoradas em resumos de contexto. O IAIP torna a orientação **obrigatória**. A extensão `.iai` serve como um sinal claro para o assistente: "leia isto antes de continuar".