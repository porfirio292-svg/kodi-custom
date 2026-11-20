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
- A escolha de idioma deve ser confirmada no pacote final e testada com títulos que tenham variantes pt-PT e pt-BR.

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

1. Manter o scraper TMDb de filmes fornecido com Kodi 21.3 Omega.
2. Integrar o TheTVDB v4 como add-on Kodi independente, a partir do repositório oficial do respetivo projeto, preservando a estrutura, licença GPL-3.0-or-later e dependências.
3. Fixar uma versão/commit do TheTVDB v4 no projeto, em vez de seguir automaticamente o ramo principal.
4. Preparar o pacote de distribuição para disponibilizar o scraper numa instalação limpa e aplicar `Portuguese - Portugal` como idioma predefinido.
5. Definir TMDb para novas fontes de filmes e TheTVDB v4 para novas fontes de séries, sem alterar bibliotecas já existentes.
6. Manter a possibilidade de alterar fornecedor e idioma nas definições do Kodi.

## Critérios de aceitação

1. Uma pasta de filmes é identificada pelo TMDb e obtém título, ano, sinopse e artwork em pt-PT quando disponíveis.
2. Uma pasta de séries é identificada pelo TheTVDB v4 e obtém séries, temporadas e episódios em pt-PT quando disponíveis.
3. A ausência de metadados pt-PT não interrompe a identificação; aplica-se o fallback do fornecedor.
4. Os scrapers e dependências ficam disponíveis numa instalação limpa do Kodi Custom.
5. As bibliotecas existentes não são modificadas sem consentimento do utilizador.
6. A integração é testada em Android TV/TV Box e em Android móvel.

## Próximo passo

Preparar o empacotamento do add-on TheTVDB v4 e a configuração inicial de idioma/fornecedor. Antes de o considerar integrado, validar instalação, dependências, pesquisa de séries, temporadas, episódios e fallback de idioma em Kodi 21.3 Omega.
