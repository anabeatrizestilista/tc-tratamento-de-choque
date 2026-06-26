# Guia: conectando a Ficha de Inscrição ao Google Forms

Este guia explica como ligar a página `inscricao.html` (já pronta e estilizada) a um Google Forms de verdade, para que as respostas caiam automaticamente numa planilha do Google Sheets — sem precisar contratar nenhum desenvolvedor ou serviço pago.

A ideia: o visitante preenche o formulário bonito do site. Por trás dos panos, o site manda essas respostas para o Google Forms, que salva tudo na planilha. O visitante nunca vê o Google Forms — só a sua tela elegante.

---

## Passo 1 — Criar o Google Form espelhando a ficha

Acesse forms.google.com e crie um formulário novo chamado, por exemplo, **"TC9 — Ficha de Inscrição (interno)"**.

Recrie exatamente os mesmos campos que já estão em `inscricao.html`, na mesma ordem (isso facilita não se perder depois). A lista completa de campos está nos comentários do arquivo, mas resumindo:

**Seção 1 — Dados pessoais:** nome completo, data de nascimento, idade, CPF, sexo, telefone, e-mail, endereço, cidade, igreja, nome do contato de emergência, telefone do contato de emergência, parentesco.

**Seção 2 — Acomodação:** peso, altura, precisa de cama de baixo (sim/não), motivo, cuidado especial no dormitório.

**Seção 3 — Saúde:** problema de saúde (sim/não) + detalhe, alergia (sim/não) + detalhe, medicação contínua (sim/não) + detalhe, medicação controlada (sim/não) + detalhe, acompanhamento especial, convênio.

**Seção 4 — Termos:** os 4 termos como perguntas de "caixa de seleção" (pode ser uma pergunta só com 4 itens, ou 4 perguntas separadas — o importante é que fiquem registradas).

> Não precisa caprichar no visual do Google Form em si — ele nunca será visto pelo público. O que importa é que os **nomes dos campos** e a **ordem** batam com o site.

---

## Passo 2 — Restringir o acesso (importante, por causa dos dados de saúde)

Como esta ficha pede dados sensíveis (saúde, medicação), configure o Google Form com cuidado:

1. No menu de configurações (⚙️) do Form, **não** marque a opção de coletar e-mails automaticamente se isso não for necessário.
2. Vá em **Respostas → planilha do Google Sheets** (ícone verde) para criar a planilha vinculada.
3. Na planilha gerada, clique em **Compartilhar** e dê acesso **apenas** às pessoas da coordenação do TC9 que realmente precisam ver esses dados (ex: você e mais 1 ou 2 pessoas de confiança da equipe de saúde/dormitórios). Nunca deixe como "qualquer pessoa com o link".
4. Considere criar uma cópia/exportação dessa planilha **sem** as colunas de saúde para quem só precisa dos dados de contato, mantendo a aba completa restrita a quem cuida da parte médica/dormitórios.

---

## Passo 3 — Descobrir os "entry IDs" de cada pergunta

Esse é o único passo técnico, mas é simples:

1. Abra o Google Form **como se fosse responder** (modo de visualização pública, não o modo de edição).
2. Preencha qualquer coisa em todos os campos (pode ser "teste").
3. Clique nos três pontinhos (⋮) no canto superior direito → **Copiar link**. Em alguns formulários, vá em "Pré-visualizar" e use a opção **"Obter link pré-preenchido"** (preencha o formulário de teste primeiro, depois peça o link pré-preenchido com essas respostas).
4. Esse link vai ter um monte de pedaços assim:
   `...&entry.123456789=teste&entry.987654321=teste...`
5. Cada `entry.NÚMERO` é o ID daquele campo específico — na mesma ordem em que você preencheu.

---

## Passo 4 — Preencher o arquivo `inscricao.html`

Abra `inscricao.html` em qualquer editor de texto e vá até o final, na tag `<script>`:

1. Troque `GOOGLE_FORM_ACTION` pela URL de envio do seu formulário. Para descobrir essa URL: na mesma tela de pré-visualização, veja o endereço — ele será parecido com:
   `https://docs.google.com/forms/d/e/1FAIpQLS.../viewform`
   Troque `/viewform` por `/formResponse` no final. Essa é a `GOOGLE_FORM_ACTION`.

2. No objeto `ENTRY_IDS`, troque cada `"ENTRY_ID_AQUI"` pelo número real (ex: `"entry.123456789"`) do campo correspondente que você anotou no Passo 3.

3. Salve o arquivo. Pronto — agora, quando alguém preencher e enviar a ficha no site, a resposta cai direto na sua planilha do Google Sheets.

---

## Alternativa mais simples (sem nenhuma configuração técnica)

Se preferir não mexer em código nenhum: troque o formulário em `inscricao.html` por um botão simples "Preencher minha ficha" que abre o link do Google Forms direto em uma nova aba. É menos elegante (a pessoa sai do visual do site por um momento), mas funciona imediatamente, sem nenhum passo técnico. Posso preparar essa versão também, se preferir começar por aqui e migrar para a versão integrada depois.

---

## Sobre a LGPD

Dados de saúde e medicação são considerados **dados sensíveis** pela LGPD (Lei Geral de Proteção de Dados). Por isso:

- O termo de consentimento já está incluído na ficha — não remova essa parte.
- Mantenha o acesso à planilha restrito (Passo 2).
- Defina, com a coordenação, por quanto tempo esses dados serão mantidos após o TC9 (ex: até 30 dias após o encerramento, depois excluídos), e comunique isso se alguém perguntar.
- Evite exportar ou compartilhar essa planilha por WhatsApp ou e-mail sem necessidade — prefira que as pessoas acessem direto pelo link restrito do Google Sheets.
