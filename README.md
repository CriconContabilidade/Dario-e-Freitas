# Custas e Repasses — Dario & Freitas Advocacia

Ferramenta de página única (sem instalação, sem servidor) para o cliente
**DARIO & FREITAS ADVOCACIA** (CNPJ 43.791.590/0001-20), no mesmo padrão
das outras ferramentas do Hub (Baixas de Parcelas, Cadastro de Clientes e
Fornecedores, Conversões Cibele).

## Por quê

Escritório de advocacia que emite poucas notas fiscais. A maior parte do
extrato é: custas processuais pagas (às vezes reembolsadas meses depois
pelo mesmo valor) e valores recebidos da Justiça que precisam ser
repassados a um cliente final (também às vezes meses depois). Quando há
nota fiscal, dá baixa normalmente; quando não há, guarda como pendência
até aparecer o lançamento oposto do mesmo valor, em qualquer mês.

## Como usar

1. Abra `index.html` (local ou publicado no GitHub Pages).
2. Arraste o extrato SICOOB do mês (PDF) — a ferramenta já descarta saldo
   do dia, RDC automático e tarifas sozinha.
3. Se houver título em aberto no mês, arraste também Contas a Pagar e/ou
   Contas a Receber (PDF do Domínio).
4. Clique **Processar**. A ferramenta:
   - Baixa contra nota fiscal aberta primeiro (soma exata + janela de
     dias, mesmo algoritmo da Baixas de Parcelas — inclusive títulos
     "Parcial", usando o saldo residual).
   - Casa o que sobrar contra pendências já abertas (de qualquer mês
     anterior) por valor exato, sempre em direção oposta.
   - O que não casar com nada e parecer custas/repasse vira candidato a
     nova pendência — sugestão por palavra-chave, mas a confirmação é
     sempre manual (nunca decide sozinho).
5. Revise as tabelas, baixe os `.txt` de baixa se houver, e clique
   **Gravar decisões no banco de pendências**.

## Onde os dados ficam

- **Cadastro de CNPJ**: compartilhado com o resto do Hub (Firestore,
  projeto `conversoes-cibele`, collection `clientes`).
- **Pendências de custas/repasses**: guardadas num campo array
  (`pendencias_advocacia`) dentro do documento da empresa em
  `empresas_cricon/43791590000120` — **não** é uma collection nova, porque
  as regras de segurança do Firestore só liberam `clientes` e
  `empresas_cricon` (testado nesta sessão: qualquer collection ou
  subcollection nova dá "Missing or insufficient permissions").

## Publicar

Depois de revisar, publique pelo **GitHub Desktop** (não uso `git push`
por linha de comando). Sugestão de nome do repositório no GitHub:
`Dario-Freitas-Custas-Repasses`, org `CriconContabilidade`, com GitHub
Pages habilitado na branch principal.

## Origem

Existe também uma versão em Python (scripts de linha de comando) da mesma
lógica em `Meu Drive/GUILHERME/Dario e Freitas Advocacia/Ferramenta Custas
e Repasses/` — foi o protótipo que validou o algoritmo antes desta versão
web. Esta versão web é a atual para uso do dia a dia.
