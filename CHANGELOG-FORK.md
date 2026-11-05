# Alterações próprias do fork alexgauna/DFe.NET

O `master` deste fork é espelho do `upstream/master` (ZeusAutomacao/DFe.NET), somente fast-forward.
As alterações abaixo existem apenas na branch `compat/dotnet10` (e nas `release/*` derivadas dela).

Commit upstream de base: veja `.fork/upstream-base`.

## compat/dotnet10

Base: `ZeusAutomacao/DFe.NET@4551ef540215b707364abff27f03bf1eec62e6e3` (26/10/2026).

1. **Target `net10.0`** adicionado a: DFe.Classes, DFe.Utils, DFe.Wsdl, NFe.Classes, NFe.Utils,
   NFe.Wsdl.Standard, NFe.Servicos, NFe.Danfe.Html. Os targets originais foram mantidos.
2. **NFe.Servicos.csproj:** a condição que inclui `NFe.Wsdl.Standard` passa a considerar `net10.0`.
3. **DFe.Utils.csproj:** `System.Security.Cryptography.Xml 10.0.14` no `net10.0` (os demais targets
   seguem com 6.0.1). Remove os transitivos vulneráveis `System.Security.Cryptography.Pkcs 6.0.1` e
   `System.Formats.Asn1 6.0.0` (NU1903) no .NET 10.
4. **NFe.Danfe.Html.csproj:** referencia os projetos do repositório em vez do pacote
   `Zeus.Net.NFe.NFCe 2025.12.22.2042`.
5. **Testes** (NFe.Utils.Testes, NFe.Classes.Testes, DFe.Testes, DadosDeTestes) executados em `net10.0`.
6. **Empacotamento:** `NuGet/AlexGauna.DFe.NFe.NFCe` (`lib/net10.0`, dependências declaradas,
   LICENSE/NOTICE/CHANGELOG incluídos).
7. **CI próprio:** `.github/workflows/fork-ci.yml` (build e testes em Windows e Linux; publicação no
   GitHub Packages só por tag `v*`). O workflow herdado `DFe.NET_build.yml` não é usado neste fork.

Nenhuma regra fiscal, classe ou serialização foi alterada.

### Limitações conhecidas
- `NFe.Danfe.Html` compila em `net10.0` contra o código do fork, mas **não é empacotado**: depende de
  `NetBarcode 1.7.0` → `SixLabors.ImageSharp 2.x`, que tem vulnerabilidades altas sem correção na linha 2.x
  (fixado em 2.1.13, que reduz mas não elimina). A decisão fica para o spike de DANFE da plataforma.
- `DanfePdfClownChaveAlfanumericaTestes` falha no Linux (`System.Drawing.Common` não suportado fora
  do Windows). O CI Linux exclui esses testes; o PdfClown não faz parte dos pacotes deste fork.
