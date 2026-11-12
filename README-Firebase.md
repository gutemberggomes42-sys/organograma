# Sincronização do Organograma — Firebase

O sistema sincroniza em tempo real quando é aberto por um endereço HTTP/HTTPS. Abrir `index.html` como `file://` salva apenas neste navegador e não conecta ao Firestore.

## Acesso compartilhado sem login

Por solicitação do proprietário, as regras em `firestore.rules` permitem leitura e gravação públicas apenas nos documentos `appData/organization` e `appData/roster`. Qualquer pessoa que tenha acesso ao app pode visualizar e alterar o organograma e a base de colaboradores. Não use este modo para dados que precisem de controle de acesso.

## Publicação

O projeto Firebase configurado é `organograma-43954`. Com o Firebase CLI instalado e autenticado, neste diretório execute:

```sh
firebase deploy --only firestore:rules,hosting
```

Depois, abra o endereço HTTPS do Firebase Hosting. O indicador no topo mostra se está sincronizado ou se está operando apenas localmente.

## Dados e sincronização

- Estrutura: `appData/organization`.
- Lista de colaboradores: `appData/roster`.
- Alterações são transmitidas aos navegadores conectados por listener em tempo real.
- Os 497 registros da lista inicial permanecem embutidos no código-fonte.
- A configuração do Firebase e a lista inicial estão visíveis no código publicado.
