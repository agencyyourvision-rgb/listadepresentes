# 🎁 Lista de Presentes

Um site para telemóvel onde cada pessoa da família escreve o que gostaria de ganhar. Todos podem ver a lista de todos, mas só o dono, com o seu PIN de 4 números, pode mexer na própria lista.

## O que a família vai ver

1. **Ecrã de entrada: "Quem é você?"** É a primeira coisa que aparece. A pessoa toca no seu nome e digita o PIN. Na primeira vez é **1234**, e o próprio ecrã avisa.
2. **Página inicial.** Depois de entrar, aparece o botão "Ver e editar a minha lista" e os cartões de toda a família. Na **primeira vez**, um guia rápido de 4 passos mostra onde está cada coisa e, no fim, sugere criar um PIN próprio. Na primeira visita à própria lista há mais um guia curto (botão +, tamanhos e voltar). Os guias podem ser saltados e revistos em Ajuda › "Ver o guia outra vez".
3. **A minha lista.** O botão grande **+** adiciona presentes, sem limite. Só o nome do presente é obrigatório; detalhes, cor e link da loja são opcionais.
4. **Tamanhos (opcional).** Roupa, calças, calçado e outras informações aparecem no topo da lista de cada pessoa.
5. **Apagar com "Desfazer".** O site pede confirmação e, depois de apagar, ainda dá 7 segundos para recuperar o presente.
6. **Ajuda** (sempre no canto de cima). Mostra como funciona, permite escolher o **tamanho da letra** (Normal, Grande ou Maior), explica como **guardar no ecrã principal** e o que fazer em **"Esqueci o meu PIN"**.
7. **"Não é a Ana? Toque aqui".** Serve para quando duas pessoas usam o mesmo telemóvel.
8. **Aviso quando o site é aberto dentro do WhatsApp, Instagram ou Facebook.** Explica como abrir no Safari ou no Chrome, para a pessoa não ter de entrar sempre de novo.

## Ficheiros

| Ficheiro | Para que serve |
|---|---|
| `index.html` | O site completo (é só uma página) |
| `icon.png`, `manifest.json` | Ícone 🎁 para quando a pessoa guarda o site no ecrã principal |
| `vercel.json` | Configuração da Vercel |
| `supabase/schema.sql` | Base de dados: tabelas, segurança, código de administração e nomes |

---

## Como pôr no ar

### 1. Supabase
1. Crie um projeto em supabase.com (região *West EU*).
2. Abra `supabase/schema.sql` e edite duas coisas:
   - **No fim do ficheiro:** os **nomes da família**. Se houver nomes repetidos, use o nome completo ou um apelido (por exemplo, "Tia Ana").
   - **Perto do início:** o **código de administração**. Troque `135790` por 6 números seus.
3. No Supabase, abra **SQL Editor › New query**, cole o ficheiro inteiro e carregue em **Run**.
4. Em **Project Settings › API**, copie o **Project URL** e a chave **anon** ou **publishable**.

### 2. Configurar o site
No início do `<script>` do `index.html`, preencha:

```js
SUPABASE_URL: 'https://xxxx.supabase.co',
SUPABASE_KEY: 'a-chave-anon-ou-publishable',
ADMIN_NOME: 'Kelly',              // aparece em "Esqueci o meu PIN"
ADMIN_WHATSAPP: '351912345678',   // opcional: cria o botão "Pedir no WhatsApp"
```

Não faz mal a chave anon ficar visível. As tabelas estão fechadas e tudo passa por funções que verificam o PIN.

### 3. Vercel
Corra `npx vercel --prod` dentro da pasta, ou importe a pasta pelo GitHub em vercel.com. É um site estático, não há nada para configurar.

### 4. Enviar à família
Mande o link no grupo. Na primeira vez, cada pessoa vê as boas-vindas e é guiada passo a passo.

---

## Administração: `seusite.vercel.app/#/admin`

Entre com o código de 6 números. Esta página **não aparece no site**; guarde o endereço só para si. Aí pode:
- **Repor PIN:** quem se esqueceu volta a ter o PIN 1234.
- **Acrescentar pessoa**, **mudar nome** e **remover pessoa**. Nomes repetidos são recusados.

**Mudar o código de administração** (no SQL Editor):
```sql
update admin_config set codigo_hash = extensions.crypt('NOVO6NUMEROS', extensions.gen_salt('bf')) where id = 1;
```

**Recomeçar no próximo ano** (apaga os presentes e mantém as pessoas): `delete from itens;`

> Supabase já configurado: projeto **lista-presentes**.
