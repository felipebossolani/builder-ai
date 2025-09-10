
# README — Workflows n8n (Builder AI – Aula 04)

Este repositório contém os workflows em JSON (V00 a V04) usados na aula 04.  
Os arquivos estão **higienizados** para facilitar importação em qualquer instância do n8n.

---

## 1) Workflows disponíveis

- [A04-P1-V0.json](A04-P1-V0.json)
- [A04-P1-V1.json](A04-P1-V1.json)
- [A04-P1-V2.json](A04-P1-V2.json)
- [A04-P1-V3.json](A04-P1-V3.json)
- [A04-P1-V4.json](A04-P1-V4.json)

### Higienização feita
- Removidos **Resource Locators (`__rl`)** nos nós de Google Sheets → uso de ID literal e nome de aba estável.
- Corrigidas expressões que referenciavam nós antigos (ex.: `Read Leads_Normalized_V2` → `Read Leads_Normalized_V4`).
- Ajustados nomes dos nós *Read* para alinharem com as abas corretas.

---

## 2) Subindo o n8n localmente (Docker)

O projeto inclui um `docker-compose.yaml`.  
Suba a stack com:

```bash
docker compose up -d
```

O n8n ficará disponível em [http://localhost:5678](http://localhost:5678).  
Dados persistem em `~/.n8n`. Para resetar o ambiente, remova/renomeie essa pasta (cuidado para não perder dados).

---

## 3) Importando os JSONs

1. Abra o n8n em `http://localhost:5678`.
2. Vá em **Workflows → Import**.
3. Escolha um dos arquivos higienizados.
4. Salve.

Após importar, é **obrigatório reconfigurar as credenciais** (Sheets e Gemini).

---

## 4) Configuração de credenciais

### Google Sheets
- Recomenda-se **Service Account** ou OAuth 2.0.
- Escopo: `https://www.googleapis.com/auth/spreadsheets`.
- Compartilhe a planilha com o e-mail da Service Account, se usar este método.

### Gemini (Google AI Studio)
- Configure credencial LangChain → Google Gemini.
- Insira a chave de API do Google AI Studio.
- Modelo: mantido conforme a versão (ex.: `gemini-1.5-flash`).  
  Pode padronizar manualmente todos os nós para o mesmo modelo, se preferir.

---

## 5) Teste rápido (Smoke Test)

Após importar e configurar credenciais:

1. Rode o workflow com 1 linha de teste.
2. Verifique:
   - Nó **Parse LLM JSON** produz campos esperados (`ts`, `email_norm`, `classificacao`).
   - **Switch** direciona corretamente (quente/morno/frio).
   - **Append/Update** grava nas abas certas (CRM, Frios, Log, Métricas).
   - Nó de **Follow Up Draft** gera texto se existir.

---

## 6) Boas práticas

1. Use **ID literal da planilha + nome da aba** (evite `__rl`).
2. Evite expressões dependentes do nome do nó (`$item(0,'Read ...')`); prefira `{{$json.campo}}`.
3. Não misture tipos de nó Gemini (base vs LangChain) no mesmo workflow.
4. Padronize o modo de escrita no Sheets (Append/Update com colunas **ou** range A:Z).
5. Versione planilhas/abas com sufixo de versão (`_V0..V4`) quando mudar schema.

---

## 7) Resolução de problemas

- **Planilha “sumiu” no nó** → Reassocie a credencial e confirme ID + nome da aba.  
- **Erro 403/404 no Sheets** → Compartilhe acesso com a conta da credencial.  
- **Campos vazios** → Verifique se `{{$json…}}` está pegando do input correto.  
- **Erro no Gemini** → Confira chave, modelo e cotas no AI Studio.

---

## 8) Backup

Todo o estado fica em `~/.n8n` (SQLite + credenciais).  
Para backup, copie essa pasta.  
Para restaurar, basta substituir e reiniciar os containers.

---

