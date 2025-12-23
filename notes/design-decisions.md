# Decisões de Design

### Orientação, não Agentes
O IAIP não define agentes para não ficar amarrado a APIs ou IDEs específicas. O foco é a natureza do projeto. Assim como um pintor pode usar pincel ou compressor para entregar o resultado, o IAIP foca na instrução (pintar a parede), deixando a execução para o agente.

### Soberania no Repositório
As regras devem viver no repositório para garantir **versionamento e histórico**. Prompts de chat se perdem, mas regras no código permitem auditar por que uma escolha foi feita ou quando uma instrução mudou.

### Exclusividade: Metadata vs Include
Decidimos que um `kind` usa ou `metadata` ou `include`, nunca ambos. Isso evita confusão de inferência e "regras fantasmas" que seriam difíceis de controlar e priorizar.

### Separação por Kind
Arquivos extensos são difíceis de manter e entender. A separação por tipos (context, guardrails, tasks) permite uma orientação focada e evita que a IA leia tudo de uma vez sem necessidade.

### Termo "Consumer"
Utilizamos "Consumer" para desvincular o protocolo da "moda" dos agentes. O consumo de instruções pode vir de qualquer lugar: IDEs, CLIs ou reuniões de governança.