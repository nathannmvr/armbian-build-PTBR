<div align="center">
  <img src="https://raw.githubusercontent.com/armbian/.github/master/profile/logo.png" alt="Armbian logo" width="25%">
  <br>
  <h1>Armbian para TV Box com SoC Rockchip RK322x</h1>
</div>

Este repositório é um fork do [Armbian Build Framework](https://github.com/armbian/build), o framework oficial para compilação de imagens Armbian.

O objetivo principal deste fork **não é a compilação**, mas sim fornecer uma **documentação consolidada e em português** para a instalação do sistema Armbian em TV Boxes baseadas no SoC Rockchip RK322x (como a MXQ Pro 4K, HK1 Mini, etc.). As instruções foram adaptadas do tópico guia [CSC Armbian for RK322x TV box boards](https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/) no fórum oficial.

---

### ⚠️ **Aviso Importante**

> O procedimento descrito abaixo, especialmente a instalação na memória interna (eMMC/NAND), substitui o sistema Android original e pode **inutilizar permanentemente (brickar)** seu dispositivo se algo der errado (como uma queda de energia ou o uso de uma imagem incompatível).
>
> **Prossiga por sua conta e risco.** Recomenda-se testar o boot pelo cartão SD antes de qualquer instalação permanente.

---

## **Índice**

1.  [**Pré-requisitos**](#-pré-requisitos)
2.  [**Downloads Essenciais**](#-downloads-essenciais)
3.  [**Instruções de Instalação**](#-instruções-de-instalação)
    * [**Método 1: Rodando a partir do Cartão SD**](#método-1-rodando-a-partir-do-cartão-sd-recomendado-para-iniciantes)
    * [**Método 2: Instalação na Memória Interna (eMMC)**](#método-2-instalação-na-memória-interna-emmc)
    * [**Método 3: Instalação na Memória Interna (NAND)**](#método-3-instalação-na-memória-interna-nand)
4.  [**Configuração Pós-Instalação**](#-configuração-pós-instalação)
5.  [**Informações Adicionais**](#-informações-adicionais)
    * [Ordem de Boot](#ordem-de-boot)
    * [Compilando sua Própria Imagem (Avançado)](#compilando-sua-própria-imagem-avançado)
6.  [**Créditos e Projeto Original**](#-créditos-e-projeto-original)

---

### ✅ **Pré-requisitos**

Antes de começar, você precisará de:

* **Uma TV Box** com SoC Rockchip RK3228A ou RK3229.
* **Um cartão MicroSD** de boa qualidade (Classe 10, mínimo de 8 GB).
* **Um computador** com leitor de cartão SD.
* **Software para gravar imagens:** [Balena Etcher](https://www.balena.io/etcher/) ou similar.
* **Fonte de alimentação** da TV Box e cabo HDMI.

---

### 📥 **Downloads Essenciais**

Você precisará de duas imagens principais: a imagem do **Armbian** e a imagem da ferramenta **Multitool**.

1.  **Imagem do Armbian para RK322x:**
    * **Recomendado (Nightly Builds):** Imagens semanais automáticas. Procure pelo `rk322x-box` na lista:
        * [**Nightly Builds no GitHub**](https://github.com/armbian/community/releases)
    * **Imagens de Arquivo (Archive):** Versões mais antigas e estáveis.
        * [**Archive Builds**](https://stpete-mirror.armbian.com/archive/rk322x-box/archive/)

2.  **Imagem do Multitool:**
    * O **Multitool** é uma ferramenta de manutenção essencial para fazer backup, restaurar e, principalmente, gravar a imagem do Armbian na memória interna do seu aparelho.
    * [**Baixe o Multitool aqui**](https://users.armbian.com/jock/web/rk322x/multitool/multitool.img.xz)

---

### ⚙️ **Instruções de Instalação**

Escolha o método que melhor se adapta à sua necessidade. Para os métodos eMMC e NAND você precisa saber o tipo de memória da TV Box, caso não saiba pode ir com o primeiro método ou testar os outros dois e ver qual vai funcionar.

#### **Método 1: Rodando a partir do Cartão SD (Recomendado para iniciantes)**

Este método permite usar o Armbian sem apagar o Android original.

1.  **Prepare o ambiente:** Se sua TV Box ainda tem o Android instalado, você **precisa primeiro apagar a memória interna** para que ela possa dar boot pelo SD.
    * Grave a imagem do **Multitool** em um cartão SD usando o Balena Etcher.
    * Insira o SD na TV Box e ligue-a na energia. A ferramenta irá iniciar.
    * **(Opcional, mas recomendado)** Use a opção `Backup flash` para salvar seu sistema Android.
    * Selecione a opção `Erase flash` e confirme. Ao terminar, desligue a TV Box.

2.  **Grave o Armbian no Cartão SD:** Use o Balena Etcher para gravar a imagem do **Armbian** (que você baixou anteriormente) no mesmo cartão SD.

3.  **Inicie o Sistema:**
    * Insira o cartão SD com Armbian na TV Box.
    * Conecte o cabo de energia.
    * Aguarde. O primeiro boot pode demorar vários minutos para redimensionar o sistema de arquivos. Um LED piscando e imagem no HDMI são bons sinais.

4.  **Configure o Armbian:** Siga as instruções do [**passo de configuração final**](#-configuração-pós-instalação).

#### **Método 2: Instalação na Memória Interna (eMMC)**

Este método apaga o Android e instala o Armbian permanentemente na TV Box.

1.  **Grave o Multitool:** Use o Balena Etcher para gravar a imagem do **Multitool** em seu cartão SD.
2.  **Copie a imagem do Armbian:** Após gravar, o SD terá uma partição visível no seu computador (geralmente NTFS). Copie o arquivo de imagem do **Armbian** (ex: `Armbian_2X.XX_Rk322x-box_jammy_legacy_5.10.160.img.gz`) para a pasta `images` dentro desta partição.
3.  **Inicie o Multitool:** Insira o SD na TV Box e conecte a energia. O menu do Multitool aparecerá na tela.
4.  **(Opcional, mas recomendado)** Use a opção `Backup flash` para salvar seu sistema Android.
5.  **Grave a Imagem:**
    * Selecione a opção `Burn image to flash`.
    * Escolha o dispositivo de destino (geralmente `mmcblk2`).
    * Selecione a imagem do Armbian que você copiou.
6.  **Finalize:** Aguarde o processo terminar. Quando acabar, escolha `Shutdown` no menu.
7.  **Primeiro Boot:** Desconecte a energia, **remova o cartão SD** e reconecte a energia. O Armbian começará a iniciar da memória interna. O primeiro boot é demorado.
8.  **Configure o Armbian:** Siga as instruções do [**passo de configuração final**](#-configuração-pós-instalação).

#### **Método 3: Instalação na Memória Interna (NAND)**

O processo é quase idêntico ao de eMMC, mas com uma opção específica no menu.

1.  **Siga os passos 1, 2 e 3** do método eMMC.
2.  **(Opcional, mas recomendado)** Use a opção `Backup flash`.
3.  **Grave a Imagem:**
    * Selecione a opção `Burn Armbian image via steP-nand`. **Certifique-se de usar uma imagem de kernel LEGACY.**
    * Escolha o dispositivo de destino (geralmente `rknand0`).
    * Selecione a imagem do Armbian.
4.  **Finalize e Configure:** Siga os passos 6, 7 e 8 do método eMMC.

---

### 🚀 **Configuração Pós-Instalação**

Após o primeiro boot, o sistema pedirá para você:

1.  Criar uma senha para o usuário `root`.
2.  Criar um novo usuário padrão e sua senha.

Depois de fazer o login com seu novo usuário, execute os seguintes comandos para configurar o hardware específico da sua box e as opções do sistema:

```bash
# Para configurar o hardware (LEDs, Wi-Fi, velocidade da eMMC, etc.)
sudo rk322x-config

# Para configurar opções do sistema (fuso horário, idioma, etc.)
sudo armbian-config
```

Parabéns, seu Armbian está instalado e configurado!

---

### ℹ️ **Informações Adicionais**

#### **Ordem de Boot**

Quando o bootloader U-Boot do Armbian está instalado na memória interna (eMMC/NAND), ele procura por um sistema para iniciar na seguinte ordem:

1.  **Cartão SD Externo**
2.  **Dispositivo USB Externo** (na porta OTG)
3.  **Memória eMMC Interna**

Isso significa que, mesmo com o Armbian instalado internamente, você ainda pode dar boot por um cartão SD ou USB simplesmente inserindo-o antes de ligar o aparelho.

#### Compilando sua Própria Imagem (Avançado)

Para usuários avançados que desejam personalizar a imagem (incluindo o kernel mais recente, patches específicos ou pacotes pré-instalados), é possível compilar uma versão do Armbian do zero. O processo utiliza o framework de compilação original do Armbian.

1.  **Clone o repositório original do Armbian Build Framework:**
    ```bash
    git clone --depth=1 [https://github.com/armbian/build](https://github.com/armbian/build)
    cd build
    ```

2.  **Execute o script de compilação:**
    ```bash
    sudo ./compile.sh
    ```

3.  **Selecione a placa correta:**
    > **Ponto Crítico:** Durante a execução do script, você será apresentado a um menu interativo. Para compilar a imagem para esta TV Box, você deve navegar e selecionar a placa correta:
    > 1. Primeiro, escolha a opção que mostra as placas `CSC / TVB / EOL`.
    > 2. Na lista seguinte, selecione a placa **`rk322x-box`**.

O script irá baixar todas as dependências e compilar o bootloader, o kernel e o sistema de arquivos. Para mais detalhes sobre as opções de compilação e os requisitos de sistema (como SO, RAM e espaço em disco), consulte a [documentação oficial do Armbian para desenvolvedores](https://docs.armbian.com/Developer-Guide_Overview/).

---

### ❤️ **Créditos e Projeto Original**

Todo o crédito pelo incrível trabalho de desenvolvimento do sistema Armbian e das ferramentas de compilação vai para a **equipe Armbian e seus contribuidores**.

* **Repositório do Build Framework (Original):** [https://github.com/armbian/build](https://github.com/armbian/build)
* **Site Oficial:** [https://www.armbian.com](https://www.armbian.com)
* **Fórum da Comunidade:** [https://forum.armbian.com](https://forum.armbian.com)
* **Tópico Guia Para RK322x:** [CSC Armbian for RK322x TV box boards](https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/)
