# Finanças da Família V3 — Firebase + Vercel

Aplicativo independente em HTML, CSS e JavaScript Vanilla (com Firebase SDK modular). Não utiliza Higgsfield nem localStorage para transações.

## Instalação local

1. Instale Node.js LTS.
2. Na pasta do projeto execute `npm install`.
3. Copie `.env.example` para `.env` e preencha os valores de **Firebase Console > Configurações do projeto > Seus apps > Web**. Esses identificadores são públicos; **nunca** insira uma chave privada de conta de serviço no front-end.
4. Em **Firebase Authentication > Sign-in method**, habilite **E-mail/senha**.
5. Crie o banco **Cloud Firestore** em modo de produção e cole `firestore.rules` na aba **Regras**, depois clique em **Publicar**. Não use regras abertas.
6. Em **Firebase Authentication > Settings > Authorized domains**, inclua o domínio `.vercel.app` atribuído ao projeto (e seu domínio próprio, se houver).
7. Execute `npm run dev` e abra a URL indicada no terminal.

## Vercel com GitHub

Suba os arquivos para um repositório GitHub (sem o `.env`). Na Vercel clique em **Add New > Project**, importe o repositório, selecione **Framework Preset: Vite**, **Build Command: npm run build**, **Output Directory: dist**. Em **Environment Variables**, adicione todas as `VITE_FIREBASE_*` do `.env.example` com os valores reais. Clique **Deploy**. Depois registre o domínio gerado nos domínios autorizados do Firebase Auth.

## Fluxo familiar

A primeira pessoa cria uma conta e depois uma família. A segunda cria sua própria conta e usa um convite gerado em **Configurações**. O convite expira em 24 horas e é de uso único. As duas contas ficam vinculadas ao mesmo ID de família e veem as mesmas transações. Cada usuário mantém seu tema individual.

## Recorrências

O formulário aceita Única, Diariamente, Semanalmente, Mensalmente ou Anualmente e até 120 ocorrências. O algoritmo preserva o dia quando possível, ajustando meses curtos para o último dia do mês, e usa `writeBatch` para criar documentos individuais em uma única operação. Cada ocorrência tem baixa independente. Alterar uma ocorrência não altera automaticamente as demais.

## Segurança e limitações

As regras impedem leituras de transações de famílias diferentes, validam campos e exigem autenticação. O convite é um segredo temporário; envie apenas à pessoa convidada. **A primeira implantação deve ser testada com contas de teste antes de inserir dados reais.** O código não foi validado contra um projeto Firebase real, pois faltam as credenciais e a configuração do seu ambiente. O controle de orçamento não é uma conexão bancária. Não existe migração automática do banco local anterior. Não inclua senhas ou dados bancários confidenciais em notas. Para uma implantação com alto grau de segurança, considere App Check, limites de convite via Cloud Functions e trilhas de auditoria.
