# Especificação do MVP — Oráculo

Documento de referência único para o projeto. Cole-o no início de conversas com qualquer ferramenta de IA (Replit, Claude, Gemini) para manter todas alinhadas.

## 1. Objetivo do MVP

Validar se famílias usam um app que (a) sugere a lista de compras e (b) avisa até quando o estoque dura, e se a comparação de preços entre locais ajuda a decidir onde comprar.

**Hipóteses a validar**

- H1: pessoas voltam ao app para consultar a duração do estoque
- H2: a comparação de preços muda onde a pessoa compra
- H3: lojistas se interessam em aparecer nos resultados (testado depois, por entrevista)

## 2. Escopo

### Dentro do MVP

1. Cadastro e login
2. Cadastro da família: nº de pessoas, restrições alimentares
3. Catálogo básico de produtos (arroz, feijão, ovos, óleo, etc.)
4. Estoque: o usuário informa o que comprou e a quantidade
5. Cálculo de duração do estoque e alerta de "acaba em X dias"
6. Lista de compras sugerida (baseada no que está acabando e nas restrições)
7. Preços cadastrados manualmente por local, com comparação simples

### Fora do MVP (não implementar agora)

Scraping de encartes/sites, mapa e crowdsourcing, painel de lojistas, pagamentos e plano Pro, aba Farmácia e saúde, painel de despesas globais, exportação de PDF.

## 3. Regra de cálculo da duração

```
consumo_diario_item = consumo_por_pessoa_por_dia × nº_de_pessoas
dias_restantes      = quantidade_atual ÷ consumo_diario_item
data_estimada_fim   = hoje + dias_restantes
```

- O cálculo é **aritmético, sem IA**.
- O consumo por pessoa vem de um valor padrão por produto e pode ser ajustado pelo usuário.
- Casos de borda a tratar e testar: quantidade zero, consumo zero ou negativo, família com 0 pessoas, mudança de tamanho da família no meio do período.

## 4. Uso de IA

- A IA é usada **apenas** para sugerir a lista de compras/cardápio respeitando restrições alimentares.
- A resposta do modelo deve ser validada contra um formato fixo (JSON com schema) antes de aparecer na tela.
- Se a IA falhar, o app mostra uma lista baseada em regras simples (itens com estoque baixo).

## 5. Modelo de dados inicial

| Tabela | Campos principais |
|---|---|
| `users` | id, email, password_hash, created_at |
| `households` | id, user_id, nome, nº_pessoas |
| `dietary_restrictions` | id, household_id, tipo (ex.: low carb, zero açúcar) |
| `products` | id, nome, categoria, unidade, consumo_padrao_por_pessoa_dia |
| `stock_items` | id, household_id, product_id, quantidade, atualizado_em |
| `stores` | id, nome, bairro, tipo |
| `prices` | id, product_id, store_id, preco, data, registrado_por |
| `shopping_lists` | id, household_id, criada_em |
| `shopping_list_items` | id, list_id, product_id, quantidade, comprado |

Relações: um usuário tem uma ou mais famílias; uma família tem vários itens de estoque; cada preço liga um produto a uma loja.

## 6. Stack

- Node.js + TypeScript
- PostgreSQL
- Desenvolvimento no Replit; código sempre sincronizado com este repositório (branch `mvp`)
- Mantenha o código portátil: sem dependências exclusivas do Replit, para permitir migrar depois

## 7. Segurança (desde o início)

- Senhas com hash (Argon2 ou bcrypt); autenticação com JWT
- Validação de toda entrada do usuário; consultas parametrizadas
- Rate limiting nas rotas de login e de IA
- Segredos apenas em variáveis de ambiente; `.env` fora do Git
- Minimizar dados pessoais coletados (LGPD)

## 8. Testes mínimos do MVP

- Unitários: cálculo de duração e regras da lista de compras
- Integração: cadastro, login e fluxo de estoque com banco de teste
- 2–3 testes E2E nos fluxos principais (cadastro → estoque → alerta)

## 9. Critérios de sucesso da validação

Definir com números antes de lançar. Exemplo para preencher:

- [ ] ___ famílias de teste usando por ___ semanas
- [ ] ___ % voltam ao app mais de uma vez por semana
- [ ] ___ pessoas dizem que mudariam o local de compra por causa dos preços

## 10. Decisões registradas

| Data | Decisão |
|---|---|
| 2026-10 | MVP no Replit; produto final construído depois com apoio do Claude; Gemini como apoio de pesquisa |
| 2026-10 | Repositório no GitHub antes de iniciar o MVP |

## 11. Riscos

- Escopo crescer além do MVP
- Poucos preços cadastrados, deixando a comparação sem valor
- Custo de IA e de banco imprevisível no Replit: monitorar créditos
- Código gerado por agente sem revisão: revisar tudo antes de juntar ao `main`
