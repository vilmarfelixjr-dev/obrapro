# ObraPro — Guia de Publicação na Nuvem

## O que está neste pacote

```
obrapro/
├── public/
│   └── index.html     ← O app completo
├── vercel.json        ← Configuração de hospedagem
└── README.md          ← Este guia
```

---

## PASSO A PASSO COMPLETO

### ETAPA 1 — Criar conta no GitHub (5 minutos)

1. Acesse https://github.com
2. Clique em "Sign up"
3. Preencha email, senha e username
4. Confirme o email

---

### ETAPA 2 — Criar repositório e subir o projeto (5 minutos)

1. Dentro do GitHub, clique no botão verde **"New"** ou **"+"** no topo
2. Nome do repositório: `obrapro`
3. Deixe como **Public**
4. Clique em **"Create repository"**
5. Na próxima tela, clique em **"uploading an existing file"**
6. Arraste a pasta `obrapro` inteira (ou os arquivos `index.html` e `vercel.json`)
7. Clique em **"Commit changes"**

---

### ETAPA 3 — Hospedar no Vercel (5 minutos)

1. Acesse https://vercel.com
2. Clique em **"Sign up"** → escolha **"Continue with GitHub"**
3. Autorize o Vercel a acessar seu GitHub
4. Clique em **"Add New Project"**
5. Encontre o repositório `obrapro` e clique em **"Import"**
6. Não mude nada, só clique em **"Deploy"**
7. Aguarde 1-2 minutos
8. **Pronto! Seu app está no ar** com um link tipo `obrapro.vercel.app`

---

### ETAPA 4 — Banco de dados com Supabase (15 minutos)

1. Acesse https://supabase.com
2. Clique em **"Start your project"** → faça login com GitHub
3. Clique em **"New project"**
4. Dê um nome: `obrapro`
5. Crie uma senha forte (guarde ela!)
6. Região: **South America (São Paulo)**
7. Clique em **"Create new project"** e aguarde 2 minutos

#### Criar as tabelas:

Dentro do Supabase, clique em **"SQL Editor"** e cole este código:

```sql
-- Empresas/Clientes
CREATE TABLE clientes (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  nome TEXT NOT NULL,
  cnpj TEXT,
  cidade TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Usuários
CREATE TABLE usuarios (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  nome TEXT NOT NULL,
  email TEXT UNIQUE NOT NULL,
  role TEXT NOT NULL DEFAULT 'user',
  empresa_id UUID REFERENCES clientes(id),
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Obras
CREATE TABLE obras (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  nome TEXT NOT NULL,
  responsavel TEXT,
  orcamento NUMERIC DEFAULT 0,
  gasto NUMERIC DEFAULT 0,
  data_inicio DATE,
  data_termino DATE,
  local TEXT,
  status TEXT DEFAULT 'Ativa',
  empresa_id UUID REFERENCES clientes(id),
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Pedidos
CREATE TABLE pedidos (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  obra_id UUID REFERENCES obras(id),
  descricao TEXT NOT NULL,
  categoria TEXT,
  fornecedor TEXT,
  quantidade NUMERIC DEFAULT 1,
  valor_unitario NUMERIC DEFAULT 0,
  status TEXT DEFAULT 'Pendente',
  solicitante TEXT,
  empresa_id UUID REFERENCES clientes(id),
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

Clique em **"Run"** para criar as tabelas.

#### Pegar as credenciais:

1. No menu lateral, clique em **"Settings"** → **"API"**
2. Copie a **"Project URL"** (começa com https://...)
3. Copie a **"anon public"** key

#### Conectar ao app:

Abra o arquivo `index.html`, encontre estas linhas no início do JavaScript:

```javascript
const SUPABASE_URL = 'COLE_SUA_URL_AQUI';
const SUPABASE_KEY = 'COLE_SUA_CHAVE_AQUI';
```

Substitua pelos seus valores e faça upload novamente no GitHub.
O Vercel atualiza automaticamente em 1-2 minutos.

---

### ETAPA 5 — Domínio próprio (opcional, R$40/ano)

1. Acesse https://registro.br
2. Pesquise o domínio desejado (ex: `obrapro.com.br`)
3. Compre e confirme o pagamento
4. No painel do Vercel, vá em **"Settings"** → **"Domains"**
5. Clique em **"Add"** e digite seu domínio
6. O Vercel mostrará um código DNS — copie ele
7. No Registro.br, vá em **"DNS"** do seu domínio e adicione o registro
8. Aguarde até 24h para propagar (geralmente menos de 1h)

---

## LOGINS DE DEMONSTRAÇÃO

| Perfil | Email | Senha |
|--------|-------|-------|
| Super Admin (Você) | vilmar@obrapro.com | admin123 |
| Admin Cliente | carlos@demo.com | cliente123 |
| Funcionário | ana@demo.com | func123 |

---

## SUPORTE

Em caso de dúvida em qualquer etapa, entre em contato.
Este sistema foi desenvolvido especialmente para Vilmar — ObraPro v1.0
