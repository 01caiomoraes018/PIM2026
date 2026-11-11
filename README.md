# PIM Burguer — PIM 2026

Projeto acadêmico de sistema de pedidos para hamburgueria, com site, aplicativo Android e API compartilhada.

## Organização

| Pasta | Conteúdo |
| --- | --- |
| `PIMBURGUER-web-api/PIMBURGUER_API_SIMPLES/API` | API ASP.NET Core 9, Entity Framework Core, autenticação JWT e migrations |
| `PIMBURGUER-web-api/PIMBURGUER_API_SIMPLES/WEB` | Site HTML, CSS e JavaScript, imagens e Bootstrap local |
| `PIMBURGUER-web-api/PIMBURGUER_API_SIMPLES/BANCO` | Estruturas PostgreSQL, consultas e bases iniciais sem cadastros de clientes |
| `PIM_Burguer_App/pim_burguer_app` | Aplicativo Flutter para Android, cache SQLite e testes existentes |
| `PIM_Burguer_Bancos` | Orientações sobre os scripts de banco publicados |

## Preparar os bancos

Use PostgreSQL com ferramentas compatíveis com os scripts do pacote, exportados pelo PostgreSQL 18. Crie dois bancos novos e vazios: `pimburguer_v2` e `pimburguer_mobile`.

Os scripts atuais estão em `PIMBURGUER-web-api/PIMBURGUER_API_SIMPLES/BANCO/MOBILE/DOCUMENTACAO_ATUALIZADA`:

- Para o banco web, execute `ESTRUTURA_WEB_ATUAL.sql` em `pimburguer_v2`. Esse arquivo cria a estrutura sem dados de clientes ou catálogo.
- Para o app, execute `PIM_BURGUER_MOBILE_INICIAL_ATUALIZADO.sql` em `pimburguer_mobile`. Essa base contém sete produtos iniciais e o histórico de migrations, sem usuários, sessões ou pedidos.
- `ESTRUTURA_MOBILE_ATUAL.sql` é uma alternativa que contém somente a estrutura. Execute apenas uma alternativa de criação em cada banco vazio.

Você pode executar os scripts pelo Query Tool do pgAdmin. O catálogo web pode ser cadastrado pelo painel administrativo após a configuração da API.

## Executar API e site

Instale o SDK .NET 9 compatível com o projeto e configure o PostgreSQL. Abra `PimBurguer.sln` no Visual Studio ou um terminal dentro de `PIMBURGUER-web-api/PIMBURGUER_API_SIMPLES/API`.

Configure as credenciais apenas no seu ambiente. Exemplo no PowerShell, substituindo os valores de exemplo:

```powershell
$env:ConnectionStrings__PimBurguer = 'Host=127.0.0.1;Port=5432;Database=pimburguer_v2;Username=postgres;Password=SUA_SENHA'
$env:ConnectionStrings__PimBurguerApp = 'Host=127.0.0.1;Port=5432;Database=pimburguer_mobile;Username=postgres;Password=SUA_SENHA'
$env:AdministradorLocal__Senha = 'SUA_SENHA_ADMIN'
dotnet restore
dotnet run --launch-profile http
```

O usuário local de desenvolvimento é `admin_pim`. A senha inicial vem da variável acima e só cria o administrador se ele ainda não existir. O site usa <http://localhost:5181> e o Swagger, <http://localhost:5181/swagger>. As rotas web ficam em `/api/...`; as do app, em `/mobile/api/...`.

Em Development, o projeto gera a chave JWT fora dos fontes. Para executar em outro ambiente, configure também `Jwt__Key`, HTTPS e os demais dados do servidor.

## Executar o aplicativo

Instale o Flutter compatível com o `pubspec.yaml` e o Android SDK. Com a API em execução, abra `PIM_Burguer_App/pim_burguer_app` no Android Studio ou execute nessa pasta:

```powershell
flutter pub get
flutter run
```

No emulador Android, o endereço padrão é `http://10.0.2.2:5181/mobile/`. Para um celular por USB:

```powershell
adb reverse tcp:5181 tcp:5181
flutter run --dart-define=API_BASE_URL=http://127.0.0.1:5181/mobile
```

Os testes existentes podem ser executados com `flutter test`; a análise do app, com `flutter analyze`.

## Sobre esta publicação

Os arquivos do projeto foram extraídos do ZIP `PIM FINAL.zip` fornecido pelo autor. Foram preservados os fontes, recursos e a organização do pacote. Arquivos gerados, caches de dependências, configurações locais, backups com dados de clientes e a senha local de administrador não foram publicados. As referências à senha nos guias usam um valor de exemplo.

Alguns guias originais descrevem caminhos específicos do computador e backups privados que não acompanham o GitHub. Para uma instalação nova, siga os scripts e configurações deste README.
