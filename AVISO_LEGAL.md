# Aviso Legal, Privacidade e Licenças de Código Aberto — RetroConsole 3D TV

---

## 1. Natureza Independente, Não Comercial e Marcas Registradas

O **RetroConsole 3D TV** é um projeto independente, gratuito e sem fins lucrativos ou comerciais, desenvolvido para uso pessoal em dispositivos Android TV e Amazon Fire TV.

Este aplicativo **não possui qualquer vínculo, afiliação, patrocínio, autorização ou endosso** da **Nintendo**, **Sony Interactive Entertainment**, **Sega**, **Amazon** ou de quaisquer outros estúdios, publicadoras ou titulares de direitos autorais e propriedade intelectual relacionados às plataformas, consoles, logotipos, nomes comerciais, capas, artes e títulos exibidos.

Todas as marcas registradas, denominações de sistemas e imagens ilustrativas pertencem exclusivamente aos seus respectivos titulares e são referenciadas unicamente para fins descritivos, informativos de catalogação e identificação de compatibilidade.

---

## 2. Arquitetura Híbrida (`LIBRETRO_3D` e `ARCADE_WEB`), Direitos Autorais e Responsabilidade pelo Conteúdo

O **RetroConsole 3D TV** não reivindica titularidade sobre obras, jogos ou códigos de terceiros e opera por meio de dois módulos tecnicamente isolados:

- **Modo Emulação Nativa 3D (`LIBRETRO_3D` — PlayStation 1 e Nintendo 64):** o aplicativo atua exclusivamente como interface de catalogação e ambiente de execução local baseado no motor **LibretroDroid**. Cabe integral e exclusivamente ao usuário utilizar apenas cópias de segurança (*backups* de **ROMs/ISOs**) e arquivos de sistema (**BIOS**) sobre os quais detenha direito legítimo de uso ou licença válida, conforme a legislação vigente em seu país.
- **Modo Clássicos 2D Online (`ARCADE_WEB` / `WebView` — Arcade, Super Nintendo e Mega Drive):** o aplicativo atua exclusivamente como navegador e cliente de visualização (`WebView`) com ajustes e filtros locais de interface voltados à usabilidade em telas de TV. O projeto não hospeda, não controla, não edita e não garante a disponibilidade contínua de páginas ou conteúdos de terceiros acessados via internet, sendo o usuário integralmente responsável pela conformidade legal desse acesso em sua jurisdição.

---

## 3. Isenção de Garantias (*As-Is* / *As-Available*) e Limitação de Responsabilidade

Este software é disponibilizado **"no estado em que se encontra" (*as-is*) e "conforme disponibilidade" (*as-available*)**, sem garantias expressas ou implícitas de qualquer natureza — incluindo, sem limitação, garantias de funcionamento ininterrupto, compatibilidade universal com todos os títulos, disponibilidade permanente de servidores ou fontes externas, adequação a um propósito específico ou ausência total de falhas.

Na máxima extensão permitida pela legislação aplicável, o desenvolvedor não se responsabiliza por incompatibilidades de hardware ou sistema operacional, instabilidades de rede, alterações em serviços externos ou eventual perda de progresso, arquivos temporários, *memory cards* virtuais, estados salvos (*save states*) ou demais dados locais.

---

## 4. Privacidade, Armazenamento Local e Conformidade com a LGPD (Lei nº 13.709/2018)

O **RetroConsole 3D TV** adota privacidade mínima por concepção em conformidade com os princípios da **Lei Geral de Proteção de Dados Pessoais (LGPD — Lei nº 13.709/2018)**:

- **Sem conta ou identificação pessoal:** o aplicativo não solicita cadastro, login, senha, nome, CPF, e-mail ou telefone.
- **Sem coleta de dados pessoais, rastreadores ou publicidade:** não há coleta, armazenamento ou compartilhamento de dados pessoais identificáveis em servidores próprios, nem exibição de anúncios, SDKs de rastreamento comportamental ou telemetria comercial.
- **Armazenamento exclusivamente no dispositivo local:** arquivos de progresso (*save states* e *memory cards*/SRAM), lista de favoritos, histórico recente, preferências de vídeo/controle e cache temporário de ROMs permanecem gravados apenas no **dispositivo local** do usuário, sob seu controle exclusivo, podendo ser apagados a qualquer momento pelo próprio aplicativo ou pelas configurações do sistema.
- **Conexões de rede estritas:** acessos à rede ocorrem unicamente para:
  1. Verificar e baixar atualizações oficiais do próprio aplicativo (`version.json` / GitHub Releases, com validação criptográfica `SHA-256`);
  2. Baixar pacotes ou carregar recursos solicitados diretamente pelo usuário durante o uso do catálogo.

---

## 5. Canal de Notificação e Remoção (*Notice & Takedown*)

Titulares de direitos autorais, marcas registradas ou seus representantes legais que identifiquem qualquer referência, metadado, capa ou link que considerem incompatível com seus direitos podem encaminhar solicitação fundamentada de esclarecimento ou remoção pelo repositório público oficial do projeto no GitHub:

- **Canal oficial (*Notice & Takedown*):** `https://github.com/rfernandes0375/RetroConsole-3D-TV-Releases/issues`

O mantenedor compromete-se a analisar de boa-fé toda notificação devidamente fundamentada e a realizar a retirada célere ou o ajuste pertinente sempre que cabível.

---

## 6. Software de Terceiros e Conformidade de Licenças Open-Source

O **RetroConsole 3D TV** utiliza e integra componentes, núcleos de emulação e bibliotecas de código aberto desenvolvidos por terceiros sob suas respectivas licenças originais (**GPL**, **LGPL**, **MIT**, **Apache 2.0**, **BSD** e **Domínio Público**), incluindo:

| Componente / Biblioteca | Função no Aplicativo | Licença |
| :--- | :--- | :--- |
| **LibretroDroid** (`com.github.Swordfish90:LibretroDroid`) | Ponte Android / OpenGL ES para núcleos Libretro | **GPLv3** |
| **Libretro API** | Interface padrão de emulação para núcleos nativos | **MIT** |
| **PCSX-ReARMed** (núcleo Libretro) | Emulação de PlayStation 1 (`armeabi-v7a` / `arm64-v8a`) | **GPLv2** |
| **Mupen64Plus / Mupen64Plus-Next** (núcleo Libretro) | Emulação de Nintendo 64 (`armeabi-v7a` / `arm64-v8a`) | **GPLv2** |
| **libarchive / android-libarchive** (`me.zhanghai.android.libarchive`) | Extração nativa de pacotes `.7z` e `.rar` | **BSD / Apache 2.0** (compatível com LGPL) |
| **AndroidX** (`core-ktx`, `appcompat`, `recyclerview`, `constraintlayout`, `lifecycle`, `webkit`) | Interface TV, ciclo de vida e cliente `WebView` | **Apache 2.0** |
| **Kotlin & Kotlinx Coroutines** | Linguagem e concorrência assíncrona | **Apache 2.0** |
| **OkHttp** | Cliente HTTP/HTTPS para transferências e OTA | **Apache 2.0** |
| **Apache Commons Compress** | Suporte auxiliar a arquivos compactados `.7z` / `.zip` | **Apache 2.0** |
| **junrar** | Suporte auxiliar à leitura de arquivos `.rar` | **Licença aberta junrar (estilo BSD / código aberto)** |
| **XZ for Java** (`org.tukaani:xz`) | Descompressão auxiliar de fluxos LZMA/XZ | **Domínio Público** |

Cada componente listado permanece integralmente regido pelos termos de sua respectiva licença original, preservando os créditos e direitos autorais de seus respectivos autores e colaboradores; o **RetroConsole 3D TV** não reivindica titularidade sobre códigos de terceiros.
