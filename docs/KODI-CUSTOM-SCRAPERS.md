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
- A configuração de idioma do add-on upstream tem atualmente `English` como valor predefinido. Para cumprir a decisão do projeto, o pacote incluído no Kodi Custom terá de definir `Portuguese - Portugal` como predefinição, mantendo a opção alterável pelo utilizador.
- A versão do upstream não deve ser considerada validada apenas por declarar dependências mínimas: ainda é necessário compilar/instalar e testar no Kodi 21.3 Omega.

## Estratégia de integração

1. Manter o scraper TMDb de filmes fornecido com Kodi 21.3 Omega; a predefinição de idioma pt-PT já foi alterada na branch de desenvolvimento.
2. Integrar o TheTVDB v4 como add-on Kodi independente, a partir do repositório oficial do respetivo projeto, preservando a estrutura, licença GPL-3.0-or-later e dependências.
3. Fixar uma versão/commit do TheTVDB v4 no projeto, em vez de seguir automaticamente o ramo principal.
4. A análise do CMake 21.3 Omega confirmou que a lista de add-ons empacotados pelo mecanismo `find_addon_xml_in_files` é descoberta através de `addons/*/addon.xml.in`. O TheTVDB upstream fornece `addon.xml` (não `addon.xml.in`) e um add-on Python com subpastas; por isso, copiar apenas o diretório para `addons/` não garante que seja incluído na instalação nem que os recursos sejam instalados corretamente.
5. A integração deve usar o mecanismo de distribuição de add-ons Python adequado ao Kodi (repositório/package de add-ons ou etapa explícita de instalação no build), em vez de presumir que o CMake do core o inclui automaticamente.
6. No pacote do TheTVDB, definir `Portuguese - Portugal` como valor inicial, mantendo a opção alterável e sem sobrescrever preferências já existentes.
7. Definir TMDb para novas fontes de filmes e TheTVDB v4 para novas fontes de séries, sem alterar bibliotecas já existentes.
8. Manter a possibilidade de alterar fornecedor e idioma nas definições do Kodi.

## Critérios de aceitação

1. Uma pasta de filmes é identificada pelo TMDb e obtém título, ano, sinopse e artwork em pt-PT quando disponíveis.
2. Uma pasta de séries é identificada pelo TheTVDB v4 e obtém séries, temporadas e episódios em pt-PT quando disponíveis.
3. A ausência de metadados pt-PT não interrompe a identificação; aplica-se o fallback do fornecedor.
4. Os scrapers e dependências ficam disponíveis numa instalação limpa do Kodi Custom.
5. As bibliotecas existentes não são modificadas sem consentimento do utilizador.
6. A integração é testada em Android TV/TV Box e em Android móvel.

## Próximo passo

Implementar uma etapa explícita de distribuição/instalação do add-on Python TheTVDB v4 no build Android, incluindo todos os seus recursos e dependências, e validar a instalação em Kodi 21.3 Omega. Só depois definir o fornecedor por defeito para novas fontes de séries e testar pesquisa, temporadas, episódios e fallback de idioma.
