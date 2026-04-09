# User Flow Documentation

## 1. Visão geral
- **Descrição breve:** Fluxo de uso da plataforma **Pequi** que permite ao Agente Comunitário de Saúde (ACS) cadastrar, triagem e acompanhar contatos de pacientes diagnosticados com hanseníase, garantindo o follow‑up anual.
- **Objetivo do usuário:** Registrar um novo contato, executar triagem de sintomas, receber o score de risco e agendar/confirmar visita de acompanhamento.
- **Contexto de uso:** O ACS está em uma comunidade rural com acesso limitado à internet; ele usa um smartphone Android com a aplicação PWA da Pequi, normalmente em modo offline/online.

## 2. Persona do usuário
| Atributo | Detalhes |
|----------|----------|
| **Nome** | **Maria Silva**, 42 anos |
| **Objetivos** | <ul><li>Mapear todos os contatos de um paciente recém‑diagnosticado.</li><li>Priorizar visitas aos contatos de maior risco.</li><li>Cumprir o protocolo de acompanhamento anual exigido pelo SUS.</li></ul> |
| **Frustrações** | <ul><li>Memória falha – esquecer quem já foi visitado.</li><li>Agenda manual sobrecarregada.</li><li>Estigma impede que os contatos respondam a visitas presenciais.</li></ul> |
| **Comportamentos** | <ul><li>Prefere telas simples, com poucos campos.</li><li>Usa o smartphone em modo offline e sincroniza ao final do dia.</li><li>Confia em lembretes via WhatsApp, mas evita mensagens que mencionem “hanseníase”.</li></ul> |

## 3. Pontos de entrada
1. **Notificação de novo caso** – O ACS recebe, via WhatsApp da secretaria, a informação de que um paciente foi diagnosticado.
2. **Abertura do app Pequi** – Maria abre a aplicação já instalada em sua tela inicial.
3. **Tela inicial** – Botão “Cadastrar contato” destacado na home.

## 4. Fluxo passo a passo

### Passo 1: Selecionar “Cadastrar contato”
- **Ação do usuário:** toca no botão “+ Contato” na tela inicial.
- **Resposta do sistema:** abre formulário “Cadastro de contato” com campos: Código (gerado automaticamente), Idade, Grau de parentesco, Localização (opcional GPS), Observações.
- **Pensamento do usuário:** “Preciso registrar rápido, sem escrever nome completo.”
- **Pontos de dor:** Campo “Localização” pode falhar offline; uso de GPS consome bateria.
- **Oportunidades:** Preencher automaticamente o código com QR‑scan ou sugestão de “último paciente”.

### Passo 2: Preencher informações do contato
- **Ação:** Digita idade, escolhe parentesco (ex.: “filho”), adiciona observação “mora próximo à casa 12”.
- **Resposta:** Validação em tempo real – aviso se idade fora do intervalo (0‑120); campo de localização mostra “offline – inserir manualmente”.
- **Pensamento:** “Será que preciso colocar endereço exato? Não, só o bairro.”
- **Pontos de dor:** Incerteza sobre o nível de detalhe necessário.
- **Oportunidades:** Tooltip explicativo “Basta informar bairro ou ponto de referência”.

### Passo 3: Salvar contato
- **Ação:** Clica em “Salvar”.
- **Resposta:** Tela de carregamento → **“Contato salvo!”** com feedback vibratório. Dados armazenados localmente (IndexedDB) e enfileirados para sincronização.
- **Pensamento:** “Ótimo, agora preciso triá‑lo.”
- **Pontos de dor:** Nenhum imediato, mas se a conexão cair, o contato ainda fica apenas local.
- **Oportunidades:** Mostrar ícone de “pendente de sincronização” que desaparece ao final do dia.

### Passo 4: Iniciar triagem de sintomas
- **Ação:** Na lista de contatos, toca no recém‑criado → botão “Triagem”.
- **Resposta:** Questionário de 5 perguntas (ex.: “Manchas na pele?”, “Dormência?”, “Febre?”). Cada pergunta tem opções “Sim/Não/Não sei”.
- **Pensamento:** “Essas perguntas são parecidas com outras doenças de pele… será que estou respondendo certo?”
- **Pontos de dor:** Dúvida sobre a interpretação das perguntas; risco de respostas erradas.
- **Oportunidades:** Texto de ajuda ao lado de cada pergunta, exemplo de sintoma.

### Passo 5: Receber score de risco
- **Ação:** Finaliza a triagem e toca “Calcular risco”.
- **Resposta:** Barra de risco (Verde/Amarelo/Vermelho) + número 0‑10; mensagem “Alto risco – visite em até 7 dias”.
- **Pensamento:** “Preciso agendar essa visita logo.”
- **Pontos de dor:** Se o score for médio, o ACS pode hesitar se a visita é realmente necessária.
- **Oportunidades:** Sugestão automática: “Adicionar ao calendário” com opção de confirmar ou adiar.

### Passo 6: Agendar visita
- **Ação:** Toca “Agendar visita” → calendário interno (offline) → escolhe data dentro de 7 dias.
- **Resposta:** Confirmação “Visita agendada para 15/04 – lembrete será enviado 1 dia antes”.
- **Pensamento:** “E se eu precisar mudar depois?”
- **Pontos de dor:** Falta de sincronização imediata pode gerar conflitos de agenda.
- **Oportunidades:** Integração com Google Calendar ou agenda do dispositivo, com sincronização automática quando houver conexão.

### Passo 7: Enviar lembrete ao contato
- **Ação:** O sistema automaticamente agenda envio de mensagem via WhatsApp (ou SMS) 1 dia antes.
- **Resposta:** Mensagem “Olá, aqui é a Maria, agente da saúde. Gostaria de lembrar da triagem de saúde familiar – clique no link para confirmar a visita.”
- **Pensamento do contato:** (não visível ao ACS) pode ficar confuso se não reconhecer a origem da mensagem.
- **Pontos de dor:** O contato pode não responder ou não abrir o link.
- **Oportunidades:** Mensagem neutra, sem mencionar hanseníase; incluir opção “Responder SIM para confirmar”.

### Passo 8: Realizar visita e registrar resultado
- **Ação:** No dia agendado, o ACS visita e abre a app → botão “Registro de visita”.
- **Resposta:** Tela para marcar “Visita concluída”, campo para observações e foto (opcional).
- **Pensamento:** “Preciso registrar rapidamente e voltar ao ponto no mapa.”
- **Pontos de dor:** Conexão fraca impede upload da foto; campo de observação pode ser longo.
- **Oportunidades:** Salvar foto localmente e sincronizar depois; detectar texto de observação via voz (microfone).

### Passo 9: Fechar ciclo anual
- **Ação:** Após 12 meses, o sistema gera “Lembrete de follow‑up anual” automaticamente.
- **Resposta:** Notificação push “Contato X necessita de avaliação anual – agende visita.”
- **Pensamento:** “Preciso garantir que não deixarei nenhum contato para trás.”
- **Pontos de dor:** Acúmulo de lembretes pode gerar sobrecarga.
- **Oportunidades:** Priorizar lembretes por score de risco e por tempo desde último contato.

## 5. Pontos de decisão
| Nº | Momento | Opções | Consequência |
|----|---------|--------|--------------|
| 1 | **Triagem de sintomas** | Responder “Sim”/“Não”/“Não sei” | Define o score de risco. |
| 2 | **Agendar visita** | Data próxima (≤ 7 dias) / Data distante (> 7 dias) / Adiar | Impacta risco de perda de contato. |
| 3 | **Confirmar lembrete** (contato) | Responder “SIM” / Ignorar / Responder “NÃO” | Afeta taxa de comparecimento. |
| 4 | **Finalizar visita** | Registrar foto / Apenas texto / Não registrar | Afeta qualidade dos dados para o SUS. |
| 5 | **Receber reminder anual** | Agendar imediatamente / Postergar / Ignorar | Pode gerar **drop‑off** do ciclo. |

## 6. Resumo dos pontos de dor
1. Falha de GPS/offline ao inserir localização.
2. Incerteza na interpretação das perguntas de triagem.
3. Confusão do contato ao receber mensagens neutras.
4. Conectividade limitada para upload de fotos/observações.
5. Sobrecarga de lembretes quando muitos contatos acumulam pendências.

## 7. Oportunidades de melhoria
| Área | Ideia acionável |
|------|-----------------|
| **Entrada de localização** | Permitir “ponto de referência” manual + mapa offline (OpenStreetMap) que salva lat/long quando houver conexão. |
| **Triagem** | Incluir ícones ilustrativos e tooltip com exemplos de cada sintoma. |
| **Comunicação** | Mensagem personalizada com nome do ACS, usando linguagem neutra (“check‑up de saúde familiar”). |
| **Sincronização** | Queue de uploads automático que retenta até sucesso; indicador de “pendente” visível. |
| **Gestão de agenda** | Integração opcional com calendário nativo do Android e resolução de conflitos (ex.: “já existe visita marcada nesse horário”). |
| **Feedback de foto** | Compressão automática e preview local; opção “usar voz” para inserir observação rápida. |
| **Prioridade de lembretes** | Algoritmo que reordena lembretes por score + tempo decorrido, evitando sobrecarga. |

## 8. Caminhos alternativos
- **Erro ao salvar contato (sem internet):** O app salva localmente e exibe aviso “Aguardando conexão”. Quando a rede volta, sincroniza silenciosamente.
- **Contato recusa visita:** ACS pode marcar “Recusado” → o sistema gera próximo lembrete em 30 dias com mensagem diferente.
- **Score de risco baixo, mas ACS decide visitar:** Permite “Visita opcional” → registro de visita “não mandatória” que ainda gera dado para o SUS.
- **Falha ao enviar mensagem WhatsApp:** Botão “Reenviar agora” aparece; se falhar novamente, a mensagem fica na fila e tenta novamente a cada 15 min.

## 9. Métricas a acompanhar
| Métrica | Definição | Meta inicial |
|---------|-----------|--------------|
| **Taxa de cadastro de novos contatos** | % de pacientes diagnosticados que geram ao menos um contato cadastrado. | > 85 % |
| **Precisão da triagem** | % de triagens que resultam em score coerente com avaliação clínica posterior. | > 90 % |
| **Tempo médio até visita** | Dias entre triagem e visita efetiva. | ≤ 7 dias para risco alto |
| **Taxa de entrega de lembretes** | % de mensagens WhatsApp/SMS entregues com sucesso. | > 95 % |
| **Taxa de resposta ao lembrete** | % de contatos que confirmam visita após receber mensagem. | > 70 % |
| **Conformidade anual** | % de contatos que recebem acompanhamento anual. | > 90 % |
| **Uso offline** | % de sessões que ocorrem sem conexão e ainda assim são sincronizadas. | > 80 % |

## 10. Visualização do fluxo (mermaid)
```mermaid
flowchart TD
    A[Notificação de novo caso] --> B[Abrir app Pequi]
    B --> C[Selecionar "Cadastrar contato"]
    C --> D[Preencher formulário]
    D --> E[Salvar contato]
    E --> F[Iniciar triagem]
    F --> G[Responder perguntas]
    G --> H[Calcular score de risco]
    H --> I{Score = Alto?}
    I -- Sim --> J[Agendar visita (≤7 dias)]
    I -- Não --> K[Visita opcional / adiar]
    J --> L[Envio automático de lembrete]
    K --> L
    L --> M[Visita realizada]
    M --> N[Registrar resultado/foto]
    N --> O[Sincronizar dados]
    O --> P[Lembrete anual (12 meses)]
    P --> Q[Repetir ciclo]
```
