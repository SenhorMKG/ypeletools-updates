**YPELÉ TOOLS — BETA 3.7.0**
Changelog oficial · outubro de 2026
Plataforma de referência: Windows 10 e 11 (64 bits) · Python 3.13

A 3.7.0 traz a aba yTorrents (links diretos, MEGA, MediaFire, Pixeldrain, Dropbox, arquivos .torrent e magnets), transforma o Vocal Remover em Áudio Tools com um BPM Analyzer e um Key Detector, renova o visual de todas as abas, corrige o Spotify depois das mudanças que o próprio Spotify fez na API em 2026, adiciona zoom com a roda do mouse na aba Imagens e deixa o atualizador ir direto para a versão mais nova.

====================================================================

**NOVIDADE: YTORRENTS**

**O que baixa**
- Links diretos HTTP e HTTPS, com:
  - retomada de onde parou (se o servidor permitir), inclusive depois de fechar o app;
  - novas tentativas automáticas quando a conexão cai (5 por padrão, configurável);
  - redirecionamentos, nome e tamanho lidos do servidor, velocidade e tempo restante;
  - limite de velocidade opcional e conferência do tamanho final (e do SHA-256 ou MD5, quando o link informa).
- MEGA (links públicos de arquivo): o arquivo é baixado e decifrado no seu computador, e a assinatura do MEGA (MAC) é conferida no fim.
- MediaFire, Pixeldrain e Dropbox (links públicos).
- Arquivos .torrent e links magnet, com um motor de BitTorrent próprio do app:
  - rastreadores HTTP e UDP, DHT e troca de peers (PEX);
  - nome, tamanho, progresso, velocidade, tempo restante, peers e seeds;
  - cada peça é conferida pelo SHA-1 do torrent antes de ser gravada como pronta;
  - pausar, retomar e cancelar; depois de fechar e abrir o app, continua das peças que já estavam prontas;
  - torrents com várias pastas e arquivos mantêm a estrutura original.
- Serviços que exigem CAPTCHA, espera com propaganda, login ou pagamento (DropGalaxy e parecidos) não são contornados: o app diz isso na revisão e não adiciona o link.

**Como usar**
- Cole um ou vários links (um por linha, ou separados por espaço) e clique em "Adicionar link", ou use "Adicionar .torrent".
- O app identifica cada link, lê nome e tamanho e mostra uma revisão. Nada entra na fila antes de você confirmar, e dá para desmarcar o que não quiser.
- "Iniciar fila" começa os downloads. Cada linha tem pausar/retomar, cancelar, abrir a pasta e um menu com mais opções.
- Abas "Baixando" e "Concluídos", com o número de itens.
- No rodapé: pausar todos, retomar todos, cancelar todos (com confirmação), tentar as falhas de novo e limpar os concluídos (tira da lista, nunca apaga os arquivos).
- Ao cancelar, você escolhe se mantém o que já foi baixado (para retomar depois) ou se apaga.

**Onde ficam os arquivos**
- Downloads\yPeléTools\yTorrents\<nome>\<arquivo>, uma pasta por download. Dá para desligar a pasta individual nas configurações.
- A pasta de destino pode ser trocada no topo da aba.
- Nomes inválidos no Windows são corrigidos e nada é sobrescrito: se o nome já existir, entra um número.
- Enquanto baixa, o arquivo fica como .ypele-parcial. Ele só ganha o nome final depois de conferido.

**Configurações (engrenagem da aba)**
- Downloads ao mesmo tempo (1 a 10), tentativas automáticas, limite de velocidade por download, pasta individual e DHT.
- A fila é guardada e volta ao abrir o app. Quem estava baixando volta para a fila.

====================================================================

**NOVIDADE: ÁUDIO TOOLS (antigo Vocal Remover)**

A aba Vocal Remover agora se chama Áudio Tools e tem três ferramentas: Vocal Remover, BPM Analyzer e Key Detector.

**Vocal Remover**
- Funciona como antes.
- Ao terminar uma separação, o app abre a pasta exata dos stems, uma vez só. Se a separação falhar, nada é aberto.
- A tela de exportação ganhou o link "abrir pasta", que abre a pasta dos arquivos exportados.

**BPM Analyzer**
- Cole um link do YouTube, do SoundCloud ou do Spotify. O app mostra capa, título, artista, origem e duração.
- O BPM vem primeiro do catálogo público do Deezer, só quando título, artista e duração batem com a música (ou pelo ISRC, no Spotify).
- Se não houver BPM nos dados disponíveis, aparece "Não encontramos o BPM nos dados disponíveis. Deseja analisar o áudio?". Você pode analisar o áudio do próprio link (baixado numa pasta temporária e apagado no fim) ou anexar um arquivo do computador.
- A análise do áudio é feita no seu computador: detecção dos ataques, autocorrelação e acompanhamento das batidas.
- O resultado mostra o BPM, o método usado (catálogo ou análise do áudio) e a confiança. Quando a música pode ser lida como metade ou o dobro (ex.: 87 ou 174), o app mostra a alternativa.

**Key Detector**
- Mesmo fluxo: link ou arquivo.
- Mostra a tonalidade (ex.: Fá maior), a tônica, o modo, a notação Camelot (ex.: 7B), a confiança e o método.
- A afinação da gravação é estimada antes, para músicas que não estão em lá = 440 Hz.

**Tela do resultado**
- Inspirada no layout do Tunebat: capa grande, título e artista, e cartões com Tonalidade, Camelot, BPM e Duração.
- Botões "Inserir outro link", "Anexar arquivo" e "Analisar novamente".
- Arquivos de áudio e vídeo aceitos: os mesmos do Vocal Remover. O arquivo original nunca é alterado.

====================================================================

**CORREÇÕES E MELHORIAS**

**1. Spotify**
- O erro "API do Spotify: acesso negado pela API (403)" tinha uma causa real: desde março de 2026, o Spotify só libera a Web API, em modo de desenvolvimento, para apps cujo dono tenha Premium ativo, e mudou os endereços das playlists.
- A página pública do Spotify (embed) passou a ser o caminho principal. Não precisa de conta, chave nem login.
- Com credenciais próprias, o app usa os endereços novos da API (/items), com o antigo como reserva, e renova o token vencido uma vez.
- Quando a API recusa, o painel diz o motivo exato (Premium exigido, playlist que não é sua, limite de consultas com o tempo de espera, link inexistente) e o app não insiste a cada faixa: espera 30 minutos antes de tentar a API de novo.
- Nenhuma chave, token ou cookie vem dentro do app.
- O áudio continua vindo do YouTube, pela busca da faixa, como antes. O app não baixa áudio do Spotify e não contorna DRM.

**2. Visual**
- Todas as abas foram revisadas no mesmo estilo escuro, com a cor de cada ferramenta: YouTube vermelho, SoundCloud laranja, Spotify verde, Redes Sociais roxo (#B300FF), Conversor azul, Imagens ciano, Áudio Tools rosa e yTorrents azul com detalhes roxos.
- O ícone da barra superior agora é o mesmo desenho do ícone da aba no menu lateral, num quadro com a cor da aba.
- Cada aba de download ganhou o ícone no cabeçalho.

**3. Aba Imagens: zoom com a roda do mouse**
- Roda para cima aumenta, para baixo diminui, centrado onde está o mouse, até 800%.
- Arraste para navegar pela imagem ampliada. Clique duplo alterna entre 100% e "Ajustar".
- Um selo mostra a porcentagem. O botão de 100% e os outros controles de zoom continuam funcionando e acompanham a roda.
- O zoom é só da visualização: a imagem salva não muda.

====================================================================

**ATUALIZAÇÃO E INSTALADOR**
- Quem tem a Alpha 3.3 ou 3.4, ou a Beta 3.5.0 ou 3.6.0, vai direto para a 3.7.0 pelo botão amarelo, sem instalar as versões do meio.
- A partir da 3.7.0, o app procura a maior versão estável publicada, mesmo que a Release marcada como "mais recente" no GitHub seja uma correção antiga. Pré-lançamentos, rascunhos e versões iguais ou menores nunca são oferecidos.
- A assinatura Ed25519, a chave pública, o manifesto e a conferência SHA-256 continuam iguais.
- O pacote traz dois arquivos novos na pasta do app: ypele_transferencias.py (yTorrents) e ypele_analise.py (BPM e tonalidade).
- Nenhuma dependência nova: o requirements.txt é o mesmo da 3.6.0.
- Configurações, filas, histórico, separações, downloads e a pasta de dados ficam como estão.

====================================================================

**LIMITES CONHECIDOS**
- Torrents: sem criptografia de protocolo (MSE), sem uTP e sem torrents só v2. Peers que exigem criptografia não conectam. Na primeira vez que um torrent roda, o Firewall do Windows pode pedir permissão para o app receber conexões.
- MEGA: só links públicos de arquivo. Pastas do MEGA e o limite de transferência do MEGA não são contornados.
- O BPM do catálogo depende do Deezer ter a música. Músicas com andamento livre ou muito sincopadas podem ter confiança baixa.
- A página pública do Spotify mostra até 100 faixas de uma playlist. Para listas maiores, é preciso usar a API com uma conta Premium dona do app.
