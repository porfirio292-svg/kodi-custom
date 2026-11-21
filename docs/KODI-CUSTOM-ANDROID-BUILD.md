# Kodi Custom — compilação Android

A compilação Android é executada pelo GitHub Actions, a partir da branch `custom-development`.

## Iniciar uma compilação

1. Abrir o separador **Actions** do repositório.
2. Selecionar **Build Kodi Custom Android APK**.
3. Escolher **Run workflow**.
4. Manter `arm64-v8a` para a maioria dos equipamentos Android atuais ou escolher `armeabi-v7a` para equipamentos de 32 bits.
5. Iniciar a execução e aguardar que o job termine.
6. Abrir a execução concluída e descarregar o artefacto `kodi-custom-<arquitetura>-<execução>`.

O artefacto contém a APK de teste. Não é uma versão publicada nem assinada com uma chave de distribuição: a compilação usa uma chave de debug gerada durante o job.

## O que o workflow valida

- Faz checkout da branch `custom-development`.
- Inicializa os submódulos Git de forma recursiva, incluindo o scraper oficial TheTVDB v4.
- Confirma a presença do add-on, do override de idioma pt-PT e das respetivas entradas de instalação CMake.
- Prepara o ambiente Android com JDK 17, SDK Android 34 e NDK 21.4.7075529, conforme a documentação de compilação da base Kodi 21.3 Omega.
- Compila as dependências, configura e compila o Kodi e gera a APK.

## Limitações da primeira compilação

A conclusão do workflow confirma que foi possível compilar e empacotar o código para a arquitetura escolhida. Não substitui os testes de instalação e funcionamento.

Após obter a APK, é necessário testar:
- arranque e reprodução de ficheiros locais;
- acesso a partilhas SMB;
- deteção e seleção de legendas externas e incorporadas;
- scraper TMDb para filmes, com idioma pt-PT;
- scraper TheTVDB v4 para séries, temporadas e episódios;
- instalação e navegação em Android TV/TV Box e em Android móvel.

A primeira APK é apenas para validação. A assinatura de distribuição, a identidade visual final e a publicação em GitHub Releases ficam para uma fase posterior.
