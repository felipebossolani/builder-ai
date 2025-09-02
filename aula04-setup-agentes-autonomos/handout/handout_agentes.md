# Framework para Setup de Agentes Autônomos

## Estrutura
- **Role:** Quem o agente é? (ex: “assistente de onboarding”)
- **Objective:** Qual meta específica ele deve alcançar?
- **Guardrails:** O que ele *não pode* fazer?
- **Input/Output:** Quais dados recebe e qual formato deve devolver?

## Checklist Rápido
1. O papel está claro para todos?
2. O objetivo é mensurável e único?
3. Os guardrails cobrem riscos óbvios?
4. A entrada/saída é validada no fluxo?

## Exemplo
- **Role:** verificador de documentos
- **Objective:** classificar documento como válido/inválido
- **Guardrails:** nunca aprovar sem foto legível
- **Input:** imagem de documento → **Output:** status (válido/inválido + justificativa)
