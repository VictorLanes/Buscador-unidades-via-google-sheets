# VeoLink – Painel de Unidades

Aplicação web para técnicos, supervisores e administradores da VeoLink controlarem unidades de clientes, consultar endereços e iniciar rotas rapidamente via Google Maps, Waze e Google Calendar. O aplicativo roda 100% no front‑end e sincroniza dados com o Google Sheets por meio de um endpoint Apps Script.

## Principais recursos

- **Autenticação local**: perfis `Administrador`, `Supervisor` e `Técnico` armazenados em `localStorage`, com fluxo de criação de usuários pelo admin.
- **Cadastro e busca de unidades**: formulário avançado para supervisores/admins registrarem nome, UF, endereço e observações. Técnicos visualizam as unidades com filtros e busca em tempo real.
- **Prévia do mapa**: iframe com preview do Google Maps e botões de ação (Maps, Waze, copiar dados, adicionar à agenda).
- **Integração com Google Sheets**: consumo de API Apps Script (`GET` para listar, `POST` para criar/deletar), permitindo que o Google Sheets funcione como base de dados.
- **Registro de OS por técnico**: formulário rápido para associar número/descrição de OS à unidade selecionada. Histórico fica visível ao técnico e supervisores exportam CSV por técnico.
- **Responsividade**: layout adaptado para uso em tablets e smartphones, pronto para incorporar em um app híbrido (WebView/PWA).

## Estrutura do projeto

```
veolink/
├── index.html    # Front-end completo (HTML/CSS/JS)
└── README.md     # Este guia
```

## Preparação da planilha

1. Crie uma planilha no Google Sheets e renomeie a aba principal para `unidades`.
2. Cabeçalhos obrigatórios na linha 1: `id`, `name`, `state`, `address`, `notes`, `created_at`.
3. Opcional: crie uma aba `os_logs` para futuras sincronizações do histórico de OS caso deseje centralizar esse dado (atualmente está em `localStorage`).

## Apps Script (API)

1. No Google Sheets, vá em **Extensões ▸ Apps Script**.
2. Apague o conteúdo padrão e cole o script do endpoint (`doGet`, `doPost`, `create`, `delete`). O código usado no projeto está em `index.html` (seção `<script>`).
3. Publique a aplicação como web app:
   - **Implantar ▸ Implementar como aplicativo da web**
   - Executar como: você.
   - Quem tem acesso: qualquer pessoa.
4. Copie a URL final e substitua o valor de `const API_URL` em `index.html`.

## Como testar localmente

1. Abra `veolink/index.html` no navegador (duplo clique ou via servidor local).
2. Primeiro login: `admin / admin123`. Em seguida crie os usuários reais.
3. Cadastre algumas unidades e confirme se aparecem/atualizam na planilha.
4. Entre com um usuário técnico para validar:
   - Visualização das unidades e filtros.
   - Prévia do mapa e ações (Maps/Waze/cópia/agenda).
   - Registro de OS (preenche número, descrição e salva).
5. Entre como supervisor/admin para gerar o relatório CSV das OS.

## Personalização

- **Tema**: altere as variáveis CSS no topo do arquivo (`--bg`, `--accent`, etc.).
- **Fluxo de login corporativo**: substitua o storage local por API própria/SSO conforme necessidade.
- **Histórico de OS centralizado**: adicione métodos no Apps Script para gravar/leitura das OS (aba `os_logs`) e troque o `localStorage` pelos endpoints.

## Publicação no GitHub Pages

1. Crie um repositório e envie os arquivos (`index.html` e `README.md`).
2. Nas configurações do repositório, habilite **GitHub Pages** apontando para a branch principal.
3. Atualize a constante `API_URL` se for usar planilhas diferentes em produção vs. testes.

## Licença

Defina a licença conforme política da empresa (MIT, GPL, etc.) antes de publicar.
