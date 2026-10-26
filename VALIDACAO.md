# Validação — yPeléTools Beta 3.7.0

Data: 25/10/2026. Tudo abaixo foi **executado** neste ambiente (Linux, Python 3.13.14, Xvfb e Wine 10.17).
Nada foi testado num Windows de verdade: onde aparece "Wine", o teste rodou o `.exe` do Windows dentro do Wine.

## Artefato testado

| Arquivo | SHA-256 |
|---|---|
| `release/v3.7.0/YPeleTools-Setup-Beta-3.7.0.exe` (22,6 MB) | `c5186a4f0821b8b0189b6e65472e7ad37bbaceace19a9a1f8a2568a26e03e8d9` |

Montado por `scripts/build_release.py` → `instalador/montar.py` (Go 1.24.7, Python portátil 3.13.17 conferido pelo
SHA-256). Um build novo gera outro SHA-256 (o pacote leva a data do build), com o mesmo conteúdo do app.

## 1. Testes automatizados

| O quê | Onde | Resultado |
|---|---|---|
| Suíte completa (`python -m unittest discover -s testes`) | Linux, Python 3.13.14 | **373 testes, OK** (1 pulado: motor de GPU do Upscayl, precisa do realesrgan-ncnn-vulkan) |
| `test_logica`, `test_atualizacao`, `test_publicacao`, `test_instalador`, `test_ytorrents` | **Python do instalador (Windows) no Wine** | 23 + 33 + 8 + 23 OK; yTorrents 54 OK, 2 pulados (MEGA sem biblioteca de AES no Wine, ver limitações) |
| `py_compile` de todos os `.py` e do `ypele_setup.pyw` | Linux | OK |

## 2. Conferência do instalador gerado (`build_release.py`, automática a cada build)

- PE de 64 bits do Windows; informações de versão com `3.7.0.0`, `Beta 3.7.0` e `yPeléTools`.
- O pacote embutido abre sem erro (testzip) e o `manifesto.json` diz Beta 3.7.0 / 3.7.0 / Python 3.13.17.
- Os 17 arquivos de `app/` são **byte a byte** iguais aos do projeto (`ypele_tools.py`, módulos, changelogs,
  `requirements.txt`, ícone); nenhum arquivo a mais.
- Leva `setup/ypele_setup.pyw` com a logo embutida, `runtime/python.exe`, `pythonw.exe`, `python313.dll`, `_tkinter`, `_ssl`.
- Nenhuma chave privada, `.chave`, `publicacao.json` ou cache de Python do projeto dentro.

## 3. Instalação limpa (Wine)

Prefixo do Wine novo. Os pacotes Python vieram de um índice local (o PyPI não é acessível daqui); FFmpeg e Deno de
espelhos locais com os arquivos oficiais do GitHub (SHA-256 conferidos pelo próprio instalador).

- Tela "Instalar o yPeléTools", Beta 3.7.0, pasta `%LOCALAPPDATA%\Programs\YPele Tools`.
- Todas as etapas: `python=ok, app=ok, ytdlp=ok, imagens=ok, extras=ok, ffmpeg=ok, deno=ok, atalhos=ok, verificar=ok`.
- FFmpeg 9.0.2 e Deno 2.9.8 instalados e testados pelo instalador; yt-dlp 2026.10.21 e yt-dlp-ejs 0.8.0 reais.
- `runtime\yPeleTools.exe` criado (executável próprio com o nome e o ícone do app).
- Atalhos: `Menu Iniciar\Programs\yPeléTools.lnk` e `Desktop\yPeléTools.lnk`.
- "Aplicativos instalados" (HKCU\…\Uninstall\YPeleTools): DisplayName yPeléTools, DisplayVersion Beta 3.7.0,
  VersionMajor 3, VersionMinor 7, UninstallString e QuietUninstallString.
- "Abrir yPeléTools" abriu o app 3.7.0 pelo `yPeleTools.exe`: Início, menu com Áudio Tools e yTorrents, rodapé
  "Beta 3.7.0", abas yTorrents e Áudio Tools abertas sem erro, nenhum crash-log.

## 4. Atualização por cima da 3.6.0 publicada (Wine)

- Instalado o `YPeleTools-Setup-Beta-3.6.0.exe` publicado (SHA-256 `03a8b9b4…`), aberto uma vez.
- Dados de usuário criados: `dados\config.json` (simultâneos = 7), um arquivo próprio em `dados\` e um download em
  `Downloads\yPeléTools\Minha playlist\`.
- O instalador 3.7.0 reconheceu: "O yPeléTools Beta 3.6.0 está instalado nesta pasta e será atualizado para a Beta
  3.7.0"; componentes já instalados foram reaproveitados (`python=pronto … app=ok, atalhos=ok, verificar=ok`).
- Depois: `app\ypele_tools.py` = 3.7.0, módulos novos presentes, `instalacao.json` = Beta 3.7.0.
- **Dados do usuário byte a byte iguais** (SHA-256 de cada arquivo antes e depois).
- O app 3.7.0 abriu, achou o download antigo (Armazenamento: 1 arquivo) e, ao fechar, manteve "simultâneos = 7".

## 5. Desinstalação (Wine)

- `QuietUninstallString` (`--desinstalar --silencioso`): saiu com código 0.
- Atalhos e a entrada de "Aplicativos instalados" removidos.
- Configurações, filas (`downloads_fila`, `conversor_fila`, `ytorrents_fila`) e downloads **ficaram**.
- A pasta do Python do app sai por um `cmd` que espera o desinstalador fechar; rodando esse mesmo comando no Wine, a
  pasta `YPele Tools` foi apagada.

## 6. Atualização automática (atualizadores publicados)

Com o `ypele_atualizador.py` **publicado** nas Releases v3.4.0 e v3.6.0 (código real) e um pacote 3.7.0 montado pelo
`publicar_atualizacao.py` (assinado com uma chave de TESTE pelo modo de teste que esses atualizadores já têm, só em
127.0.0.1): procurar → baixar → conferir assinatura e SHA-256 → aplicar. 17/17 arquivos certos, backup criado,
arquivos e dados do usuário intactos; depois disso nenhuma oferta de versão igual ou menor. (Linux.)

## 7. Envio ao GitHub

`publicar_atualizacao.py enviar` testado com um `gh` falso e um GitHub local (8 testes): rascunho → conferência dos
anexos → publicação como "latest" → conferência pública; recusa Release existente, anexo incompleto e pasta adulterada.
O envio real **não** foi feito: depende da sua chave privada e da sua conta.

## Limitações e o que não deu para testar

- **Windows real**: não testado. Wine não é Windows (SmartScreen, antivírus, permissões e fontes podem variar).
- **Pacotes Python no Wine**: numpy, Pillow, onnxruntime, curl_cffi, pycryptodomex, brotli etc. eram SUBSTITUTOS de
  teste (o PyPI não é acessível daqui). Por isso, no Wine, Imagens/Vocal Remover/BPM e o MEGA não foram exercitados;
  no Windows real o instalador baixa os pacotes verdadeiros do PyPI. Essas funções passam nos testes do Linux.
- **GitHub Actions**: o workflow foi validado como YAML e roda os mesmos comandos testados aqui, mas não foi
  executado num runner (precisa do repositório no GitHub).
- **Wine e nomes com acento**: atalhos com "é" no nome só funcionam no Wine com o locale UTF-8; no Windows não há essa
  limitação.
- **Assinatura de código (Authenticode)**: o `.exe` não é assinado com certificado; o Windows pode mostrar o
  SmartScreen na primeira execução. Isso já era assim nas versões anteriores.
