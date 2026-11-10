# Configuração do Baravilhosas

Passo a passo para colocar o app no ar. Leva uns 20 minutos e só precisa ser feito uma vez.

## 1. Firebase (banco de dados e senhas)

1. Acesse [console.firebase.google.com](https://console.firebase.google.com) com uma conta Google e clique em **Adicionar projeto**. Nome: `baravilhosas`. O Google Analytics pode ficar desligado.
2. **Login por senha**: menu **Authentication** > **Primeiros passos** > **E-mail/senha** > ativar > Salvar.
3. **Contas**: em **Authentication** > **Usuários** > **Adicionar usuário**, crie:
   - `leitura@baravilhosas.app` com a senha de visualização;
   - `editora@baravilhosas.app` com a senha de edição.

   Os e-mails não precisam existir. O app só pede a senha.
4. **Proteção contra excesso de cadastros**: em **Authentication** > **Configurações** > **Ações do usuário**, desmarque **Ativar criação (inscrição)**. Assim ninguém consegue criar outras contas.
5. **Banco**: menu **Firestore Database** > **Criar banco de dados** > local `southamerica-east1 (São Paulo)` > **modo de produção**.
6. **Regras**: em **Firestore Database** > **Regras**, apague o conteúdo, cole o arquivo [`firebase/firestore.rules`](../firebase/firestore.rules) e clique em **Publicar**.
7. **App web**: em **Configurações do projeto** (engrenagem) > **Seus apps**, clique no ícone `</>`, dê o nome "Baravilhosas" e registre. Copie o bloco `firebaseConfig`.
8. No `index.html`, troque a linha `const FIREBASE_CONFIG = null;` pelo bloco copiado, por exemplo:

   ```js
   const FIREBASE_CONFIG = {
     apiKey: "...",
     authDomain: "baravilhosas.firebaseapp.com",
     projectId: "baravilhosas",
     storageBucket: "...",
     messagingSenderId: "...",
     appId: "..."
   };
   ```

   Esses dados não são secretos. Quem protege os dados são as regras do passo 6.

## 2. GitHub (hospedagem)

1. Crie um repositório (por exemplo, `baravilhosas`) e envie o conteúdo desta pasta.
2. Em **Settings** > **Pages**, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`, e salve.
3. Em 1 ou 2 minutos, o app fica disponível em `https://SEU-USUARIO.github.io/baravilhosas/`.
4. No Firebase, em **Authentication** > **Configurações** > **Domínios autorizados**, adicione `SEU-USUARIO.github.io`.

## 3. Primeiro acesso

1. Abra o endereço do app e entre com a senha de edição.
2. Na tela inicial, clique em **Importar histórico de 2026**. Os 9 encontros de fevereiro a outubro entram com os totais recalculados.
3. **Fase de teste**: a cliente pode criar encontros de teste, lançar súmulas, incluir convidadas e até testar "Iniciar novo ano".
4. Compartilhe com o grupo o endereço e a senha de visualização.

## Entrega definitiva (feita por nós)

Quando a cliente avisar que vai começar a usar para valer:

1. Abra o app com a senha de edição e acesse o endereço escondido `.../#/restaurar` (essa página não aparece em nenhum menu).
2. Clique em **Restaurar dados reais (até outubro)**. Tudo o que foi criado no teste é apagado, e 2026 volta aos 9 encontros reais.
3. No `index.html`, troque `const MODO_TESTE = true;` por `const MODO_TESTE = false;` e publique no GitHub. A página de restauração deixa de existir.
4. Avise a cliente: a partir daí, ela cria o encontro de novembro e lança os pontos de verdade.

## Uso no dia a dia

| Tarefa | Onde |
|---|---|
| Criar encontro | Encontros > Novo encontro |
| Lançar um jogo pela súmula | Abrir o encontro > Nova súmula |
| Lançar ou corrigir pontos | Abrir o encontro > Lançar pontos direto |
| Incluir convidada | Abrir o encontro > Dados do encontro |
| Imprimir ou mandar no WhatsApp | Botões Imprimir e Salvar imagem, no ranking geral ou no do dia |
| Virar o ano | Gerenciar > Iniciar novo ano |
| Backup | Gerenciar > Temporadas > Baixar backup |
| Trocar uma senha | Firebase > Authentication > Usuários > menu da conta > Redefinir senha |

## Testar sem Firebase

Com `FIREBASE_CONFIG = null`, o app abre em modo demonstração, com os dados de 2026 e as alterações guardadas só no navegador. Ele precisa ser aberto por um servidor, por exemplo:

```powershell
python -m http.server 8765
```

Depois acesse `http://127.0.0.1:8765/`.
