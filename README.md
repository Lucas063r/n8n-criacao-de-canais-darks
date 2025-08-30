# 🎬 Sistema Criador de Vídeos Curtos 1.0 (n8n + Docker)

Este repositório contém um **workflow do n8n** que automatiza a criação de vídeos curtos narrados e legendados (TikTok, Reels, Shorts, etc).

Ele integra **LLMs (OpenAI/Groq)**, **TTS**, **together.ai (imagens)** e serviços auxiliares (**Baserow**, **FFmpeg**).

---

## ⚙️ Funcionalidades
- Input do usuário via **Form Trigger** (título, duração, estilo, voz, provedor de imagens).  
- Geração de **roteiro viral** com título, descrição, gancho, script e CTA.  
- Conversão do roteiro em **áudio narrado (TTS)**.  
- Criação de **legendas sincronizadas**.  
- Geração de **imagens com together.ai (Flux Schnell)**.  
- Montagem do vídeo final: **cenas + narração + legendas**.

---

## 📂 Estrutura
- `Sistema_Criador_De_V_deos_Curtos_de__Conte_do_.json` → Workflow exportado do n8n.  
- `docker-compose.yml` → Subida dos serviços auxiliares.  
- `README.md` → Documentação do projeto.  

---

## 🚀 Como rodar com Docker Compose

1. **Clone o repositório**
   ```bash
   git clone https://github.com/SEU_USUARIO/n8n-criador-videos.git
   cd n8n-criador-videos

2. Crie o arquivo .env

  N8N_PORT=5678
  N8N_BASIC_AUTH_ACTIVE=true
  N8N_BASIC_AUTH_USER=admin
  N8N_BASIC_AUTH_PASSWORD=senha123

  # Tokens de API
  OPENAI_API_KEY=sk-xxxxx
  GROQ_API_KEY=gsk-xxxxx
  TOGETHER_API_KEY=xxxxx

3. Suba os containers

  docker compose up -d


4. Acesse o n8n

  URL: http://localhost:5678

  Login: conforme definido no .env

5. Importe o workflow

  Vá em Workflows → Import from File

  Selecione Sistema_Criador_De_V_deos_Curtos_de__Conte_do_.json


## 🔒 Segurança

⚠️ O JSON original contém tokens de API hardcoded.

  Recomenda-se remover antes de publicar em repositórios públicos.

  Prefira usar variáveis de ambiente no .env.

## ✨ Próximos Passos

  Criar versão que usa apenas APIs cloud (sem dependências locais).

  Integrar upload automático em YouTube / TikTok API.

  Adicionar pipeline de CI/CD para importar/exportar workflows do n8n automaticamente.


---

## 📌 docker-compose.yml

```yaml
version: "3.9"
services:
  n8n:
    image: n8nio/n8n
    ports:
      - "${N8N_PORT}:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=${N8N_BASIC_AUTH_ACTIVE}
      - N8N_BASIC_AUTH_USER=${N8N_BASIC_AUTH_USER}
      - N8N_BASIC_AUTH_PASSWORD=${N8N_BASIC_AUTH_PASSWORD}
      - OPENAI_API_KEY=${OPENAI_API_KEY}
      - GROQ_API_KEY=${GROQ_API_KEY}
      - TOGETHER_API_KEY=${TOGETHER_API_KEY}
    volumes:
      - ./n8n_data:/home/node/.n8n
    restart: unless-stopped

  baserow:
    image: baserow/baserow:1.22.1
    ports:
      - "8081:80"
    environment:
      - BASEROW_PUBLIC_URL=http://localhost:8081
    volumes:
      - ./baserow_data:/baserow/data
    restart: unless-stopped

  ffmpeg:
    image: linuxserver/ffmpeg
    container_name: ffmpeg
    restart: unless-stopped















  
