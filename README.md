# 🤖 Agente de IA com WhatsApp e N8N

Este projeto demonstra como criar e configurar um **Agente de Inteligência Artificial** integrado ao **WhatsApp**, rodando **localmente** de forma gratuita com **Docker**, **N8N** e a **API Google Gemini**.

O agente é capaz de manter conversas, lembrar o histórico e responder a perguntas com base em instruções personalizadas.

---

## 🧰 Pré-requisitos

- [Docker Desktop](https://www.docker.com/products/docker-desktop): É essencial que o Docker esteja instalado e funcionando na sua máquina.

---

## 🚀 Passo a Passo da Instalação

### 1. Preparação do Ambiente

1. **Baixe o `docker-conteiner.yml`**: (disponibilizado no repositorio)
2. **Crie a pasta do projeto**:
   ```bash
   mkdir n8n-waha-local
   mv docker-conteiner.yml n8n-waha-local/
   cd n8n-waha-local
   ```
3. **Suba os contêineres**:
   ```bash
   docker-conteiner up -d
   ```
   Isso iniciará os serviços:
   - N8N
   - WAHA (API não-oficial do WhatsApp)
   - Redis (memória de conversas)

---

### 2. Conectando o WAHA ao WhatsApp

1. No **Docker Desktop**, clique no link do contêiner WAHA para abrir o painel.
2. Acesse o **Dashboard** do WAHA.
3. Na sessão `default`, clique em **Start**.
4. Escaneie o **QR Code** com o seu WhatsApp (Menu > Aparelhos conectados).

---

### 3. Configurando o N8N

1. No Docker Desktop, clique no link do contêiner N8N para abrir o painel.
2. **Crie sua conta** com nome, e-mail e senha.
3. *(Opcional)* Ative funcionalidades extras com uma **Instance Key** obtida gratuitamente no site do [N8N](https://n8n.io).
4. Instale o nó da comunidade WAHA:
   - Vá em **Community Nodes > Install Community Node**
   - Procure por `n8n-nodes-waha`
   - Marque e clique em **Install**

---

### 4. Construção do Workflow no N8N

#### 🔗 Trigger: Webhook

1. Adicione um nó **Webhook** com:
   - Método: `POST`
   - Path: `webhook`
2. Copie a URL de teste e cole no **WAHA > Sessão > Webhooks**
   - Marque apenas o evento `message`
3. Clique em **Listen for test event** no N8N e envie uma mensagem para o WhatsApp conectado.

#### 🧹 Tratamento de Dados

- Adicione um nó **Set** e crie variáveis como:
  - `session`, `chatId`, `pushName`, `message` etc.

#### 🔄 Filtro de Evento

- Adicione um nó **Switch** para filtrar apenas mensagens do tipo `message`.

#### 🧠 Agente de IA

1. Adicione um nó **AI Agent**
2. No campo "Prompt", use a variável `message`
3. Adicione uma **System Message** com instruções (ex: *"Você é um guia turístico..."*)
4. Escolha **Google Gemini** como modelo
5. Crie uma credencial com sua **API Key** do [Google AI Studio](https://aistudio.google.com)
   - Use o modelo: `gemini-2.0-flash`

#### 💾 Memória de Conversa (Redis)

1. Adicione o nó **Redis Chat Memory**
2. Crie uma nova credencial:
   - Host: `host.docker.internal`
   - Password: `default`
3. Use o `chatId` como Session ID
4. Configure TTL (ex: `3600` segundos = 1 hora)

#### 📩 Envio de Resposta

1. Adicione nó WAHA: `Mark as Read`
2. Adicione outro nó WAHA: `Send Message`
   - No campo "Text", insira o resultado do nó "AI Agent"
   - Host URL: `host.docker.internal:3000`

---

### ✅ Ativação

Clique em **Activate** no canto superior direito do N8N.

Seu **Agente de IA com WhatsApp** está pronto para uso! 🎉

---

## 📄 Licença

Este projeto é open-source e pode ser utilizado e modificado livremente.

---

## 🙋‍♂️ Contribuições

Contribuições são bem-vindas! Sinta-se à vontade para enviar issues ou pull requests.


