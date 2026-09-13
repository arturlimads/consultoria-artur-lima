# Consultoria Online — Personal Artur Lima

Sistema full-stack sem dependências externas, feito para Node.js 22.5+.

## O que já funciona
- Site público responsivo com identidade preto/branco/verde neon e logo AL.
- Anamnese pública salva no banco de dados.
- Primeiro acesso seguro para criação do administrador.
- Login com senha protegida por `scrypt`, sessão HTTP-only e perfis admin/aluno.
- CRM de alunos, leads e exportação CSV.
- Cadastro de alunos com plano, valor, vencimento e senha temporária.
- Cadastro e visualização de treinos e exercícios.
- Check-ins com peso, cintura, aderência, sono, energia, dor e observações.
- Fotos de evolução por aluno (JPEG/PNG/WebP, até 5 MB).
- Financeiro com cobranças, vencimentos e marcação manual de pagamento.
- Área individual do aluno.
- SQLite persistente (`data/consultoria.db`).

## Rodar no computador
1. Instale Node.js 22.5 ou superior.
2. Abra um terminal nesta pasta.
3. Execute `npm start`.
4. Abra `http://localhost:3000`.
5. Na primeira vez, acesse `http://localhost:3000/login.html` e crie sua conta de administrador.

Não é necessário `npm install`: o sistema utiliza apenas módulos nativos do Node.js.

## Produção
Para colocar em domínio próprio, use um servidor Node compatível (VPS, Railway, Render, Fly.io ou similar) e configure HTTPS. **SQLite exige disco persistente**. Para escala maior, migre a camada de dados para PostgreSQL.

### Variáveis recomendadas
- `PORT`: porta HTTP (padrão 3000)
- `NODE_ENV=production`

## Pagamento real
O sistema controla cobranças e status, mas **não armazena cartão e não gera Pix sozinho**. Isso é intencional. Para cobrança real, conecte um gateway (Mercado Pago, Stripe, PagSeguro etc.) no backend e use webhook para atualizar `payments.status` de forma confiável.

O endpoint `/api/student/checkout` atualmente retorna uma mensagem indicando que o gateway ainda não está configurado.

## Segurança antes de publicar
- Use HTTPS obrigatório.
- Faça backups do arquivo `data/consultoria.db` e da pasta `uploads/`.
- Adicione política de privacidade e termo de consentimento/LGPD apropriados à coleta de dados de saúde/anamnese.
- Prefira armazenamento privado/autorizado para fotos em produção, em vez de URL pública direta.
- Implemente recuperação de senha por e-mail antes de liberar para clientes reais.
- Considere rate limiting, CSRF e logs de auditoria para uma implantação pública.

## Estrutura
- `server.mjs`: servidor, autenticação e API
- `public/`: site, painel administrativo e área do aluno
- `data/`: banco SQLite criado na primeira execução
- `uploads/`: fotos enviadas pelos alunos
