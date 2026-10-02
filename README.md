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

O **Geotag & EXIF Studio** é uma ferramenta profissional de engenharia desenvolvida para estampar automaticamente parâmetros técnicos (coordenadas geográficas GPS, altitude ortométrica/absoluta, azimute da tomada, data/hora e identificação do equipamento) diretamente sobre fotografias aéreas obtidas por drones (DJI, Autel, etc.) ou levantamentos de campo.

A aplicação opera sob o paradigma **Single-File Autonomous** (arquivo único e autônomo): todo o código HTML5, estilos CSS nativos e lógica JavaScript estão consolidados no arquivo [`geotag_exif_studio.html`](file:///d:/Exif_Studio/geotag_exif_studio.html), sem nenhuma dependência de conexões com servidores externos ou instaladores complexos.

---

## 2. Principais Funcionalidades

* 🌐 **100% Offline & Privacidade Absoluta:**
  Nenhum byte de imagem ou metadado trafega pela internet. Todas as operações de leitura binária, decodificação, renderização gráfica e compressão ocorrem localmente no navegador via memória RAM.
* 🧭 **Bússola Gráfica Vetorial Integrada:**
  Renderização procedural em Canvas 2D da orientação azimutal da câmera com agulha magnética bicolor (Norte em vermelho internacional e Sul em branco neutro), mostrador graduado, pontos cardeais em português (`N`, `L`, `S`, `O`) e selo numérico de graus.
* 📍 **Posicionamento em 6 Quadrantes:**
  Liberdade total para posicionar a bússola em qualquer canto ou centro da fotografia: *Superior Esquerda*, *Superior Centro*, *Superior Direita*, *Inferior Esquerda*, *Inferior Centro* ou *Inferior Direita*.
* 🛰️ **Suporte Avançado a Drones DJI (EXIF + XMP):**
  Extração de ângulo de rotação do gimbal (`GimbalYawDegree`), rumo da fuselagem (`FlightYawDegree`) e altitude de voo (`AbsoluteAltitude`), convertendo valores contínuos para os 8 rumos cardeais/colaterais em português (`Norte`, `Nordeste`, `Leste`, `Sudeste`, `Sul`, `Sudoeste`, `Oeste`, `Noroeste`).
* 🏷️ **Faixa de Texto Customizável:**
  Faixa semitransparente com cantos arredondados contendo dados organizados em camadas: identificação de projeto/obra, telemetria (LAT, LON, ALT, DIR), equipamento e carimbo de data/hora.
* 🛡️ **Logotipo / Marca d'Água:**
  Suporte a logotipos corporativos em formato PNG transparente ou JPEG, com escala proporcional e controle de opacidade.
* ⚡ **Processamento em Lote de Alto Desempenho:**
  Capacidade de processar centenas de fotos sequencialmente, com acompanhamento de progresso em tempo real, opção de interrupção imediata e empacotamento compactado em arquivo `.zip` via motor DEFLATE.
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
│   ├── EXIF-js v2.3.0: Parser binário de blocos TIFF/EXIF               │
│   └── JSZip v3.10.1: Criação e compressão assíncrona de arquivos ZIP   │
├────────────────────────────────────────────────────────────────────────┤
│  [Módulos JavaScript ES6+]                                             │
│   ├── Módulo 1: Estado Global Reativo (Single Source of Truth)         │
│   ├── Módulo 2: Cache de Elementos do DOM                              │
│   ├── Módulo 3: Matemática Geográfica (DMS → DD, Ângulo Normalizado)   │
│   ├── Módulo 4: Gerenciamento da Fila & Prevenção de Memory Leak       │
│   ├── Módulo 5: Extrator Unificado EXIF + XMP DJI                      │
│   ├── Módulo 6: Motor Vetorial de Bússola 2D (Trigonometria Polar)     │
│   ├── Módulo 7: Presets Visuais (Dark Pro, Light Clean, Segurança)     │
│   ├── Módulo 8: Compositor Gráfico de Alta Fidelidade                  │
│   ├── Módulo 9: Motor de Navegação no Viewport (Zoom/Pan)              │
│   └── Módulo 10: Processador em Lote Assíncrono com Compressão ZIP     │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Requisitos e Como Executar

### Requisitos Mínimos
- Qualquer navegador web moderno com suporte a HTML5 Canvas (Google Chrome, Microsoft Edge, Mozilla Firefox, Opera, Brave ou Safari).
- **Nenhum** servidor local, Node.js, Python ou conexão de internet é necessário.

### Como Abrir
1. Navegue até a pasta do projeto: `d:\Exif_Studio`.
2. Dê um duplo clique no arquivo [`geotag_exif_studio.html`](file:///d:/Exif_Studio/geotag_exif_studio.html).
3. O aplicativo abrirá instantaneamente pronto para uso.

---

## 5. Guia de Uso Passo a Passo

### 5.1 Carregamento de Imagens
- **Selecionar Arquivos:** Permite escolher múltiplos arquivos `.jpg` ou `.jpeg` individualmente.
- **Selecionar Pasta:** Carrega recursivamente todas as fotos contidas em um diretório do computador.
- **Arrastar e Soltar (Drag & Drop):** Arraste uma seleção de imagens diretamente sobre a janela do aplicativo.
- **Limpar Fila:** Esvazia a lista, libera a memória RAM dos objetos em cache e reseta os seletores.

### 5.2 Bússola Gráfica Vetorial
- **Exibir Bússola:** Ativa ou oculta a bússola vetorial na imagem.
- **Mostrar Ângulo (°):** Adiciona uma etiqueta abaixo da bússola com a indicação precisa do azimute e rumo (ex: `142° SE`).
- **Posição:** Escolha entre os 6 quadrantes:
  - *Superior Esquerda*
  - *Superior Centro*
  - *Superior Direita* (Padrão)
  - *Inferior Esquerda*
  - *Inferior Centro*
  - *Inferior Direita*
- **Tamanho e Opacidade:** Ajuste o diâmetro da bússola (% proporcional à resolução) e a transparência do disco de fundo.

### 5.3 Conteúdo e Telemetria Estampada
- **Texto Superior:** Campo livre para nome de projeto, cliente, número do lote ou tipo de vistoria (ex: `Vistoria Técnica • Obra 12`).
- **Modelo do Drone:** Preenchido automaticamente com base nas tags da câmera/drone, ou editável conforme a necessidade.
- **Toggles de Informação:** Marque ou desmarque a exibição de:
  - Coordenadas GPS (Latitude e Longitude em graus decimais com 6 dígitos de precisão).
  - Altitude técnica (em metros).
  - Azimute no texto (direção angular da tomada).
  - Identificação do equipamento.
  - Data e hora do disparo da foto.

### 5.4 Logotipo Institucional
- Clique em **Carregar Logotipo** e selecione um arquivo `.png` (com transparência recomendada) ou `.jpg`.
- Defina o quadrante de exibição do logo, escala proporcional e a opacidade da caixa de fundo.

### 5.5 Estilização e Presets Rápidos
Três presets profissionais estão disponíveis com um clique:
- 🌑 **Dark Pro:** Fundo grafite profundo com tipografia branca de alto contraste.
- ☀️ **Light Clean:** Fundo branco semitransparente com texto azul petróleo e estética limpa.
- ⚠️ **Segurança:** Fundo preto opaco com texto amarelo internacional (padrão de segurança do trabalho e laudos periciais).
- **Controles Personalizados:** Ajuste fino de cores de texto e fundo, tamanho da fonte, opacidade do texto e opacidade da faixa.

### 5.6 Visualizador Interativo (Zoom & Pan)
- **Roda do Mouse (Scroll):** Amplie ou reduza o zoom dinamicamente.
- **Arrasto (Pan):** Clique com o botão esquerdo e arraste para navegar em qualquer quadrante da foto em alta definição.
- **Toolbar Superior:** Botões rápidos de zoom in (`+`), zoom out (`-`) e `Ajustar` (restaura a imagem perfeitamente enquadrada na tela).

### 5.7 Exportação e Empacotamento ZIP
- **Baixar Imagem Atual (JPEG):** Renderiza e salva imediatamente a foto selecionada com qualidade de 95%.
- **Processar e Baixar Todas (ZIP):** Inicia a fila automatizada. A barra de progresso exibe a foto atual e a porcentagem concluída. Ao finalizar, o arquivo compactado `geotag_batch_[timestamp].zip` é descarregado automaticamente.
- **Cancelar Processamento:** Permite interromper o processamento em lote a qualquer momento de forma segura.

---

## 6. Engenharia de Metadados (EXIF & DJI XMP)

### Coordenadas GPS
As coordenadas brutas sexagesimais (Graus, Minutos e Segundos) são convertidas para graus decimais através da fórmula matemática:
$$\text{DD} = \text{Graus} + \frac{\text{Minutos}}{60} + \frac{\text{Segundos}}{3600}$$
*(Aplicando sinal negativo se o hemisfério for Sul `S` ou Oeste `W`)*.

### Cascata de Extração de Azimute
Para garantir que a orientação magnética seja detectada com a máxima fidelidade, o aplicativo implementa uma cadeia de prioridade em cascata:
1. **`drone-dji:GimbalYawDegree`** (Pacote XMP): Orientação real em graus da câmera motorizada do gimbal.
2. **`GPSImgDirection`** (Tag EXIF padrão): Direção de mira da câmera.
3. **`drone-dji:FlightYawDegree`** (Pacote XMP): Rumo da proa da aeronave.
4. **`GPSTrack` / `GPSDestBearing`** (Tags EXIF): Rumo de trajetória do GPS.

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
* **Canvas Offscreen Reutilizável:** Durante o lote, um único elemento Canvas oculto é reaproveitado para todas as renderizações, evitando vazamento de contexto WebGL/2D.
* **Compressão DEFLATE Otimizada:** O JSZip é configurado com compressão de nível 6, equilibrando velocidade de CPU e taxa de compactação.

---

## 8. Autor e Licença

* **Desenvolvedor:** Kuriakin Humberto Toscan
* **Repositório GitHub:** [https://github.com/KuriakinToscan](https://github.com/KuriakinToscan)
* **Licença:** [MIT License](https://opensource.org/licenses/MIT)

> *Este software é fornecido "como está", sob os termos da licença livre e de código aberto MIT, sendo livre para uso pessoal, comercial ou institucional.*
