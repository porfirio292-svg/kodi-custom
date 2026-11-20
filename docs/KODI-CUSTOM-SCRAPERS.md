# Kodi Custom — configuração inicial dos scrapers

Base de desenvolvimento: Kodi 21.3 Omega  
Branch: `custom-development`

## Decisões aprovadas

| Conteúdo | Scraper principal | Idioma preferencial |
|---|---|---|
| Filmes | The Movie Database (TMDb) Python | Português de Portugal (`pt-PT`) |
| Séries | The TVDB v4 | Português de Portugal (`pt-PT`) |

A identificação automática em português fica ativada. Quando o fornecedor não disponibilizar metadados em português de Portugal, deve ser aplicado um fallback controlado (preferencialmente inglês), sem substituir títulos originais quando a opção correspondente estiver ativa.

## Integração prevista

- Distribuir os scrapers como componentes Kodi pré-instalados e configurados, mantendo o mecanismo oficial de add-ons e as respetivas dependências.
- Filmes: utilizar o scraper oficial `metadata.themoviedb.org.python`.
- Séries: utilizar o scraper oficial TheTVDB v4 (`metadata.tvshows.thetvdb.com.v4.python`), após validar a compatibilidade da versão publicada com Kodi 21.3 Omega.
- Definir os fornecedores como opções predefinidas para novas bibliotecas/fontes, sem alterar automaticamente bibliotecas já existentes.
- Manter a possibilidade de o utilizador alterar manualmente o fornecedor e as opções de idioma.

## Nota técnica / validação necessária

O código-fonte Kodi 21.3 Omega já contém o scraper Python do TMDb para filmes. A árvore base contém também o scraper TMDb para séries, mas não inclui diretamente o add-on TheTVDB v4. Este último é distribuído no repositório oficial de add-ons do Kodi e terá de ser integrado/empacotado a partir da versão oficial compatível, respeitando dependências, licença e mecanismo de atualização.

A preferência `pt-PT` deve ser validada em ambos os scrapers. Se o fornecedor usar um código de idioma diferente ou não tiver conteúdo nessa variante, a configuração deverá recorrer ao fallback definido, sem assumir que português do Brasil é equivalente a português de Portugal.

## Critérios de aceitação

1. Uma pasta de filmes é identificada pelo TMDb e obtém título, ano, sinopse e artwork em pt-PT quando disponíveis.
2. Uma pasta de séries é identificada pelo TheTVDB v4 e obtém séries, temporadas e episódios em pt-PT quando disponíveis.
3. A ausência de metadados pt-PT não interrompe a identificação; aplica-se o fallback configurado.
4. Os scrapers e dependências ficam disponíveis numa instalação limpa do Kodi Custom.
5. As bibliotecas existentes não são modificadas sem consentimento do utilizador.
