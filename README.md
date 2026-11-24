# RetroConsole 3D TV

Aplicativo nativo para **Android TV** e **Amazon Fire TV**, desenvolvido por **Rodrigo Ferreira**, para jogar clássicos de **PlayStation 1 (PS1)** e **Nintendo 64 (N64)** com interface desenhada para controle remoto (D-Pad) e controles Bluetooth (como DualShock 4 e gamepads padrão Android).

> **Download direto da versão mais recente (`v1.0.5`):**  
> - **Pelo Downloader (Fire TV / Android TV):** código **`3690043`** (`aftv.news/3690043`)  
> - **Link direto do APK:** [Baixar RetroConsole3D-FireTV.apk](https://github.com/rfernandes0375/RetroConsole-3D-TV-Releases/releases/latest/download/RetroConsole3D-FireTV.apk)

---

## Visão Geral e Propósito

O **RetroConsole 3D TV** nasceu para rodar em TVs e TV Boxes com pouco armazenamento interno (como Fire TV Stick) sem precisar entupir a memória com dezenas de gigabytes de ROMs fixas.

Em vez de instalar uma biblioteca inteira no aparelho, o aplicativo opera sob demanda:
- Navegue por um catálogo curado local com **430 jogos** e **430 capas** embutidas.
- Ao escolher um jogo, a ROM compactada (`.7z`) é baixada sob demanda, extraída em área temporária com aceleração nativa e iniciada a **60 FPS** em processo isolado.
- Apenas seus **saves reais** (Save States e Memory Cards / SRAM validados) permanecem gravados permanentemente no dispositivo, preservando espaço e garantindo retomada segura.

---

## Principais Recursos

- **Catálogo completo e offline-first na vitrine:** **430 jogos** (**295 de Nintendo 64** e **135 de PlayStation 1**), incluindo **24 títulos multi-disco de PS1** (2, 3 e 4 CDs) com seletor rápido de disco no Menu Rápido.
- **Execução isolada e estável:** motor nativo **LibretroDroid** (`pcsx_rearmed` para PS1 e `mupen64plus_next` para N64, ABIs `armeabi-v7a` e `arm64-v8a`) rodando em processo dedicado `:player`, com retorno limpo coordenado pelo `PlayerExitCoordinator`.
- **Extração `.7z` nativa acelerada:** decodificação em C/C++ via `libarchive` (com fallback JVM), buffers amplos de `256 KB` e barra de progresso em tempo real exibindo percentual (`%`), velocidade, tamanho em MB, tempo decorrido e estimativa restante.
- **Armazenamento inteligente e autolimpeza:**
  - `active_rom/`: área temporária limpa automaticamente ao cancelar, encerrar o jogo ou detectar `.7z` corrompido/truncado.
  - `kept_rom/`: opção ao sair do jogo para manter até **1 jogo/disco** no cache para abertura instantânea na próxima partida.
  - Barra compacta de retomada rápida no cabeçalho (`Último jogado` + status real de save + botão **Retomar**) sem poluir os 430 cards da grade.
- **Detecção estrita de saves reais:** validação binária de Memory Cards de PS1 e EEPROM/MemPak/SRAM/FlashRAM de N64 (`SramSaveValidator` e `SaveStateRepository`), separando rigorosamente **Recentes** de **Com Save** (badge `SAVE`).
- **Atualizações automáticas (OTA) integradas:** botão **Atualizar** no cabeçalho com checagem silenciosa na abertura e periódica (intervalo mínimo de 30 minutos), indicador discreto `• Atualizar (vX.Y.Z)`, verificação criptográfica `SHA-256` antes da instalação e zero tokens embutidos no APK.
- **Controles refinados para TV e Gamepad:** suporte nativo a DualShock/Analógicos no PS1 com espelhamento inteligente do analógico esquerdo para D-Pad em jogos clássicos sem suporte analógico, atalho seguro do Menu Rápido sem colisões em partidas e busca/filtros combináveis por plataforma, gênero e modo `2 Jogadores`.

---

## Linha do Tempo e Evolução Técnica

1. **Fundação Nativa em Processo Isolado (`:player`)**
   - Projeto construído em Kotlin (`minSdk 23`, `targetSdk 34`, `compileSdk 34`, JVM 17) separando a vitrine (`HomeActivity`) do emulador (`NativePlayerActivity` no processo `:player`).
   - Adoção do `PlayerExitCoordinator` para pausar áudio/vídeo, gravar SRAM dentro de janela segura e só limpar ROMs temporárias após a descarga completa do núcleo nativo.

2. **Catálogo Curado de 430 Jogos, Multi-Disco e Capas Assíncronas**
   - Estruturação de 295 jogos de N64 e 135 títulos selecionados de PS1 (somando 430 jogos e 430 capas JPEG embutidas em `assets/covers/`), com carregamento assíncrono e `LruCache` dimensionado por memória (`CoverImageLoader`).
   - Suporte nativo a 24 jogos de PS1 com múltiplos CDs (troca de disco efêmera preservando o Memory Card) e download resiliente via Google Drive API v3 (incluindo Resource Keys) e URLs diretas `.7z`.

3. **Cache Inteligente, Blindagem de Saves e Limpeza de Arquivos Corrompidos**
   - Separação rigorosa entre cache efêmero (`active_rom/`), cache opcional de reabertura rápida (`kept_rom/`) e saves permanentes (`filesDir/saves/<game_id>/`).
   - Implementação do `SramSaveValidator` e do `SaveStateCoordinator` para impedir que Memory Cards recém-formatados em branco apareçam falsamente como jogos salvos.
   - Endurecimento do `EphemeralRomManager` para apagar imediatamente `.7z`, parciais e temporários ao detectar erro de extração (`Compressed data is corrupt` / fluxo truncado), mantendo saves intactos e oferecendo *"Tentar Novamente"* de forma limpa.

4. **Aceleração Nativa `.7z` (`libarchive`) e Acabamento Visual na TV**
   - Substituição da decodificação `.7z` lenta em Java puro por extração nativa via `libarchive` (`armeabi-v7a` e `arm64-v8a`), reduzindo minutos de espera no Fire TV para poucos segundos, com progresso contínuo, percentual, tempo decorrido e ETA no `DownloadTelemetryFormatter`.
   - Enquadramento proporcional da capa no modal de download e extração (`fitCenter` + `adjustViewBounds`) dentro de moldura arredondada escura (`14dp`), respeitando capas retangulares horizontais, quadradas e verticais sem cortes nem distorção.

5. **Sistema OTA Seguro em Dois Repositórios + CI/CD**
   - Separação entre código-fonte privado e repositório público de distribuição (`RetroConsole-3D-TV-Releases`), removendo senhas de keystore do Gradle e mantendo chaves apenas via variáveis de ambiente/configuração local não versionada.
   - Criação do `AppUpdateManager` com consulta a `version.json`, download em área dedicada de cache, validação obrigatória de hash `SHA-256` e abertura do instalador nativo via `FileProvider` + `REQUEST_INSTALL_PACKAGES`.

6. **Analógico Completo no PS1, Menu Rápido sem Conflito e Resumo Rápido**
   - Ativação do modo DualShock/Analógico no `pcsx_rearmed` (`pcsx_rearmed_pad1type`/`pad2type = "analog"` e `setControllerType` via `CoreConfig`) somada ao espelhamento do Analógico Esquerdo para D-Pad exclusivamente no PS1 (`deadzone 0.40f` e histerese `0.30f`), preservando intacto o comportamento do Nintendo 64.
   - Remoção do atalho `L1 + R1` (que causava abertura acidental em jogos como *Winning Eleven*) e substituição pelos atalhos seguros `L3 + R3`, `Select + Start` e `Voltar`, atualizando HUD e `QuickMenuControlsGuide`.
   - Adição de checagem silenciosa periódica de atualizações (intervalo mínimo de 30 minutos) com destaque discreto `• Atualizar (vX.Y.Z)` no cabeçalho e faixa de retomada rápida (`Último jogado` + status real de save).

7. **Qualidade Visual HD & CRT Leve Otimizada para Fire TV**
   - Criação de perfis gráficos configuráveis em tempo real no Menu Rápido (`VisualQualityProfile`): **Imagem Nítida (HD)** (padrão recomendado com `NEON 2x` no PS1, `640x480`/`960x540` no N64 e shader `Sharp`), **TV de Tubo (Retrô)** (resolução 2x + shader `CRT`) e **Modo Leve (Mais Rápido)** (`1x` nativo com `ShaderConfig.Sharp` para máxima fluidez com bordas limpas em modelos antigos de Fire TV Stick).

8. **Padronização Limpa das Capas na Grade da Vitrine (`HomeActivity`)**
   - Refatoração de `item_game_card.xml`, `bg_card_cover_frame.xml` e `GameCardAdapter` para exibir todas as 430 capas sem cortes (`fitCenter` + `adjustViewBounds`) em moldura escura arredondada (`10dp`) harmonizada ao modal de download, movendo selos (`PS1`/`N64`, `2P`, `SAVE`, `★`, tamanho, `CDs` e `EM CACHE`) para faixas dedicadas acima e abaixo da capa com recorte único no `ViewHolder` para manter rolagem fluida.

---

## Qualidade Visual e Upscaling para Fire TV

Para entregar imagem mais nítida em TVs Full HD sem sacrificar os **60 FPS** nem introduzir *input lag* (evitando filtros pesados como `xBRZ 4x/6x` ou `CRT-Royale`), o **RetroConsole 3D TV** disponibiliza três perfis gráficos leves persistidos entre sessões e alternáveis diretamente em **Menu Rápido → Qualidade**:

| Modo no Menu Rápido | PlayStation 1 (`pcsx_rearmed`) | Nintendo 64 (`mupen64plus_next`) | Shader (`GLRetroView`) | Indicação |
| :--- | :--- | :--- | :--- | :--- |
| **Qualidade: Imagem Nítida (HD)** *(padrão)* | NEON 2x (`pcsx_rearmed_neon_enhancement_enable = enabled`, preservando efeitos 2D) | `640x480` (`4:3`) / `960x540` (`16:9`) | `ShaderConfig.Sharp` (bordas nítidas sem borrão) | Fire TV Stick HD/4K e TV Boxes |
| **Qualidade: TV de Tubo (Retrô)** | NEON 2x (`enabled`) | `640x480` (`4:3`) / `960x540` (`16:9`) | `ShaderConfig.CRT` (suaviza serrilhados e *dithering*) | Visual clássico de TV de tubo |
| **Qualidade: Modo Leve (Mais Rápido)** | Resolução nativa 1x (`disabled`) | `320x240` (`4:3`) / `640x360` (`16:9`) | `ShaderConfig.Sharp` (suavização leve e limpa) | Máxima fluidez em aparelhos mais antigos |

- **Tela cheia intacta:** a mudança de qualidade visual nunca reduz a área da imagem na TV, mantendo a proporção configurada (**4:3 Original** ou **16:9 Widescreen**).
- **Mensagens simples no Menu Rápido:** ao alternar os modos durante a partida, o menu confirma a troca em linguagem simples e avisa quando é recomendável reabrir o jogo para obter a nitidez 2x completa, mantendo o progresso e os saves intactos.

---

## Guia Rápido de Instalação e Uso (Fire TV / Android TV)

### Instalação Rápida no Fire TV via Downloader

> **Compatibilidade:** funciona nativamente em **Amazon Fire TV** e dispositivos **Android TV / Google TV** (**não roda nativamente em Roku TV**).

1. **Baixe e abra o Downloader:** procure pelo aplicativo **Downloader** (ícone laranja) na loja do seu Fire TV ou Android TV, instale-o e abra.
2. **Digite o código `3690043`:** no campo de URL/busca do Downloader, digite **`3690043`** (ou `aftv.news/3690043`, que aponta diretamente para `https://github.com/rfernandes0375/RetroConsole-3D-TV-Releases/releases/latest/download/RetroConsole3D-FireTV.apk`) e confirme para iniciar o download.
3. **Libere a instalação na primeira vez:** quando o Android/Fire TV bloquear a instalação inicial, clique em **Configurações**, ative **Instalar apps desconhecidos** para o **Downloader** e volte para concluir a instalação do **RetroConsole 3D TV**.
4. **Atualizações futuras direto pelo app:** depois de instalado, basta clicar no botão **Atualizar** no cabeçalho superior do próprio **RetroConsole 3D TV** (que também exibirá **`• Atualizar (vX.Y.Z)`** quando houver versão nova). O aplicativo baixa o novo APK, valida o hash **SHA-256** e inicia o instalador — lembre-se de autorizar **Instalar apps desconhecidos** também para o **RetroConsole 3D TV** quando o sistema solicitar na primeira atualização interna.

### Controles e Atalhos

| Ação / Recurso | PlayStation 1 (Controle) | Nintendo 64 (Controle) | Teclado / Emulador |
| :--- | :--- | :--- | :--- |
| **Mover / Direcional** | D-Pad ou Analógico Esq. | Analógico Esq. / D-Pad | `WASD` ou Setas |
| **Câmera / C-Buttons** | Analógico Dir. | Analógico Dir. | `IJKL` ou Numpad `8/4/2/6` |
| **Ação Principal / Confirmar** | Cruz (`✕`) / Círculo (`○`) | Cruz (`✕`) = A / Quadrado (`□`) = B | `Z` ou `Espaço` / `X` |
| **Botões Extras / Gatilhos** | `□`, `△`, `L1`, `R1`, `L2`, `R2` | `L2` = Gatilho Z / `L1` = L / `R1` = R | `C`, `V`, `Q`, `E`, `Shift Esq.`, `R` |
| **Start / Select** | `Options` / `Share` | `Options` (`Start`) | `Enter` / `Tab` |
| **Abrir / Fechar Menu Rápido** | **`L3 + R3`**, **`Select + Start`**, `Menu` ou `Voltar` | **`L3 + R3`**, **`Select + Start`**, `Menu` ou `Voltar` | `ESC`, `F1` ou `Voltar` |
| **Navegar no Menu Rápido** | D-Pad + Cruz (`✕`) / Voltar: Círculo (`○`) | D-Pad + Cruz (`✕`) / Voltar: Círculo (`○`) | Setas ou `W`/`S` + `Enter` |

> **Atalhos extras no catálogo:** `L1`/`R1` alterna entre abas, `R2` inicia um jogo aleatório e `△` (`Y` / `Menu`) adiciona ou remove o jogo focado dos **Favoritos**.

---

## Histórico de Versões

### v1.0.5 (código 6)
- **Capas sem corte nos cards da grade (`HomeActivity`):** padronização dos cards do catálogo com enquadramento proporcional (`fitCenter` + `adjustViewBounds`) dentro de moldura escura arredondada harmonizada ao modal de download, preservando integralmente capas de PlayStation 1 (incluindo a faixa lateral "PlayStation") e de Nintendo 64 sem cortes ou distorção.
- **Badges organizadas fora da arte:** indicadores de plataforma (`PS1`/`N64`), multiplayer (`2P`), save (`SAVE`), favorito (`★`), tamanho (`MB`), multi-disco (`CDs`) e cache (`EM CACHE`) reposicionados em faixas superior e inferior dedicadas, mantendo alinhamento uniforme por linha e rolagem fluida na grade de 430 jogos.

### v1.0.4 (código 5)
- **Polimento do Modo Leve no Fire TV:** `Modo Leve (Mais Rápido)` passa a combinar resolução interna `1x` com suavização nítida (`ShaderConfig.Sharp`), mantendo os 60 FPS e reduzindo serrilhados em TVs Full HD.
- **Rótulos e mensagens simples no Menu Rápido:** modos renomeados para **Qualidade: Imagem Nítida (HD)**, **Qualidade: TV de Tubo (Retrô)** e **Qualidade: Modo Leve (Mais Rápido)**, com confirmações diretas em linguagem acessível ao jogador (sem jargões técnicos).
- **Documentação atualizada:** guia oficial alinhado aos novos nomes e sincronizado automaticamente no repositório público de releases.

### v1.0.3 (código 4)
- **Perfis de Qualidade Visual e Upscaling leve para Fire TV:** inclusão de 3 perfis gráficos (`Aprimorado HD • Recomendado` como padrão inicial, `Retrô Tubo • CRT Leve` e `Padrão • Desempenho Máximo`) com NEON 2x no PS1 (`pcsx_rearmed_neon_enhancement_enable`), resolução 2x (`640x480`/`960x540`) no N64 e shaders leves (`ShaderConfig.Sharp`, `ShaderConfig.CRT` e `ShaderConfig.Default`), sem reduzir o tamanho da imagem na TV nem sacrificar os 60 FPS.
- **Seletor em tempo real no Menu Rápido:** botão `Qualidade Visual` navegável por D-Pad/DualShock 4 no Menu Rápido com aplicação instantânea de shader via fila GL, sincronização entre `:player` e catálogo e persistência automática.
- **Documentação atualizada:** detalhamento dos perfis visuais no `README.md` e sincronização contínua com o repositório público de releases.

### v1.0.2 (código 3)
- **Analógicos completos no PS1 (`pcsx_rearmed`):** ativação nativa do modo DualShock/Analógico para Portas 1 e 2 combinada com espelhamento do Analógico Esquerdo para D-Pad (`deadzone 0.40f` e histerese `0.30f`) exclusivamente no PS1, preservando o Nintendo 64 inalterado.
- **Atalhos seguros no Menu Rápido:** remoção definitiva de `L1 + R1` (evitando abertura acidental em jogos de futebol/ação) e adoção de `L3 + R3`, `Select + Start` e `Voltar`, com atualização do HUD e do guia visual de comandos.
- **Verificação silenciosa periódica OTA:** checagem em segundo plano na abertura e ao retornar ao catálogo (cooldown de 30 minutos) exibindo indicador discreto `• Atualizar (vX.Y.Z)` no cabeçalho sem mover o foco do D-Pad.
- **Resumo compacto de retomada:** exibição discreta no cabeçalho do último jogo jogado com status estrito de save real e botão rápido **Retomar**, mantendo os 430 cards da grade leves.

### v1.0.1 (código 2)
- **Enquadramento proporcional das capas:** ajuste visual do modal de download e extração `.7z` com `fitCenter` + `adjustViewBounds` e moldura arredondada escura (`14dp`), exibindo todas as 430 capas sem cortes nem esticamento.

### v1.0.0 (código 1)
- **Versão inicial de produção:** motor nativo LibretroDroid (`pcsx_rearmed` e `mupen64plus_next`) em processo isolado `:player`.
- **Biblioteca completa:** 430 jogos (295 N64 + 135 PS1, incluindo 24 títulos multi-disco PS1) com 430 capas embutidas e download via Google Drive API v3 / `.7z`.
- **Extração nativa e blindagem:** descompactação `.7z` acelerada via `libarchive` com telemetria completa em tempo real, limpeza automática de `.7z` corrompidos, cache inteligente (`active_rom/` vs. `kept_rom/`), detecção estrita de saves reais e sistema de atualização OTA com validação `SHA-256`.
