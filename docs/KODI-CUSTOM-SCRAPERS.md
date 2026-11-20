# Kodi Custom — configuração inicial dos scrapers

Base de desenvolvimento: Kodi 21.3 Omega  
Branch: `custom-development`

## Decisões aprovadas

| Conteúdo | Scraper principal | Idioma preferencial |
|---|---|---|
| Filmes | The Movie Database (TMDb) Python | Português de Portugal (`pt-PT`) |
| Séries | The TVDB v4 | Português de Portugal (`pt-PT`) |

A identificação automática em português fica ativada. Quando o fornecedor não disponibilizar metadados em português de Portugal, deve ser aplicado um fallback controlado, preferencialmente inglês. Não se deve tratar português do Brasil como equivalente a português de Portugal.

## Estado da validação técnica

### Filmes — TMDb

- O Kodi 21.3 Omega já inclui o add-on oficial `metadata.themoviedb.org.python`.
- Versão encontrada no código-base 21.3 Omega: `1.3.1+matrix.1`.
- O scraper passa o idioma configurado à API do TMDb para pesquisa, detalhes e metadados.
- O valor predefinido de `language` foi alterado de `en-US` para `pt-PT` em `resources/settings.xml` na branch `custom-development`.
- A alteração define o idioma para instalações novas; as preferências já gravadas pelo utilizador não devem ser sobrescritas.
- Falta validar o comportamento em instalação limpa e com títulos que tenham variantes pt-PT e pt-BR.

### Séries — TheTVDB v4

- Identificador oficial: `metadata.tvshows.thetvdb.com.v4.python`.
- Repositório de origem: https://github.com/thetvdb/metadata.tvshows.thetvdb.com.v4.python
- Commit de origem analisado: `34d67a7dfe927ebf6d2a51f31c3464f217ae6f13`.
- Versão observada no `addon.xml` desse commit: `1.1.13`.
- Dependências declaradas: `xbmc.metadata 2.1.0` e `xbmc.python 3.0.0`.
- O add-on inclui a opção de idioma `Portuguese - Portugal` e recursos de idioma `pt_pt`.
- O add-on está fixado como submódulo Git no commit `34d67a7dfe927ebf6d2a51f31c3464f217ae6f13` (versão `1.1.13`).
- O ficheiro de definições upstream mantém-se intacto no submódulo; o projeto aplica um override de instalação em `overrides/thetvdb/resources/settings.xml` para definir `Portuguese - Portugal` como predefinição, mantendo a opção alterável.
- A versão do upstream não deve ser considerada validada apenas por declarar dependências mínimas: ainda é necessário compilar/instalar e testar no Kodi 21.3 Omega.

## Estratégia de integração

1. Manter o scraper TMDb de filmes fornecido com Kodi 21.3 Omega; a predefinição de idioma pt-PT já foi alterada na branch de desenvolvimento.
2. Integrar o TheTVDB v4 como add-on Kodi independente, através de submódulo Git apontado ao repositório oficial e fixado no commit `34d67a7dfe927ebf6d2a51f31c3464f217ae6f13`.
3. Incluir o caminho do submódulo em `cmake/installdata/common/addons.txt`, para os ficheiros do add-on serem copiados para os recursos instalados e incluídos na embalagem Android.
4. Aplicar o override pt-PT após os ficheiros originais do submódulo, através de `cmake/installdata/common/thetvdb-overrides.txt`. O original upstream não é modificado.
5. O CMake Kodi 21.3 identifica add-ons nativos com `addon.xml.in`, mas a lista de instalação de recursos (`addons.txt`) também permite incluir add-ons Python independentes com `addon.xml` normal.
6. A cópia do repositório tem de inicializar submódulos: `git clone --recurse-submodules https://github.com/porfirio292-svg/kodi-custom.git` ou, num clone existente, `git submodule update --init --recursive`.
7. Manter licença GPL-3.0-or-later, dependências e estrutura original do add-on.
8. Definir TMDb para novas fontes de filmes e TheTVDB v4 para novas fontes de séries, sem alterar bibliotecas já existentes; confirmar se a seleção automática de fornecedor pode ser aplicada sem alterar bibliotecas atuais.
9. Manter a possibilidade de alterar fornecedor e idioma nas definições do Kodi.

## Critérios de aceitação

1. Uma pasta de filmes é identificada pelo TMDb e obtém título, ano, sinopse e artwork em pt-PT quando disponíveis.
2. Uma pasta de séries é identificada pelo TheTVDB v4 e obtém séries, temporadas e episódios em pt-PT quando disponíveis.
3. A ausência de metadados pt-PT não interrompe a identificação; aplica-se o fallback do fornecedor.
4. Os scrapers e dependências ficam disponíveis numa instalação limpa do Kodi Custom.
5. As bibliotecas existentes não são modificadas sem consentimento do utilizador.
6. A integração é testada em Android TV/TV Box e em Android móvel.

## Próximo passo

Implementar uma etapa explícita de distribuição/instalação do add-on Python TheTVDB v4 no build Android, incluindo todos os seus recursos e dependências, e validar a instalação em Kodi 21.3 Omega. Só depois definir o fornecedor por defeito para novas fontes de séries e testar pesquisa, temporadas, episódios e fallback de idioma.
