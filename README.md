# RetroConsole 3D TV

Aplicativo nativo para **Android TV** e **Amazon Fire TV**, desenvolvido por **Rodrigo Ferreira**, para jogar clássicos de **PlayStation 1 (PS1)** e **Nintendo 64 (N64)** com interface desenhada para controle remoto (D-Pad) e controles Bluetooth (como DualShock 4 e gamepads padrão Android).

> **Download direto da versão mais recente (`v1.0.2`):**  
> [Baixar RetroConsole3D-FireTV.apk](https://github.com/rfernandes0375/RetroConsole-3D-TV-Releases/releases/latest/download/RetroConsole3D-FireTV.apk)

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

---

## Guia Rápido de Instalação e Uso (Fire TV / Android TV)

### Primeira Instalação

1. No seu **Fire TV** ou **Android TV**, baixe o APK oficial pelo aplicativo **Downloader** ou navegador/gerenciador de arquivos usando o link:
   ```text
   https://github.com/rfernandes0375/RetroConsole-3D-TV-Releases/releases/latest/download/RetroConsole3D-FireTV.apk
   ```
2. Caso o sistema solicite pela primeira vez, acesse **Configurações → Meu Fire TV → Opções do desenvolvedor → Instalar apps desconhecidos** (ou **Segurança / Apps desconhecidos** no Android TV) e habilite a permissão para o aplicativo usado no download (ex.: **Downloader**).
3. Conclua a instalação e abra o **RetroConsole 3D TV** na tela inicial.

### Como Atualizar pelo Aplicativo (OTA)

1. Quando uma nova versão estiver disponível, o botão **Atualizar** no cabeçalho superior exibirá o indicador discreto **`• Atualizar (vX.Y.Z)`** (ou clique manualmente em **Atualizar** a qualquer momento).
2. Selecione **Baixar e Instalar**: o aplicativo baixará o novo APK, validará automaticamente o hash **SHA-256** e abrirá o instalador do sistema.
3. Na primeira atualização interna via OTA no Android 8+ / Fire OS recente, autorize **"Instalar apps desconhecidos"** especificamente para o **RetroConsole 3D TV** quando solicitado; ao voltar, a tela de instalação continuará automaticamente.

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
