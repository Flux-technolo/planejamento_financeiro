# App de Planejamento Financeiro do Casal

Aplicativo de controle financeiro mensal para casais, em português (pt-BR), com projeção de 12 meses e sincronização em tempo real entre dois celulares.

## Arquivos

- **`Planejamento Financeiro.dc.html`** — arquivo principal (edite este). Contém toda a interface, lógica e persistência.
- **`App Financeiro (standalone).html`** — versão única e autossuficiente, pronta para hospedar/compartilhar. Gerada a partir do arquivo principal. Não editar direto: reexporte quando o principal mudar.
- **`support.js`** — runtime do componente (gerado automaticamente, não editar).
- **`_ds/`** — NOW Labs Design System (tokens e componentes).

## Funcionalidades

### Lançamentos
- **Entradas e saídas** com valores no formato pt-BR (R$ 1.000,00).
- **Classificação automática** de saídas em fixa / variável / parcelamento, por palavras-chave (aluguel, luz, seguro, assinaturas etc.).
- **Salários sempre recorrentes** — detectados automaticamente e migrados ao carregar.
- **Recorrência** aplicável a entradas e saídas.
- **Parcelamentos** com controle de parcela atual (x/y) ao longo dos meses.
- **Por pessoa**: cada lançamento é de uma pessoa (A / B) ou do Casal, com filtro na aba Lançamentos.
- **Clonar** um lançamento para o mês seguinte.
- **Importação em massa** por texto: `dia; descrição; valor; parcela x/y`.

### Resumo
- Totais do mês (entradas, saídas, saldo).
- Composição das saídas por categoria com **detalhamento expansível**.
- Divisão por pessoa.
- **Exportar backup** dos dados em `.txt`.

### Contas do mês
- Contas fixas e parcelamentos com alternância pago/não pago.
- **Marcar todas como pagas** de uma vez.
- Próximos vencimentos mesmo sem data definida.

### Projeção (12 meses)
- Saldo mês a mês e **saldo acumulado** do período.
- **Meses com saldo negativo destacados em vermelho** com aviso.
- **Editar eventos** direto na visão de projeção.

## Persistência e sincronização

- **Local**: os dados são salvos no navegador (localStorage) — funciona offline.
- **Nuvem (opcional)**: sincronização em tempo real via **Supabase**, para os dois usarem de qualquer celular.

### Como sincronizar

1. Crie um projeto grátis em [supabase.com](https://supabase.com).
2. No **SQL Editor**, rode:

   ```sql
   create table if not exists couples (
     code text primary key,
     data jsonb,
     updated_at timestamptz default now()
   );
   alter table couples enable row level security;
   create policy "couple access" on couples
     for all using (true) with check (true);
   alter publication supabase_realtime add table couples;
   ```

3. No app, toque em **Sincronizar** (topo) e cole a **Project URL** e a **anon/publishable key** (em Settings → API).
4. Combinem o **mesmo código do casal** nos dois aparelhos e toquem em **Conectar**.

Quem conecta primeiro envia os lançamentos atuais; o outro os recebe. Depois disso, tudo edita junto em tempo real. O código do casal funciona como senha — escolham algo não óbvio.

## Como publicar

Hospede o **`App Financeiro (standalone).html`** em qualquer host estático grátis:

- **Netlify Drop** — [app.netlify.com/drop](https://app.netlify.com/drop), arraste o arquivo.
- **GitHub Pages** ou **Vercel**.

Depois abra o link no celular e use "Adicionar à tela de início" para virar um ícone de app.

## Stack

- Design Component (HTML + classe de lógica), NOW Labs Design System.
- `@supabase/supabase-js` para a sincronização em nuvem.
