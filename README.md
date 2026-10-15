# Geotag & EXIF Studio — Versão 2.0

> **Estampa de telemetria, bússola vetorial e processamento em lote para fotos aéreas e de campo.**
> Arquivo único • 100% offline • Processamento local • EXIF padrão e XMP da DJI • Metadados preservados na exportação.

---

## 📌 Sumário
- [1. Visão Geral](#1-visão-geral)
- [2. Principais Funcionalidades](#2-principais-funcionalidades)
- [3. Requisitos e Como Executar](#3-requisitos-e-como-executar)
- [4. Guia de Uso](#4-guia-de-uso)
  - [4.1 Carregamento de Imagens](#41-carregamento-de-imagens)
  - [4.2 Logotipo Institucional](#42-logotipo-institucional)
  - [4.3 Bússola Gráfica Vetorial](#43-bússola-gráfica-vetorial)
  - [4.4 Conteúdo da Faixa de Texto](#44-conteúdo-da-faixa-de-texto)
  - [4.5 Estilo Visual, Fundo e Transparência](#45-estilo-visual-fundo-e-transparência)
  - [4.6 Visualizador (Zoom e Arrasto)](#46-visualizador-zoom-e-arrasto)
  - [4.7 Exportação](#47-exportação)
  - [4.8 Configurações Padrão](#48-configurações-padrão)
- [5. Engenharia de Metadados (EXIF e XMP DJI)](#5-engenharia-de-metadados-exif-e-xmp-dji)
- [6. Arquitetura do Código](#6-arquitetura-do-código)
- [7. Desempenho e Memória](#7-desempenho-e-memória)
- [8. Limitações Conhecidas](#8-limitações-conhecidas)
- [9. Histórico de Versões](#9-histórico-de-versões)
- [10. Autor e Licença](#10-autor-e-licença)

---

## 1. Visão Geral

O **Geotag & EXIF Studio** estampa automaticamente dados técnicos — coordenadas GPS, altitude absoluta ou relativa à decolagem, azimute, data, hora e equipamento — sobre fotografias aéreas de drones (DJI, Autel etc.) e fotos de campo, junto com uma bússola vetorial e o logotipo institucional.

Todo o aplicativo está em um único arquivo, [`geotag_exif_studio.html`](geotag_exif_studio.html): HTML, CSS, JavaScript, a biblioteca de leitura EXIF e o logotipo padrão do IBAMA estão embutidos. Não há instalação, servidor nem acesso à internet.

---

## 2. Principais Funcionalidades

* 🌐 **100% offline e privado:** nenhuma foto ou metadado sai do computador. Leitura, desenho e exportação ocorrem localmente no navegador.
* 🛰️ **Metadados EXIF e XMP da DJI:** coordenadas GPS, altitude absoluta (`GPSAltitude` / `AbsoluteAltitude`), altura relativa à decolagem (`RelativeAltitude`), azimute (`GimbalYawDegree`, `GPSImgDirection`, `FlightYawDegree`), inclinação do gimbal (`GimbalPitchDegree`), data/hora e equipamento.
* 🧭 **Bússola vetorial com dois modos:** *Seta Norte* para fotos vistas de cima (indica onde está o Norte real na imagem) e *Rumo da Câmera* para fotos oblíquas; no modo automático a escolha é feita pela inclinação do gimbal.
* 🏷️ **Faixa de texto configurável:** título, coordenadas, altitude, azimute, equipamento, data (`DD/MM/AAAA`) e hora (`HH:MM`), cada item podendo ser ligado ou desligado.
* 🛡️ **Logotipo institucional:** logotipo oficial do IBAMA embutido como padrão, reproduzido sem alterações; também aceita logotipos próprios em PNG ou JPEG.
* 🎨 **Fundo e transparência:** padrão global para texto, bússola e logotipo, com ajuste individual por elemento e contorno de legibilidade automático.
* 📍 **Seis posições** (cantos e centros superior/inferior) para bússola, faixa de texto e logotipo.
* 🗂️ **Metadados preservados:** os JPEGs exportados mantêm o EXIF (GPS, data, câmera) e o XMP originais, podendo ser reimportados em SIG, Google Earth ou fotogrametria.
* ⚡ **Processamento em lote:** progresso, cancelamento e relatório de falhas; saída em ZIP (dividido em partes de até 2 GB) ou gravação direta em pasta (Chrome/Edge).
* 🔍 **Visualizador com zoom e arrasto** (10% a 500%) e pré-visualização leve, com exportação sempre em resolução total.
* ❓ **Ajuda embutida:** botão **Ajuda** no cabeçalho (ou tecla `F1`) com o guia de uso, disponível sem internet.

---

## 3. Requisitos e Como Executar

### Requisitos
- Navegador moderno: Google Chrome, Microsoft Edge, Mozilla Firefox, Safari, Opera ou Brave.
- Nenhum servidor, Node.js, Python ou conexão com a internet.
- A gravação direta em pasta (seção 4.7) está disponível no Chrome e no Edge.

### Como abrir
1. Baixe o arquivo [`geotag_exif_studio.html`](geotag_exif_studio.html) (ou clone o repositório).
2. Dê um duplo clique no arquivo. O aplicativo abre pronto para uso.

---

## 4. Guia de Uso

O painel de controle à esquerda é organizado em cinco cards, na ordem abaixo. O botão **Ajuda**, no canto superior direito (ou a tecla `F1`), abre um resumo deste guia dentro do próprio app; `Esc` fecha a janela.

### 4.1 Carregamento de Imagens
- **Selecionar Arquivos:** escolhe um ou mais arquivos `.jpg`/`.jpeg`.
- **Selecionar Pasta:** carrega todas as fotos de uma pasta, incluindo subpastas.
- **Arrastar e soltar:** fotos arrastadas para a janela entram na fila.
- **Fila:** clique em uma foto para visualizá-la; o `×` a remove. **Limpar Fila** esvazia a lista e libera a memória.
- **Duplicados:** fotos com o mesmo caminho, tamanho e data de modificação são ignoradas. Arquivos de mesmo nome em subpastas diferentes (ex.: `DJI_0001.JPG` de voos distintos) são mantidos.
- **Telemetria:** o painel mostra GPS, altitude, altura relativa, azimute e sua fonte, inclinação do gimbal, data/hora e câmera da foto selecionada.
- Durante o processamento em lote a fila fica bloqueada.

### 4.2 Logotipo Institucional
- **Logotipo padrão:** o app abre com o logotipo oficial do IBAMA, embutido no próprio HTML em base64 com os mesmos pixels do arquivo oficial. Por ser um logotipo oficial, ele é **sempre reproduzido sem alteração de cores ou formas**, apenas redimensionado proporcionalmente.
- **Carregar Logotipo:** substitui o logo por um arquivo `.png` (transparência recomendada) ou `.jpg`.
- **Remover Logotipo** retira o logo da estampa; **Usar logotipo padrão (IBAMA)** o restaura.
- **Posição** (6 opções) e **Tamanho** (% da menor dimensão da foto).
- **Aparência:** fundo e transparência seguem o padrão do card 5 ou um ajuste individual (seção 4.5).

### 4.3 Bússola Gráfica Vetorial
- **Exibir Bússola** e **Mostrar Ângulo (°)**: o selo de ângulo mostra o azimute e o rumo (ex.: `142° Sudeste`).
- **Posição** (6 opções) e **Tamanho** (% da menor dimensão da foto).
- **Modo da Bússola:**
  - *Automático:* Seta Norte quando a foto DJI foi tirada com o gimbal apontado para baixo (`GimbalPitchDegree` ≤ -80°); Rumo da Câmera nos demais casos.
  - *Seta Norte:* para fotos vistas de cima, cujo topo aponta para o rumo da tomada. A rosa dos ventos gira e a ponta vermelha indica o Norte real (ex.: rumo 90° → Norte à esquerda).
  - *Rumo da Câmera:* o "N" fica fixo no topo do mostrador e a ponta vermelha indica para onde a câmera apontava.
- **Aparência:** fundo e transparência (seção 4.5). O selo de ângulo acompanha o fundo da bússola.
- A bússola só é desenhada em fotos que têm azimute.

### 4.4 Conteúdo da Faixa de Texto
- **Texto Superior:** título livre (projeto, obra, lote, tipo de vistoria). Quando vazio, a linha de título não é estampada.
- **Modelo do Drone / Equipamento:** em modo automático, cada foto recebe o equipamento do próprio EXIF (lotes com drones diferentes ficam corretos). Um texto digitado passa a valer para todas as fotos; apagar o campo volta ao modo automático.
- **Itens exibidos:** Coordenadas GPS (graus decimais, 6 casas, com a altitude), Azimute no Texto, Equipamento, **Data** (`DD/MM/AAAA`) e **Hora** (`HH:MM`). Data e hora são independentes: é possível exibir as duas, só uma ou nenhuma.
- **Altitude Estampada:** absoluta (`ALT`) ou relativa à decolagem (`ALT REL`). Se a foto não tiver altura relativa, a absoluta é usada.
- **Posicionamento da Faixa** (6 opções) e **Aparência** (seção 4.5).

### 4.5 Estilo Visual, Fundo e Transparência
**Presets rápidos** (alteram apenas o padrão global):
- **Azul IBAMA** (padrão): fundo azul-marinho do logotipo (`#15306E`) com texto branco.
- **Light Clean:** fundo branco translúcido com texto azul-marinho.
- **Segurança:** fundo preto com texto amarelo, para laudos e segurança do trabalho.

**Cor do Texto** e **Tamanho da Fonte** completam o estilo.

**Fundo e Transparência — padrão para todos os elementos:**
- **Exibir fundo** e **cor do fundo:** liga ou retira a faixa do texto, o disco e o selo da bússola e a caixa do logotipo.
- **Opacidade do Fundo:** 0% a 100%.
- **Opacidade dos Elementos:** transparência do conteúdo (letras do texto; borda, letras e agulha da bússola; imagem do logotipo).
- **Contorno de Legibilidade:** contorno na cor de contraste ao redor das letras. *Automático* (padrão) quando o elemento está sem fundo ou com fundo abaixo de 30%; também *Sempre* ou *Nunca*.

**Ajuste individual:** cada card (logotipo, bússola e texto) tem o bloco **Aparência**. Marcando **Ajuste individual**, o elemento passa a ter fundo, cor do fundo, opacidade do fundo e opacidade do conteúdo próprios (e cor do texto, no caso da faixa), partindo dos valores globais do momento. **Voltar ao padrão** desfaz o ajuste. Os presets preservam os ajustes individuais.

### 4.6 Visualizador (Zoom e Arrasto)
- **Roda do mouse:** amplia ou reduz.
- **Arrastar com o botão esquerdo:** navega pela foto.
- **Barra de ferramentas:** `−`, `+` e **Ajustar** (enquadra a foto na tela).
- **Atalhos:** `F1` abre a ajuda; `Esc` a fecha.

### 4.7 Exportação
- **Baixar Imagem Atual (JPEG):** exporta a foto selecionada em resolução total, qualidade 95%.
- **Processar e Baixar Todas (ZIP):** processa a fila com barra de progresso e gera `geotag_batch_[data].zip`.
  - Lotes acima de 2 GB são divididos em `..._parte1.zip`, `..._parte2.zip` etc. (o navegador pode pedir permissão para vários downloads).
  - As subpastas de origem são preservadas e nomes repetidos recebem sufixo (`geotag_DJI_0001_2.JPG`).
  - Fotos que falharem são ignoradas sem interromper o lote e informadas ao final.
- **Destino do Lote (Chrome e Edge):** *Baixar arquivo ZIP* ou *Gravar direto em uma pasta*. Na gravação em pasta, cada JPEG vai para o disco assim que é gerado — a opção mais indicada para lotes grandes. Subpastas são recriadas e arquivos existentes nunca são sobrescritos (sufixos `_2`, `_3`...).
- **Preservar metadados originais (EXIF/GPS/XMP):** ativado por padrão. Copia os blocos EXIF e XMP da foto original para o JPEG exportado; a tag `Orientation` passa a 1 (a rotação já está nos pixels) e a miniatura EXIF antiga, sem a estampa, deixa de ser referenciada.
- **Cancelar Processamento:** interrompe o lote com segurança.

### 4.8 Configurações Padrão
| Item | Padrão |
| :--- | :--- |
| Logotipo | IBAMA, superior esquerdo, 15% |
| Bússola | Superior direita, 15%, modo automático, sem selo de ângulo |
| Faixa de texto | Centro inferior, sem texto superior |
| Itens da faixa | GPS, equipamento, data e hora ligados; azimute desligado |
| Altitude | Relativa à decolagem (`ALT REL`) |
| Estilo | Preset Azul IBAMA: fundo `#15306E` a 80%, texto branco |
| Contorno de legibilidade | Automático |
| Exportação | ZIP, metadados preservados, JPEG 95% |

---

## 5. Engenharia de Metadados (EXIF e XMP DJI)

### Coordenadas GPS
As coordenadas em graus, minutos e segundos são convertidas para graus decimais:
$$\text{DD} = \text{Graus} + \frac{\text{Minutos}}{60} + \frac{\text{Segundos}}{3600}$$
com sinal negativo para os hemisférios Sul (`S`) e Oeste (`W`).

### Altitude
- **Absoluta:** `GPSAltitude` do EXIF, negativa quando `GPSAltitudeRef = 1` (abaixo do nível do mar); na ausência, `drone-dji:AbsoluteAltitude`. A referência vertical depende do equipamento — a DJI não garante altitude ortométrica.
- **Relativa:** `drone-dji:RelativeAltitude`, altura de voo em relação ao ponto de decolagem.

### Data e Hora
`DateTimeOriginal`, `DateTime` ou `GPSDateStamp` (somente data), convertidos de `AAAA:MM:DD HH:MM:SS` para `DD/MM/AAAA` e `HH:MM`. Não há conversão de fuso: a hora é a registrada pelo equipamento.

### Prioridade do Azimute
1. **`drone-dji:GimbalYawDegree`** (XMP): direção da câmera no gimbal.
2. **`GPSImgDirection`** (EXIF): direção de mira da câmera.
3. **`drone-dji:FlightYawDegree`** (XMP): rumo da aeronave.
4. **`GPSTrack`** / **`GPSDestBearing`** (EXIF): rumo de deslocamento.

### Inclinação do Gimbal
`drone-dji:GimbalPitchDegree`: -90° é a câmera apontada para baixo. Valores ≤ -80° caracterizam foto vertical, usada pelo modo automático da bússola.

### Rumos em Português
O ângulo normalizado $\theta \in [0^\circ, 360^\circ)$ é mapeado em 8 setores de 45°:
$$\text{Índice} = \text{round}\left(\frac{\theta}{45^\circ}\right) \bmod 8$$

| Setor | Rumo |
| :---: | :---: |
| $337{,}5^\circ$ a $22{,}5^\circ$ | Norte |
| $22{,}5^\circ$ a $67{,}5^\circ$ | Nordeste |
| $67{,}5^\circ$ a $112{,}5^\circ$ | Leste |
| $112{,}5^\circ$ a $157{,}5^\circ$ | Sudeste |
| $157{,}5^\circ$ a $202{,}5^\circ$ | Sul |
| $202{,}5^\circ$ a $247{,}5^\circ$ | Sudoeste |
| $247{,}5^\circ$ a $292{,}5^\circ$ | Oeste |
| $292{,}5^\circ$ a $337{,}5^\circ$ | Noroeste |

### Geometria da Bússola
No Canvas, 0 rad aponta para a direita; por isso o azimute é girado em -90°:
$$\alpha = (\theta - 90^\circ) \times \frac{\pi}{180}$$
No modo Seta Norte, a rosa dos ventos e a agulha são giradas em $-\theta$, de modo que o "N" aponte para o Norte real dentro da foto.

### Metadados na Exportação
O Canvas gera JPEGs sem metadados. Na exportação, os segmentos APP1 de EXIF e XMP do arquivo original são copiados para o novo JPEG (após o SOI/JFIF). No EXIF copiado, `Orientation` passa a 1 e o ponteiro para o IFD1 (miniatura) é zerado.

---

## 6. Arquitetura do Código

```
geotag_exif_studio.html
├── <head>   Comentário de arquitetura e mapa do código
├── <style>  CSS com variáveis de design (paleta azul do IBAMA: #15306E e #0093DD)
├── <body>   Cabeçalho (com botão Ajuda), painel de controle (cards 1 a 5),
│            visualizador, janela de ajuda e rodapé
├── <script> EXIF-js 2.3.0 (MIT), embutida e minificada
└── <script> Aplicação (ES6+), em 13 módulos:
     1  Estado global da aplicação
     2  Cache de elementos do DOM
     3  Utilitários e matemática geográfica (DMS → DD, rumos, data/hora)
     4  Fila de imagens
     5  Extração de metadados (EXIF + XMP DJI)
     6  Carregamento da imagem ativa e logotipo padrão (base64)
     7  Aparência (global e individual), presets e sincronização dos controles
     8  Bússola vetorial (Seta Norte / Rumo da Câmera)
     9  Composição da estampa no Canvas
     10 Zoom e arrasto do visualizador
     11 Arrastar e soltar arquivos
     12 Exportação: JPEG, cópia de metadados, gerador de ZIP e gravação em pasta
     13 Ajuda (guia de uso embutido)
```

---

## 7. Desempenho e Memória

* **Pré-visualização leve:** com a foto inteira na tela, a prévia usa no máximo 2560 px de lado; ao ampliar, a resolução sobe até a total. Redesenhos são agrupados por quadro de tela.
* **Exportação em resolução total:** sempre renderizada num Canvas próprio, independente da prévia.
* **Leitura parcial de metadados:** EXIF e XMP são procurados nos primeiros 256 KB de cada arquivo, com tempo limite, para que um arquivo corrompido não trave a fila.
* **Object URLs efêmeros:** cada foto do lote é liberada da memória logo após o processamento.
* **ZIP sem recompressão:** JPEG já é comprimido; os arquivos são armazenados pelo método STORE e o pacote é montado como um Blob composto pelas próprias fotos, sem cópias em memória (apenas o CRC-32 é calculado).
* **Gravação em pasta:** cada foto vai direto para o disco, sem acúmulo no navegador.

---

## 8. Limitações Conhecidas
* **Logotipo do IBAMA sobre fundo azul:** como o logotipo não pode ser alterado, as letras "MMA" (quase pretas) têm pouco contraste sobre o fundo azul-marinho padrão. Para destacá-las, use um fundo claro (preset Light Clean) ou um ajuste individual de fundo claro no card do logotipo.
* **Lotes muito grandes em ZIP:** as fotos processadas ficam guardadas pelo navegador até o download de cada parte; o navegador pode precisar de espaço temporário equivalente ao tamanho do lote. Para lotes grandes, prefira a gravação em pasta.
* **Perfis de cor ICC** da foto original não são copiados (o Canvas exporta em sRGB).
* **Celulares e tablets:** a interface foi pensada para telas de computador (largura mínima recomendada de cerca de 1100 px).

---

## 9. Histórico de Versões
As mudanças de cada versão estão em [`CHANGELOG.md`](CHANGELOG.md).

---

## 10. Autor e Licença

* **Desenvolvedor:** Kuriakin Humberto Toscan
* **GitHub:** [https://github.com/KuriakinToscan](https://github.com/KuriakinToscan)
* **Licença do código:** [MIT](LICENSE)
* **EXIF-js 2.3.0:** licença MIT, © Jacob Seidelin.
* **Logotipo do IBAMA:** marca oficial do Instituto Brasileiro do Meio Ambiente e dos Recursos Naturais Renováveis. Não é coberto pela licença MIT deste projeto; seu uso deve seguir as normas do órgão.

> *Este software é fornecido "como está", sob os termos da licença MIT, livre para uso pessoal, comercial ou institucional.*
