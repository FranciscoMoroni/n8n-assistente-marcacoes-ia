# Assistente de Marcações (n8n + Gemini)

Workflow em n8n que recebe pedidos de marcação via webhook, gere a conversa com o cliente, valida disponibilidade na agenda e regista o agendamento automaticamente.

## Como funciona

1. **Webhook (`POST /marcacoes`)** — recebe o JSON com o nome e a mensagem do cliente.
2. **AI Agent (Gemini)** — interpreta o pedido e aplica as regras da clínica (horário das 09h às 18h, segunda a sexta, e recolha obrigatória de hora e contacto telefónico antes de fechar).
3. **Simple Memory** — guarda o histórico da conversa associado ao nome do cliente para não perder o contexto entre mensagens.
4. **Integrações (Tools):**
   - **Google Calendar (leitura):** verifica os eventos já marcados para evitar sobreposições.
   - **Google Calendar (escrita):** cria o evento na agenda assim que o cliente confirma a hora e o telefone.
   - **Google Sheets:** guarda uma linha com `Nome`, `Telefone` e `Data/Hora` para histórico de contactos.
5. **Respond to Webhook** — devolve a resposta ao cliente em JSON.

## Testar via terminal

```bash
  curl -X POST https://sua-instancia.app.n8n.cloud/webhook/marcacoes \
  -H "Content-Type: application/json" \
  -d '{"nome": "Ana", "mensagem": "Queria marcar uma consulta de avaliação para sexta às 15h. O meu contacto é 912345678"}'
