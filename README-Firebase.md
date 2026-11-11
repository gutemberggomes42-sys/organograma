# Configuração Firebase — Organograma Usina Cambuí

O site usa Firebase Authentication (e-mail/senha) e Cloud Firestore. O SDK é carregado pelo CDN oficial do Firebase. Não abra `index.html` com `file://`: módulos ES e autenticação precisam de uma origem HTTP/HTTPS.

## Preparar o projeto

1. No Firebase Console do projeto `organograma-43954`, crie um banco Cloud Firestore em modo de produção.
2. Em **Authentication → Sign-in method**, habilite **E-mail/senha** e crie as contas da equipe autorizada. O app não permite cadastro público.
3. Publique as regras deste repositório (`firestore.rules`). Elas negam acesso por padrão e autorizam somente UIDs que tenham documento em `admins`.
4. Em **Authentication → Users**, copie o UID da conta que será administradora. No Firestore Console, crie o documento `admins/{UID}` (por exemplo, `admins/UID_COPIADO`) com o campo `role` igual a `admin`. Faça isso para cada usuário autorizado. As regras impedem que a própria aplicação crie ou altere essa lista.
5. Em **Authentication → Settings → Authorized domains**, confirme que o domínio de Hosting está autorizado. Instale o Firebase CLI e execute neste diretório: `firebase deploy --only firestore:rules,hosting`.
6. Acesse o endereço de Hosting publicado e entre com uma conta criada no passo 2 e autorizada no passo 4.
7. Na seção **Cadastro de colaboradores**, clique em **Importar base** e selecione o arquivo local `colaboradores-import.json`. A lista de ativos é gravada em Firestore e não é publicada no Hosting.

## Persistência e sincronização

- A estrutura do organograma é salva no documento `appData/organization`.
- Colaboradores adicionados manualmente ao cadastro são salvos em `appData/roster`; a lista importada da planilha é armazenada no documento protegido `appData/roster` e não é embutida no HTML público.
- Alterações recebidas de outros usuários aparecem em tempo real. O armazenamento local continua como cópia de contingência neste navegador.
- Use apenas contas internas autorizadas: o cadastro contém dados pessoais de colaboradores.

A configuração web do Firebase no HTML identifica o projeto, mas não concede acesso ao Firestore. As regras e a lista de administradores precisam ser configuradas no console conforme acima.
