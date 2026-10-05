# Oráculo

Aplicativo de inteligência preditiva para economia doméstica e consumo. Responde a duas perguntas:

1. **O que e onde comprar?**
2. **Até quando meu dinheiro e minha comida vão durar?**

> Status: **MVP em desenvolvimento** (validação da ideia). O escopo completo está em [`docs/ESPECIFICACAO.md`](docs/ESPECIFICACAO.md).

## O que o MVP faz

- Cadastro da família (tamanho, restrições alimentares)
- Lista de compras sugerida
- Estimativa de duração do estoque ("5 kg de arroz duram 28 dias")
- Preços cadastrados manualmente, para comparar onde comprar

## Fora do MVP (próximas fases)

Scraping de encartes e sites, mapeamento colaborativo, painel de lojistas, plano Pro, aba Farmácia e alertas de orçamento.

## Stack (MVP)

- Node.js + TypeScript
- PostgreSQL
- Desenvolvimento no Replit, código versionado neste repositório

## Como rodar

```bash
cp .env.example .env   # preencha os valores localmente, nunca faça commit do .env
npm install
npm run dev
```

## Segurança

- Nunca versionar `.env`, chaves de API ou tokens
- Senhas sempre com hash; autenticação via JWT
- Dados reais de usuários não entram em ferramentas externas de IA

## Fluxo de trabalho

- `main`: versão estável
- `mvp`: desenvolvimento do MVP (revisar antes de juntar ao `main`)
- Commits pequenos, com prefixos `feat:`, `fix:`, `docs:`, `test:`
