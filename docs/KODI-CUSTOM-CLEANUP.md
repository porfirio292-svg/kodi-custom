# Kodi Custom — plano de redução da distribuição

## Objetivo

Criar uma distribuição Android baseada no Kodi 21.3 Omega, centrada em música, filmes, séries e pesquisa/gestão de ficheiros, sem remover componentes necessários ao motor multimédia.

Este documento define o primeiro perímetro de limpeza. Não representa ainda uma APK alterada nem validação de compilação.

## Regras de implementação

- Trabalhar exclusivamente na branch `custom-development`.
- Não alterar a branch `master` nem remover código upstream.
- Preferir opções de configuração CMake e listas específicas de distribuição a apagar código-fonte.
- Excluir um componente apenas depois de confirmar que não é dependência de outro componente mantido.
- Manter os add-ons de metadados existentes nesta fase; a seleção TMDb para filmes e TheTVDB para séries permanece conforme as decisões registadas em `KODI-CUSTOM-SCRAPERS.md`.
- Não alterar os scrapers nem impor idioma por alterações ao seu código nesta fase.

## Componentes a manter

| Área | Requisito |
|---|---|
| Reprodução | Motor de áudio/vídeo, codecs, renderização, playlists e controlos OSD |
| Biblioteca | Música, filmes e séries, pesquisa e atualização das bibliotecas |
| Legendas | Internas e externas, seleção de faixa, sincronização e formatos suportados |
| Ficheiros | Navegação por pastas, gestor de ficheiros e pesquisa de ficheiros |
| Rede | VFS, SMB, NFS, credenciais, descoberta e acesso a partilhas |
| Interface | Skin, diálogos, definições necessárias, idiomas e artwork |
| Metadados | Scrapers e serviços necessários às bibliotecas de música, filmes e séries |
| Android | Integração de armazenamento, permissões, controlo remoto, áudio e sessão multimédia |

## Candidatos a exclusão da distribuição

A exclusão só será aplicada após validação de dependências e compilação:

- PVR, TV em direto, gravações, EPG e rádio associados a PVR.
- Jogos, respetivos controladores, portas e add-ons.
- Recursos e add-ons exclusivos de meteorologia.
- Web interface predefinida e interfaces web opcionais, se não forem necessárias a funcionalidades mantidas.
- Outros add-ons opcionais sem relação com reprodução, bibliotecas, ficheiros, rede ou dependências essenciais.

## Componentes que não devem ser removidos por associação nominal

- `kodi.imagedecoder` e `xbmc/imagefiles`: podem ser usados para artwork, thumbnails e recursos da interface.
- `kodi.vfs`: necessário para sistemas de ficheiros virtuais e fontes remotas, incluindo SMB/NFS.
- `kodi.inputstream`: manter até confirmar que não é necessário para streams ou formatos suportados.
- Add-ons de metadados de música e serviços associados: música continua a ser uma área principal.
- `xbmc/programs`: o nome não é suficiente para concluir que corresponde apenas ao menu “Programas”; analisar referências antes de excluir.
- Bibliotecas de imagens e descodificadores: não confundir visualização de fotografias com artwork de filmes/séries.

## Estratégia de implementação

1. Criar opções de build específicas do Kodi Custom, desligadas por omissão nas builds Kodi normais.
2. Usar essas opções para excluir módulos independentes de PVR e jogos das árvores de compilação, depois de confirmar todas as referências cruzadas.
3. Criar uma lista de add-ons da distribuição Kodi Custom, sem modificar a lista upstream partilhada por outras plataformas.
4. Aplicar a lista específica na fase de packaging Android, garantindo que add-ons de VFS, codecs, metadados e recursos essenciais continuam incluídos.
5. Simplificar a skin/menu principal para Música, Filmes, Séries e Pesquisar ficheiros.
6. Executar build e testes antes de considerar a limpeza concluída.

## Critérios de aceitação

A primeira APK candidata só será considerada válida se:

- Compilar para Android sem erros.
- Instalar e iniciar em Android TV/TV Box e em smartphone/tablet.
- Reproduzir ficheiros locais de áudio e vídeo.
- Aceder a partilhas SMB e, quando suportado pela build, NFS.
- Permitir pesquisa de ficheiros e utilização do gestor de ficheiros.
- Ler legendas embutidas e externas, permitindo selecionar e sincronizar faixas.
- Permitir importar/atualizar bibliotecas de música, filmes e séries.
- Não apresentar entradas de PVR, jogos ou meteorologia na interface.
- Não apresentar falhas por ausência de add-ons ou bibliotecas removidos.

## Estado

- [x] Identificação inicial das listas CMake de PVR e jogos.
- [x] Identificação da lista comum de add-ons instalada.
- [x] Identificação dos componentes que exigem proteção por dependências.
- [ ] Opções CMake específicas Kodi Custom.
- [ ] Lista de add-ons específica para Android.
- [ ] Menu principal simplificado.
- [ ] Build Android e testes funcionais.
