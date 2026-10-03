# Achilles · Gestão Agropecuária

**Gestão de rebanho que substitui a planilha.** Aplicativo web instalável no celular, para pecuária de corte e cria: reprodução com protocolos de IATF, regra de descarte, partos com genealogia, sanitário com carência, pesagem com ganho diário, compras de lotes e leitura do brinco e de folhas de campo por inteligência artificial.

Demonstração: **agropecuaria.achillesmedia.com.br**. Um produto Achilles, sistemas agropecuários.

---

## Para quem é

Fazendas que ainda controlam o rebanho em planilha e caderno. O operador usa o celular no meio do pasto; o dono confia no dado, porque cada registro guarda quem o fez.

## O que o sistema faz

**Painel e agenda.** Indicadores do rebanho, taxa de prenhez, ganho de peso e alertas do que precisa de ação. A agenda se monta sozinha a partir dos registros: etapas de protocolo, diagnósticos, partos previstos e reforços de vacina.

**Reprodução.** Protocolos de IATF configuráveis, quadro de matrizes por fase e etapas concluídas em lote. **Regra de descarte:** ao atingir o limite de tentativas sem prenhez definido pela fazenda (padrão: 4), o sistema propõe enviar a matriz para abate, com confirmação e registro do motivo. O parto cria o bezerro já ligado à mãe e ao touro ou sêmen.

**Rebanho e compras.** Categoria por idade, origem estruturada (nascido aponta para a mãe; comprado aponta para o lote de compra), cadastro em lote por faixa de brincos, compras com fornecedor, GTA e custo por cabeça.

**Sanitário e pesagem.** Vacinas e medicamentos com reforço automático e bloqueio de abate durante a carência. Pesagem em modo curral com ganho médio diário na hora. Movimentação e saída em lote.

**Inteligência artificial.**
- *Leitura do brinco:* pela foto do animal, a IA sugere número, pelagem e raça, cada um com nível de confiança. Nada é salvo sem confirmação.
- *Leitura de folhas de campo:* o sistema imprime as folhas de diagnóstico, pesagem e sanitário com os brincos já escritos; a equipe marca à caneta; a foto da folha é lida linha por linha e conferida antes de virar registro.

**Documentos e equipe.** Fichas e relatórios em PDF e Word, relatório reprodutivo, CSV, contas por perfil (administrador e operador) e histórico de quem registrou o quê.

## Modo demonstração

Sem banco configurado, o sistema abre com um rebanho de exemplo completo, guardado no navegador de cada visitante. Ninguém interfere nos dados de ninguém, e o botão **Reiniciar demo**, no rodapé do menu, volta tudo ao início.

As leituras por IA funcionam de verdade na demonstração quando a variável `GEMINI_API_KEY` está cadastrada no Netlify. Sem ela, o sistema mostra uma leitura simulada, sempre identificada como tal.

## Publicação (Netlify)

- Build: `npm run build`, pasta `dist` (já configurado no `netlify.toml`).
- Funções de servidor em `netlify/functions`: leitura do brinco, leitura de folha e gestão de contas. As chaves ficam nas variáveis de ambiente do site, nunca no navegador.

| Variável | Para quê |
|---|---|
| `GEMINI_API_KEY` | Leituras por IA reais. Opcional na demonstração. |
| `GEMINI_MODEL` | Opcional, troca o modelo do Gemini sem mexer no código. |

## Implantando para um cliente

1. Crie um projeto no Supabase e execute `supabase/schema.sql` no SQL Editor.
2. Crie o primeiro usuário e promova a administrador (comando no fim do `schema.sql`).
3. No site do cliente no Netlify, cadastre:

| Variável | Valor |
|---|---|
| `VITE_SUPABASE_URL` / `VITE_SUPABASE_ANON_KEY` | Projeto do cliente |
| `SUPABASE_SERVICE_ROLE_KEY` | Gestão de contas pela tela |
| `VITE_BRAND=fazenda` e `VITE_FARM_NAME` | Nome e identidade da fazenda |
| `GEMINI_API_KEY` | Leituras por IA |

Com banco ligado, as funções de IA e de contas exigem login automaticamente.

## Rodando localmente

```bash
npm install
npm run dev
```

## Decisões técnicas

- **A IA sugere, a pessoa confirma.** Nada que o modelo interpreta é gravado sem revisão.
- **Estado reprodutivo calculado, não gravado.** A fase de cada matriz vem das tentativas registradas; não existe um campo que possa ficar desatualizado.
- **Rastreabilidade por gatilho no banco.** A autoria vem da sessão (`auth.uid()`), não do navegador.
- **Permissão no banco.** Políticas de RLS; o operador não se promove sozinho; as regras da fazenda só o administrador altera.
- **Sem dependências pesadas onde não precisa.** Leitor de planilha próprio, gráficos em SVG, bibliotecas de PDF e Word carregadas só quando usadas.

## Stack

`React 18` · `TypeScript` · `Vite` · `Supabase` · `Netlify Functions` · `Google Gemini` · `jsPDF` · `docx` · `PWA`

---

Achilles · Sistemas agropecuários.
