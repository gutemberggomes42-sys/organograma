# Configuração Firebase — Organograma Usina Cambuí

O site abre diretamente no organograma, sem tela de login. O SDK do Firebase é carregado pelo CDN oficial. Não abra `index.html` com `file://` se quiser sincronização em nuvem: módulos ES precisam de uma origem HTTP/HTTPS.

## Preparar o projeto

1. No Firebase Console do projeto `organograma-43954`, crie um banco Cloud Firestore em modo de produção.
2. Para sincronização em nuvem, crie contas pelo Firebase Authentication e aprove cada UID como administrador. O uso sem login salva os dados apenas no armazenamento local do navegador.
3. Publique as regras deste repositório (`firestore.rules`). Elas negam acesso por padrão e autorizam somente UIDs que tenham documento em `admins`.
4. Em **Authentication → Users**, copie o UID de cada conta autorizada. No Firestore Console, crie um documento `admins/{UID}` (por exemplo, `admins/UID_COPIADO`) com o campo `role` igual a `admin`. Um novo usuário recebe o UID na tela após criar a conta; cadastros não aprovados continuam sem acesso ao banco. As regras impedem que a própria aplicação crie ou altere essa lista.
5. Em **Authentication → Settings → Authorized domains**, confirme que o domínio de Hosting está autorizado. Instale o Firebase CLI e execute neste diretório: `firebase deploy --only firestore:rules,hosting`.
6. Acesse o endereço de Hosting publicado. Uma conta previamente autenticada e aprovada será sincronizada automaticamente; sem uma sessão, o sistema funciona localmente neste navegador.
7. Na seção **Cadastro de colaboradores**, clique em **Importar base** e selecione o arquivo local `colaboradores-import.json`. A lista de ativos é gravada em Firestore e não é publicada no Hosting.

## Persistência e sincronização

- A estrutura do organograma é salva no documento `appData/organization`.
- Colaboradores adicionados manualmente ao cadastro são salvos em `appData/roster`; a lista importada da planilha é armazenada no documento protegido `appData/roster` e não é embutida no HTML público.
- Alterações recebidas de outros usuários aparecem em tempo real. O armazenamento local continua como cópia de contingência neste navegador.
- Use apenas contas internas autorizadas: o cadastro contém dados pessoais de colaboradores.

A configuração web do Firebase no HTML identifica o projeto, mas não concede acesso ao Firestore. As regras continuam exigindo uma conta autorizada. Para preservar os dados dos colaboradores, não publique regras que liberem leitura ou escrita anônima. Sem uma sessão Firebase, a tela de importação da base para Firestore fica indisponível e os cadastros funcionam apenas neste navegador.
