# Aviso Legal, Privacidade e Licenças de Código Aberto — RetroConsole 3D TV

---

## 1. Projeto Independente e Marcas Registradas

O **RetroConsole 3D TV** é um projeto independente e não comercial desenvolvido para uso pessoal em dispositivos Android TV e Amazon Fire TV.

Este aplicativo **não possui qualquer vínculo, afiliação, patrocínio, autorização ou endosso** da **Nintendo**, **Sony Interactive Entertainment**, **Amazon**, **SEGA** ou de quaisquer estúdios, publicadoras ou titulares de direitos autorais relacionados aos consoles, logotipos, nomes comerciais, imagens e jogos exibidos.

Todas as marcas registradas, nomes de sistemas e títulos mencionados pertencem exclusivamente aos seus respectivos proprietários e são referenciados apenas para fins de identificação e compatibilidade informativa.

---

## 2. Direitos Autorais e Uso Responsável

O **RetroConsole 3D TV** atua exclusivamente como **interface de organização e ambiente de execução para emulação**, não sendo comercializado e não reivindica titularidade sobre obras de terceiros.

Cabe integral e exclusivamente ao usuário utilizar o aplicativo em conformidade com a legislação vigente de direitos autorais em sua jurisdição, fazendo uso apenas de **cópias de segurança (*backups* de ROMs/ISOs e arquivos de sistema BIOS)** obtidas de forma legítima a partir de mídias originais próprias ou conteúdos expressamente autorizados / de domínio público.

---

## 3. Isenção de Garantias (*As-Is*)

Este software é disponibilizado **"no estado em que se encontra" (*as-is*)**, sem garantias expressas ou implícitas de qualquer natureza, incluindo, sem limitação, garantias de disponibilidade contínua de servidores ou espelhos externos, compatibilidade universal com todos os títulos, desempenho constante ou ausência total de falhas.

Em nenhuma hipótese o desenvolvedor será responsável por eventuais incompatibilidades de hardware, instabilidades de rede, encerramentos inesperados do sistema operacional ou eventual perda de progresso, arquivos temporários, *memory cards* virtuais ou estados salvos (*save states*).

---

## 4. Privacidade e Conexões de Rede

O **RetroConsole 3D TV** foi projetado com foco em privacidade mínima por padrão:

- **Sem conta ou cadastro:** o aplicativo não solicita login, senha, e-mail ou telefone.
- **Sem coleta de dados pessoais:** não há coleta de dados pessoais, rastreamento comportamental, anúncios comerciais, SDKs de publicidade nem telemetria comercial enviada a terceiros.
- **Uso estrito da rede:** as conexões de rede são realizadas exclusivamente para:
  1. Consultar e baixar atualizações oficiais do próprio aplicativo (via repositório público de releases, com validação SHA-256);
  2. Baixar pacotes ou executar jogos solicitados diretamente pelo usuário durante a navegação no catálogo.
- **Armazenamento local:** preferências de interface, lista de favoritos, histórico recente, cache temporário de ROM e progresso de jogos ficam armazenados localmente apenas no próprio dispositivo do usuário.

---

## 5. Software e Licenças de Terceiros

O **RetroConsole 3D TV** utiliza e integra componentes, núcleos de emulação e bibliotecas de código aberto desenvolvidos por terceiros, sob suas respectivas licenças (**GPL**, **LGPL**, **MIT**, **Apache 2.0** e **Domínio Público**), incluindo:

| Componente / Biblioteca | Função no Aplicativo | Licença |
| :--- | :--- | :--- |
| **LibretroDroid** (`com.github.Swordfish90:LibretroDroid`) | Ponte Android / OpenGL ES para núcleos Libretro | **GPLv3** |
| **Libretro** (API / especificação) | Interface padrão de emulação para núcleos nativos | **MIT** |
| **PCSX-ReARMed** (núcleo Libretro) | Emulação de PlayStation 1 (`armeabi-v7a` / `arm64-v8a`) | **GPLv2** |
| **Mupen64Plus / Mupen64Plus-Next** (núcleo Libretro) | Emulação de Nintendo 64 (`armeabi-v7a` / `arm64-v8a`) | **GPLv2** |
| **libarchive / android-libarchive** (`me.zhanghai.android.libarchive`) | Extração nativa de pacotes `.7z` e `.rar` (compatível com **LGPL / BSD / Apache 2.0**) | **Apache 2.0 / BSD / LGPL-compatível** |
| **Kotlin & Kotlinx Coroutines** | Linguagem e programação assíncrona | **Apache 2.0** |
| **AndroidX** (`core-ktx`, `appcompat`, `recyclerview`, `constraintlayout`, `lifecycle`, `webkit`) | Interface TV, ciclo de vida e WebView | **Apache 2.0** |
| **OkHttp** | Cliente HTTP/HTTPS para transferências e OTA | **Apache 2.0** |
| **Apache Commons Compress** | Suporte auxiliar a arquivos compactados `.7z` / `.zip` | **Apache 2.0** |
| **junrar** | Suporte auxiliar à leitura de arquivos `.rar` | **Licença aberta junrar (estilo BSD/Freeware com código-fonte aberto)** |
| **XZ for Java** (`org.tukaani:xz`) | Descompressão auxiliar de fluxos LZMA/XZ | **Domínio Público** |

Cada componente listado mantém integralmente os direitos autorais de seus respectivos autores e colaboradores originais e é regido pelos termos de sua respectiva licença.
