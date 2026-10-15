# Geotag & EXIF Studio — Versão 1.1

> **Estamparia Visual de Telemetria, Bússola Vetorial e Processamento em Lote para Fotos Aéreas e de Campo.**  
> Aplicação em Arquivo Único • 100% Offline • Processamento Local Seguro • Compatível com EXIF Padrão e Metadados DJI XMP.

---

## 📌 Sumário
- [1. Visão Geral](#1-visão-geral)
- [2. Principais Funcionalidades](#2-principais-funcionalidades)
- [3. Arquitetura e Engenharia de Software](#3-arquitetura-e-engenharia-de-software)
- [4. Requisitos e Como Executar](#4-requisitos-e-como-executar)
- [5. Guia de Uso Passo a Passo](#5-guia-de-uso-passo-a-passo)
  - [5.1 Carregamento de Imagens](#51-carregamento-de-imagens)
  - [5.2 Bússola Gráfica Vetorial](#52-bússola-gráfica-vetorial)
  - [5.3 Conteúdo e Telemetria Estampada](#53-conteúdo-e-telemetria-estampada)
  - [5.4 Logotipo Institucional](#54-logotipo-institucional)
  - [5.5 Estilização e Presets Rápidos](#55-estilização-e-presets-rápidos)
  - [5.6 Visualizador Interativo (Zoom & Pan)](#56-visualizador-interativo-zoom--pan)
  - [5.7 Exportação e Empacotamento ZIP](#57-exportação-e-empacotamento-zip)
- [6. Engenharia de Metadados (EXIF & DJI XMP)](#6-engenharia-de-metadados-exif--dji-xmp)
- [7. Desempenho e Gerenciamento de Memória](#7-desempenho-e-gerenciamento-de-memória)
- [8. Autor e Licença](#8-autor-e-licença)

---

## 1. Visão Geral

O **Geotag & EXIF Studio** é uma ferramenta profissional de engenharia desenvolvida para estampar automaticamente parâmetros técnicos (coordenadas geográficas GPS, altitude absoluta ou relativa à decolagem, azimute da tomada, data/hora e identificação do equipamento) diretamente sobre fotografias aéreas obtidas por drones (DJI, Autel, etc.) ou levantamentos de campo.

A aplicação opera sob o paradigma **Single-File Autonomous** (arquivo único e autônomo): todo o código HTML5, estilos CSS nativos e lógica JavaScript estão consolidados no arquivo [`geotag_exif_studio.html`](geotag_exif_studio.html), sem nenhuma dependência de conexões com servidores externos ou instaladores complexos.

---

## 2. Principais Funcionalidades

* 🌐 **100% Offline & Privacidade Absoluta:**
  Nenhum byte de imagem ou metadado trafega pela internet. Todas as operações de leitura binária, decodificação, renderização gráfica e compressão ocorrem localmente no navegador via memória RAM.
* 🧭 **Bússola Gráfica Vetorial Integrada:**
  Renderização procedural em Canvas 2D da orientação azimutal da câmera com agulha magnética bicolor (Norte em vermelho internacional e Sul em branco neutro), mostrador graduado, pontos cardeais em português (`N`, `L`, `S`, `O`) e selo numérico de graus.
* 🧭 **Seta Norte ou Rumo da Câmera:**
  Em fotos verticais (vistas de cima) a bússola gira para indicar onde está o Norte real dentro da imagem; em fotos oblíquas ou de solo o ponteiro indica a direção da tomada. No modo automático a escolha é feita pela inclinação do gimbal DJI (`GimbalPitchDegree`).
* 🗂️ **Metadados Preservados na Exportação:**
  Os JPEGs exportados mantêm o EXIF original (GPS, data, câmera) e o pacote XMP da DJI, podendo ser reimportados em SIG, Google Earth ou softwares de fotogrametria.
* 📍 **Posicionamento em 6 Quadrantes:**
  Liberdade total para posicionar a bússola em qualquer canto ou centro da fotografia: *Superior Esquerda*, *Superior Centro*, *Superior Direita*, *Inferior Esquerda*, *Inferior Centro* ou *Inferior Direita*.
* 🛰️ **Suporte Avançado a Drones DJI (EXIF + XMP):**
  Extração de ângulo de rotação do gimbal (`GimbalYawDegree`), rumo da fuselagem (`FlightYawDegree`) altitude absoluta (`AbsoluteAltitude`) e altura de voo relativa à decolagem (`RelativeAltitude`), convertendo valores contínuos para os 8 rumos cardeais/colaterais em português (`Norte`, `Nordeste`, `Leste`, `Sudeste`, `Sul`, `Sudoeste`, `Oeste`, `Noroeste`).
* 🏷️ **Faixa de Texto Customizável:**
  Faixa semitransparente com cantos arredondados contendo dados organizados em camadas: identificação de projeto/obra, telemetria (LAT, LON, ALT, DIR), equipamento e carimbo de data/hora.
* 🛡️ **Logotipo / Marca d'Água:**
  Suporte a logotipos corporativos em formato PNG transparente ou JPEG, com escala proporcional e controle de opacidade da caixa de fundo.
* ⚡ **Processamento em Lote de Alto Desempenho:**
  Capacidade de processar centenas de fotos sequencialmente, com acompanhamento de progresso em tempo real, opção de interrupção imediata e empacotamento em arquivo `.zip` gerado localmente (dividido em partes de até 2 GB em lotes muito grandes).
* 🔍 **Viewport com Zoom & Pan Suave:**
  Inspeção detalhada de pixels em alta resolução com navegação por arrasto do mouse e zoom responsivo (10% até 500%).

---

## 3. Arquitetura e Engenharia de Software

```
┌────────────────────────────────────────────────────────────────────────┐
│                   Geotag & EXIF Studio (Arquivo Único)                 │
├────────────────────────────────────────────────────────────────────────┤
│  [HTML5 Semântico]                                                     │
│   ├── Cabeçalho Minimalista (Identidade e Versão)                      │
│   ├── Sidebar de Configuração (Cards 1 a 5)                            │
│   ├── Viewport Interativo (HTML5 Canvas 2D + Toolbar de Zoom)          │
│   └── Rodapé Institucional (Autoria e Licença)                         │
├────────────────────────────────────────────────────────────────────────┤
│  [CSS3 Nativo com Tokens de Design]                                    │
│   ├── Variáveis CSS (:root) com paletas HSL/Hex (Slate, Indigo)        │
│   ├── Layouts com CSS Grid e Flexbox Responsivo                        │
│   ├── Filtros de Vidro (Backdrop-filter / Blur)                        │
│   └── Tipografia baseada na System Font Stack (sem chamadas web)       │
├────────────────────────────────────────────────────────────────────────┤
│  [Bibliotecas Integradas Inline]                                       │
│   └── EXIF-js v2.3.0: Parser binário de blocos TIFF/EXIF               │
│  (O gerador de ZIP é próprio, no Módulo 12, sem biblioteca externa)    │
├────────────────────────────────────────────────────────────────────────┤
│  [Módulos JavaScript ES6+]                                             │
│   ├── Módulo 1: Estado Global da Aplicação                             │
│   ├── Módulo 2: Cache de Elementos do DOM                              │
│   ├── Módulo 3: Utilitários e Matemática Geográfica (DMS → DD)         │
│   ├── Módulo 4: Gerenciamento da Fila de Imagens                       │
│   ├── Módulo 5: Extrator Unificado EXIF + XMP DJI                      │
│   ├── Módulo 6: Carregamento da Imagem Ativa                           │
│   ├── Módulo 7: Presets Visuais e Sincronização dos Controles          │
│   ├── Módulo 8: Motor Vetorial de Bússola 2D                           │
│   ├── Módulo 9: Compositor Gráfico da Estampa                          │
│   ├── Módulo 10: Navegação no Viewport (Zoom/Pan)                      │
│   ├── Módulo 11: Drag & Drop e Ingestão de Arquivos                    │
│   └── Módulo 12: Exportação Individual e em Lote (ZIP)                 │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Requisitos e Como Executar

### Requisitos Mínimos
- Qualquer navegador web moderno com suporte a HTML5 Canvas (Google Chrome, Microsoft Edge, Mozilla Firefox, Opera, Brave ou Safari).
- **Nenhum** servidor local, Node.js, Python ou conexão de internet é necessário.

### Como Abrir
1. Baixe ou clone este repositório (ou apenas o arquivo `geotag_exif_studio.html`).
2. Dê um duplo clique no arquivo [`geotag_exif_studio.html`](geotag_exif_studio.html).
3. O aplicativo abrirá instantaneamente pronto para uso.

---

## 5. Guia de Uso Passo a Passo

### 5.1 Carregamento de Imagens
- **Selecionar Arquivos:** Permite escolher múltiplos arquivos `.jpg` ou `.jpeg` individualmente.
- **Selecionar Pasta:** Carrega recursivamente todas as fotos contidas em um diretório do computador.
- **Arrastar e Soltar (Drag & Drop):** Arraste uma seleção de imagens diretamente sobre a janela do aplicativo.
- **Limpar Fila:** Esvazia a lista, libera a memória RAM dos objetos em cache e reseta os seletores.
- **Duplicados:** Fotos com o mesmo caminho, tamanho e data de modificação são ignoradas. Arquivos de mesmo nome em subpastas diferentes (ex: `DJI_0001.JPG` de voos distintos) são mantidos.
- Durante o processamento em lote a fila fica bloqueada (não é possível adicionar, remover ou limpar fotos).

### 5.2 Bússola Gráfica Vetorial
- **Exibir Bússola:** Ativa ou oculta a bússola vetorial na imagem.
- **Mostrar Ângulo (°):** Adiciona uma etiqueta abaixo da bússola com a indicação precisa do azimute e rumo (ex: `142° Sudeste`). Desabilitado por padrão.
- **Modo da Bússola:**
  - *Automático* (padrão): usa a Seta Norte quando a foto DJI foi tirada com o gimbal apontado para baixo (`GimbalPitchDegree` ≤ -80°) e o Rumo da Câmera nos demais casos.
  - *Seta Norte*: para fotos vistas de cima, cujo topo aponta para o rumo da tomada. A rosa dos ventos gira e a ponta vermelha indica o Norte real na imagem (ex: rumo 90° → Norte à esquerda).
  - *Rumo da Câmera*: o "N" fica fixo no topo do mostrador e a ponta vermelha indica o azimute para onde a câmera apontava.
- **Posição:** Escolha entre os 6 quadrantes:
  - *Superior Esquerda*
  - *Superior Centro*
  - *Superior Direita* (Padrão)
  - *Inferior Esquerda*
  - *Inferior Centro*
  - *Inferior Direita*
- **Tamanho:** Ajuste o diâmetro da bússola (% proporcional à resolução).
- **Aparência:** Fundo e transparência seguem o padrão do card 5 ou um ajuste individual (veja a seção 5.5). O selo de graus acompanha o fundo da bússola.

### 5.3 Conteúdo e Telemetria Estampada
- **Texto Superior:** Campo livre para nome de projeto, cliente, número do lote ou tipo de vistoria (ex: `Vistoria Técnica • Obra 12`). Vem em branco por padrão; quando vazio, a linha de título não é estampada.
- **Modelo do Drone:** Em modo automático, cada foto recebe o equipamento indicado no seu próprio EXIF (lotes com drones diferentes ficam corretos). Ao digitar um texto, ele passa a ser usado em todas as fotos; apagar o campo volta ao modo automático.
- **Altitude Estampada:** Escolha entre a altitude absoluta (EXIF GPS ou `AbsoluteAltitude` da DJI) e a altura relativa à decolagem (`RelativeAltitude` da DJI, estampada como `ALT REL`). Se a foto não tiver altura relativa, a absoluta é usada. O padrão é a altura relativa.
- **Toggles de Informação:** Marque ou desmarque a exibição de:
  - Coordenadas GPS (Latitude e Longitude em graus decimais com 6 dígitos de precisão).
  - Altitude em metros (exibida junto às coordenadas GPS).
  - Azimute no texto (direção angular da tomada). Desabilitado por padrão.
  - Identificação do equipamento.
  - Data e hora do disparo da foto.

### Configurações Padrão
| Item | Padrão |
| :--- | :--- |
| Bússola | Superior direita, sem o selo de ângulo |
| Faixa de texto | Centro inferior, sem texto superior |
| Azimute no texto | Desabilitado |
| Altitude estampada | Relativa à decolagem (`ALT REL`) |
| Logotipo | IBAMA, superior esquerdo, mesmo fundo dos demais elementos |

### 5.4 Logotipo Institucional
- **Logotipo padrão:** o app abre com o logotipo do IBAMA embutido no próprio HTML (base64, sem arquivo externo), no canto superior esquerdo, com o mesmo fundo da bússola e da faixa de texto. Por ser um logotipo oficial, ele é sempre reproduzido sem nenhuma alteração de cores ou formas. **Remover Logotipo** retira o logo; **Usar logotipo padrão (IBAMA)** o restaura.
- Clique em **Carregar Logotipo** e selecione um arquivo `.png` (com transparência recomendada) ou `.jpg`.
- Defina o quadrante de exibição do logo e a escala proporcional.
- **Aparência:** Fundo e transparência seguem o padrão do card 5 ou um ajuste individual. A opacidade do logotipo permite usá-lo como marca d'água translúcida.

### 5.5 Estilização e Presets Rápidos
Três presets profissionais estão disponíveis com um clique:
- 🌑 **Dark Pro:** Fundo grafite profundo com tipografia branca de alto contraste.
- ☀️ **Light Clean:** Fundo branco semitransparente com texto azul petróleo e estética limpa.
- ⚠️ **Segurança:** Fundo preto opaco com texto amarelo internacional (padrão de segurança do trabalho e laudos periciais).
- **Controles Personalizados:** Cor do texto e tamanho da fonte.

#### Fundo e Transparência
O bloco **Fundo e Transparência** do card 5 é o padrão aplicado ao texto, à bússola e ao logotipo de uma só vez:
- **Exibir fundo** e **Cor do fundo**: liga ou retira o fundo (faixa do texto, disco e selo da bússola, caixa do logo).
- **Opacidade do Fundo**: transparência do fundo, de 0% a 100%.
- **Opacidade dos Elementos**: transparência do conteúdo (letras do texto; borda, letras e agulha da bússola; imagem do logo).
- **Contorno de Legibilidade**: contorno na cor de contraste ao redor das letras. No modo *Automático* (padrão) ele aparece quando o elemento está sem fundo ou com fundo abaixo de 30%; também pode ser *Sempre* ou *Nunca*.

Cada elemento tem, no próprio card, o bloco **Aparência** com a opção **Ajuste individual**. Marcada, ela libera fundo, cor do fundo, opacidade do fundo e opacidade do conteúdo próprios (e a cor do texto, no caso da faixa), começando pelos valores globais do momento. **Voltar ao padrão** desfaz o ajuste. Os presets alteram apenas o padrão global; ajustes individuais são mantidos.

### 5.6 Visualizador Interativo (Zoom & Pan)
- **Roda do Mouse (Scroll):** Amplie ou reduza o zoom dinamicamente.
- **Arrasto (Pan):** Clique com o botão esquerdo e arraste para navegar em qualquer quadrante da foto em alta definição.
- **Toolbar Superior:** Botões rápidos de zoom in (`+`), zoom out (`-`) e `Ajustar` (restaura a imagem perfeitamente enquadrada na tela).

### 5.7 Exportação e Empacotamento ZIP
- **Baixar Imagem Atual (JPEG):** Renderiza e salva imediatamente a foto selecionada com qualidade de 95%.
- **Processar e Baixar Todas (ZIP):** Inicia a fila automatizada. A barra de progresso exibe a foto atual e a porcentagem concluída. Ao finalizar, o arquivo compactado `geotag_batch_[timestamp].zip` é descarregado automaticamente, com a mesma qualidade JPEG (95%) do download individual.
  - Lotes que ultrapassariam 2 GB são divididos em vários arquivos (`geotag_batch_[timestamp]_parte1.zip`, `_parte2.zip`, ...). O navegador pode pedir permissão para baixar vários arquivos.
  - As subpastas de origem são preservadas dentro do ZIP e nomes repetidos recebem sufixo numérico (`geotag_DJI_0001_2.JPG`), sem sobrescrever arquivos.
  - Fotos que não puderem ser processadas são ignoradas sem interromper o lote e informadas ao final.
- **Destino do Lote (Chrome e Edge):** Além do ZIP, é possível gravar as fotos diretamente em uma pasta escolhida. Cada JPEG vai para o disco assim que é gerado, sem ZIP e sem acumular dados no navegador, o que é o mais indicado para lotes grandes. As subpastas de origem são recriadas e arquivos já existentes na pasta nunca são sobrescritos (a nova foto recebe sufixo `_2`, `_3`...). Em navegadores sem suporte a essa função, a opção não aparece.
- **Preservar metadados originais (EXIF/GPS/XMP):** Ativado por padrão. Copia os blocos EXIF e XMP da foto original para o JPEG exportado. A tag `Orientation` é ajustada para 1 (a rotação já está aplicada nos pixels) e a miniatura EXIF antiga, sem a estampa, deixa de ser referenciada.
- **Cancelar Processamento:** Permite interromper o processamento em lote a qualquer momento de forma segura.

---

## 6. Engenharia de Metadados (EXIF & DJI XMP)

### Coordenadas GPS
As coordenadas brutas sexagesimais (Graus, Minutos e Segundos) são convertidas para graus decimais através da fórmula matemática:
$$\text{DD} = \text{Graus} + \frac{\text{Minutos}}{60} + \frac{\text{Segundos}}{3600}$$
*(Aplicando sinal negativo se o hemisfério for Sul `S` ou Oeste `W`)*.

### Altitude
- **Absoluta:** `GPSAltitude` do EXIF, negativa quando `GPSAltitudeRef = 1` (abaixo do nível do mar). Na ausência dela, usa-se `drone-dji:AbsoluteAltitude` do XMP. A referência vertical depende do equipamento; a DJI não garante altitude ortométrica.
- **Relativa:** `drone-dji:RelativeAltitude`, altura de voo em relação ao ponto de decolagem. Selecionável no card 3 e estampada como `ALT REL`.

### Cascata de Extração de Azimute
Para garantir que a orientação magnética seja detectada com a máxima fidelidade, o aplicativo implementa uma cadeia de prioridade em cascata:
1. **`drone-dji:GimbalYawDegree`** (Pacote XMP): Orientação real em graus da câmera motorizada do gimbal.
2. **`GPSImgDirection`** (Tag EXIF padrão): Direção de mira da câmera.
3. **`drone-dji:FlightYawDegree`** (Pacote XMP): Rumo da proa da aeronave.
4. **`GPSTrack` / `GPSDestBearing`** (Tags EXIF): Rumo de trajetória do GPS.

### Inclinação do Gimbal
`drone-dji:GimbalPitchDegree` indica a inclinação da câmera: -90° é a câmera apontada para baixo. Valores ≤ -80° caracterizam foto vertical, usada pelo modo automático da bússola.

### Mapeamento dos 8 Rumos Cardeais em Português
O ângulo normalizado $\theta \in [0^\circ, 360^\circ)$ é mapeado no setor correspondente:
$$\text{Índice} = \text{round}\left(\frac{\theta}{45^\circ}\right) \pmod 8$$

| Setor Angular | Ponto Cardeal / Colateral | Sigla |
| :---: | :---: | :---: |
| $337.5^\circ \text{ a } 22.5^\circ$ | **Norte** | `N` |
| $22.5^\circ \text{ a } 67.5^\circ$ | **Nordeste** | `NE` |
| $67.5^\circ \text{ a } 112.5^\circ$ | **Leste** | `L` |
| $112.5^\circ \text{ a } 157.5^\circ$ | **Sudeste** | `SE` |
| $157.5^\circ \text{ a } 202.5^\circ$ | **Sul** | `S` |
| $202.5^\circ \text{ a } 247.5^\circ$ | **Sudoeste** | `SO` |
| $247.5^\circ \text{ a } 292.5^\circ$ | **Oeste** | `O` |
| $292.5^\circ \text{ a } 337.5^\circ$ | **Noroeste** | `NO` |

### Trigonometria da Bússola Vetorial no Canvas
No Canvas 2D, o ângulo $0$ radianos aponta para a direita ($+X$). Como o Norte na bússola aponta para cima ($12\text{h}$), é feita a rotação de fase de $-90^\circ$:
$$\alpha = (\text{normAngle} - 90^\circ) \times \frac{\pi}{180}$$
A largura da agulha magnética é calculada aplicando o vetor ortogonal $(-\sin\alpha, \cos\alpha)$, garantindo desenho simétrico e preciso em qualquer resolução de sensor.

---

## 7. Desempenho e Gerenciamento de Memória

Processar dezenas ou centenas de fotografias de 20 a 50 Megapixels pode saturar rapidamente a memória RAM do navegador se os recursos não forem reciclados:
* **Object URLs Efêmeros:** Cada imagem carregada para o processador é instanciada via `URL.createObjectURL(file)` e imediatamente descartada com `URL.revokeObjectURL(url)` assim que o Blob é inserido no arquivo ZIP.
* **Pré-visualização em Resolução Reduzida:** Com a foto inteira na tela, a prévia é desenhada com no máximo 2560 px de lado; ao ampliar, a resolução sobe até a total. A exportação é sempre renderizada em resolução total.
* **Canvas Offscreen Reutilizável:** Durante o lote, um único elemento Canvas oculto é reaproveitado para todas as renderizações, evitando vazamento de contexto WebGL/2D.
* **Leitura Parcial de Metadados:** Os blocos EXIF e XMP são procurados nos primeiros 256 KB de cada arquivo; a foto inteira só é lida para metadados se nada for encontrado nesse trecho. A leitura tem tempo limite, de modo que um arquivo corrompido não trava a fila.
* **ZIP sem Recompressão:** JPEG já é comprimido, então os arquivos são armazenados no ZIP pelo método STORE, sem gastar CPU com DEFLATE. O pacote é montado como um Blob composto pelas próprias fotos, sem copiar os dados para a memória do JavaScript; apenas o CRC-32 de cada arquivo é calculado.

### Limitações Conhecidas
* As fotos processadas ficam guardadas pelo navegador até o download de cada parte do ZIP; em lotes muito grandes, o navegador pode precisar de espaço em disco temporário equivalente ao tamanho do lote.
* Perfis de cor ICC da foto original não são copiados (o Canvas exporta em sRGB).

---

## 8. Autor e Licença

* **Desenvolvedor:** Kuriakin Humberto Toscan
* **Repositório GitHub:** [https://github.com/KuriakinToscan](https://github.com/KuriakinToscan)
* **Licença:** [MIT License](https://opensource.org/licenses/MIT)

> *Este software é fornecido "como está", sob os termos da licença livre e de código aberto MIT, sendo livre para uso pessoal, comercial ou institucional.*
