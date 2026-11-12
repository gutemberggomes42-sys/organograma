# Configuração Firebase — Organograma Usina Cambuí

O site abre diretamente no organograma, sem tela de login. O SDK do Firebase é carregado pelo CDN oficial. Não abra `index.html` com `file://` se quiser sincronização em nuvem: módulos ES precisam de uma origem HTTP/HTTPS.

## Preparar o projeto

1. No Firebase Console do projeto `organograma-43954`, crie um banco Cloud Firestore em modo de produção.
2. Para sincronização em nuvem, crie contas pelo Firebase Authentication e aprove cada UID como administrador. O uso sem login salva os dados apenas no armazenamento local do navegador.
3. Publique as regras deste repositório (`firestore.rules`). Elas negam acesso por padrão e autorizam somente UIDs que tenham documento em `admins`.
4. Em **Authentication → Users**, copie o UID de cada conta autorizada. No Firestore Console, crie um documento `admins/{UID}` (por exemplo, `admins/UID_COPIADO`) com o campo `role` igual a `admin`. Um novo usuário recebe o UID na tela após criar a conta; cadastros não aprovados continuam sem acesso ao banco. As regras impedem que a própria aplicação crie ou altere essa lista.
5. Em **Authentication → Settings → Authorized domains**, confirme que o domínio de Hosting está autorizado. Instale o Firebase CLI e execute neste diretório: `firebase deploy --only firestore:rules,hosting`.
6. Acesse o endereço de Hosting publicado. Uma conta previamente autenticada e aprovada será sincronizada automaticamente; sem uma sessão, o sistema funciona localmente neste navegador.
7. A lista atual de 497 colaboradores já está embutida na constante `bundledRoster` de `index.html`, para aparecer sem importação manual. O botão **Importar base** permite carregar outra lista neste navegador e, com sessão Firebase autorizada, salvá-la no Firestore.

## Persistência e sincronização

- A estrutura do organograma é salva no documento `appData/organization`.
- A lista inicial (`bundledRoster`) fica dentro de `index.html`. Se publicar este HTML em um repositório ou site público, nomes, matrículas, cargos, setores, locais e datas de admissão ficam visíveis a quem acessar o código ou a página.
- Colaboradores adicionados manualmente são salvos localmente e, com sessão aprovada, em `appData/roster`. Uma lista não vazia do Firestore substitui a lista inicial no navegador.
- Alterações recebidas de outros usuários aparecem em tempo real. O armazenamento local continua como cópia de contingência neste navegador.
- Use apenas contas internas autorizadas: o cadastro contém dados pessoais de colaboradores.

A configuração web do Firebase no HTML identifica o projeto, mas não concede acesso ao Firestore. As regras continuam exigindo uma conta autorizada. A lista incluída no HTML é pública quando o sistema é publicado; mantenha a base atualizada e publique apenas se isso for adequado para os dados de RH. Sem uma sessão Firebase aprovada, importações e cadastros ficam apenas neste navegador.
