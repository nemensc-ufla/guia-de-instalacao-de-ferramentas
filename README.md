# 🌊 Guia de Instalação para Iniciantes em CFD
### Salome, OpenFOAM e ParaView no Windows e Ubuntu

[![OS - Windows](https://img.shields.io/badge/OS-Windows_10%2F11-0078D6?logo=windows&logoColor=white)](#caminho-1-instalação-no-windows)
[![OS - Ubuntu](https://img.shields.io/badge/OS-Ubuntu_Linux-E95420?logo=ubuntu&logoColor=white)](#caminho-2-instalação-no-ubuntu-linux)
[![OpenFOAM](https://img.shields.io/badge/OpenFOAM-v2412-00599C?logo=openfoam&logoColor=white)](https://openfoam.com)
[![ParaView](https://img.shields.io/badge/ParaView-Latest-red)](https://www.paraview.org/)
[![Salome](https://img.shields.io/badge/Salome-Platform-0A85EA)](https://www.salome-platform.org/)

---

Seja muito bem-vindo ao universo da **Dinâmica dos Fluidos Computacional (CFD)**! 🚀

Para realizar qualquer simulação numérica completa, nós normalmente seguimos três etapas essenciais:

```mermaid
flowchart LR
    A[📐 Pré-processamento<br><b>Salome</b>] --> B[⚙️ Solução / Solver<br><b>OpenFOAM</b>]
    B --> C[📊 Pós-processamento<br><b>ParaView</b>]
```

1. **Pré-processamento (Salome):** Construção da geometria 3D e geração da malha (*mesh*).
2. **Processamento / Solução (OpenFOAM):** Onde as equações da física dos fluidos são de fato resolvidas numericamente.
3. **Pós-processamento (ParaView):** Onde visualizamos os resultados em mapas de cores, vetores, linhas de corrente e animações.

---

## 📑 Sumário

* [🪟 Caminho 1: Instalação no Windows (via WSL2)](#caminho-1-instalação-no-windows)
  * [Passo 1: Instalação do WSL](#1-preparando-o-terreno-instalando-o-wsl)
  * [Passo 2: Instalação do OpenFOAM e ParaView no WSL](#2-instalando-openfoam-e-paraview-no-wsl)
  * [Passo 3: Instalação do Salome no Windows](#3-instalando-o-salome-no-windows)
* [🐧 Caminho 2: Instalação no Ubuntu Linux (Nativo)](#caminho-2-instalação-no-ubuntu-linux)
  * [Passo 1: Instalação do OpenFOAM](#1-instalando-o-openfoam-no-ubuntu)
  * [Passo 2: Instalação do ParaView](#2-instalando-o-paraview-no-ubuntu)
  * [Passo 3: Instalação do Salome](#3-instalando-o-salome-no-ubuntu)
* [🛠️ Solução de Problemas Comuns (FAQ)](#solução-de-problemas-comuns)

---

# 🪟 Caminho 1: Instalação no Windows

O OpenFOAM foi originalmente desenvolvido para sistemas Linux. Para executá-lo com máximo desempenho e fidelidade no Windows, utilizaremos o **WSL 2 (Subsistema do Windows para Linux)**, que integra uma distribuição completa do Ubuntu ao Windows de forma rápida e transparente.

```
+-------------------------------------------------------------+
|                      WINDOWS 10 / 11                        |
|                                                             |
|   +--------------------------+    +---------------------+   |
|   | 📐 Salome (.exe nativo)  |    | 🐧 WSL 2 (Ubuntu)   |   |
|   +--------------------------+    |   ├── OpenFOAM      |   |
|                                   |   └── ParaView      |   |
|                                   +---------------------+   |
+-------------------------------------------------------------+
```

### 1. Preparando o terreno: Instalando o WSL

1. No menu Iniciar do Windows, procure por **Prompt de Comando** (ou **PowerShell**), clique com o botão direito e selecione **"Executar como Administrador"**.
2. Digite o seguinte comando e aperte `Enter`:
   ```cmd
   wsl --install -d Ubuntu
   ```
3. Aguarde o download e a configuração dos pacotes. Ao término, **reinicie o computador**.
4. Após reiniciar, uma janela preta (terminal do Ubuntu) se abrirá automaticamente.
   * O sistema solicitará um **nome de usuário** (digite um nome sem espaços ou acentos, em letras minúsculas, por exemplo: `estudante`).
   * Em seguida, pedirá uma **senha**.

> [!WARNING]
> **Atenção à senha oculta:** Ao digitar sua senha no terminal do Linux, os caracteres **não aparecem na tela** (nem mesmo asteriscos ou pontinhos). Isso é um comportamento padrão de segurança. Apenas digite a senha completa e aperte `Enter`.

> [!TIP]
> **Como abrir no dia a dia:** Para acessar seu ambiente Linux a qualquer momento, basta abrir o menu Iniciar e pesquisar por **Ubuntu**.

---

### 2. Instalando OpenFOAM e ParaView no WSL

Abra o seu terminal **Ubuntu** no Windows.

#### A. Atualize o sistema
Cole a linha abaixo no terminal (para colar no WSL, basta clicar com o **botão direito do mouse** dentro da janela) e pressione `Enter`:
```bash
sudo apt update && sudo apt upgrade -y
```

#### B. Instale o OpenFOAM
Adicione a chave e o repositório oficial da OpenCFD:
```bash
sudo sh -c "wget -O - https://dl.openfoam.com/add-debian-repo.sh | bash"
```

Em seguida, instale a versão oficial:
```bash
sudo apt update
sudo apt install -y openfoam2412-default
```

> [!NOTE]
> Essa etapa pode demorar alguns minutos dependendo da sua conexão, pois o servidor do OpenFOAM possui taxas de transferência limitadas para downloads públicos.

#### C. Configurar a inicialização automática
Para que os comandos do OpenFOAM fiquem disponíveis sempre que você abrir o terminal, vincule as variáveis de ambiente ao arquivo `.bashrc`:
```bash
echo "source /usr/lib/openfoam/openfoam2412/etc/bashrc" >> ~/.bashrc
source ~/.bashrc
```

Para validar se a instalação funcionou, execute:
```bash
simpleFoam -help
```
*Se um bloco de texto com opções de uso for exibido, o OpenFOAM está pronto!* 🎉

#### D. Instalando o ParaView no WSL
Instale o ParaView diretamente pelo gerenciador do Ubuntu:
```bash
sudo apt install -y paraview
```

> [!IMPORTANT]
> **Configuração do Fuso Horário durante a instalação:**  
> O instalador poderá pausar solicitando a configuração do fuso horário (*geographic area*). Proceda da seguinte forma:
>
> 1. Na lista de regiões, digite `2` para selecionar **America** e tecle `Enter`.
> 2. Na lista de cidades subsequente, digite o número correspondente à sua região (por exemplo, digite `22` para selecionar **Bahia**) e confirme com `Enter`.

Para iniciar o ParaView a partir do terminal (as janelas abrirão diretamente na sua interface gráfica do Windows 10/11 via WSLg):
```bash
paraview &
```

> [!TIP]
> **Onde ficam salvos os meus arquivos?**
> * **Do Linux para o Windows:** Acesse seus discos do Windows pela pasta `/mnt/c/`.
> * **Do Windows para o Linux:** Para ver e manipular seus arquivos do Ubuntu usando a interface visual do Explorador de Arquivos do Windows, digite no terminal:
>   ```bash
>   explorer.exe .
>   ```

---

### 3. Instalando o Salome no Windows

O Salome pode ser executado diretamente no Windows através de seu instalador nativo.

1. Acesse o portal oficial de downloads: [salome-platform.org](https://www.salome-platform.org/?page_id=2430).
2. Localize a versão empacotada para **Windows** (arquivo `.exe` ou pacote `.zip`).
3. Execute o instalador e avance pelo assistente:
   * Clique em **Next / Avançar**.
   * Aceite os termos de licença.
   * Mantenha o diretório padrão sugerido (por exemplo, `C:\SALOME\...`).
4. Ao final da instalação, utilize o atalho criado na Área de Trabalho ou no menu Iniciar para iniciar o programa.

---

# 🐧 Caminho 2: Instalação no Ubuntu Linux

Para usuários que possuem o Ubuntu (versões LTS 20.04, 22.04, 24.04 ou superiores) instalado de forma nativa na máquina:

### 1. Instalando o OpenFOAM no Ubuntu

Abra o terminal (`Ctrl + Alt + T`) e execute os comandos:

```bash
# 1. Registrar repositório oficial da OpenCFD
sudo sh -c "wget -O - https://dl.openfoam.com/add-debian-repo.sh | bash"

# 2. Atualizar índices e baixar os binários da v2412
sudo apt update
sudo apt install -y openfoam2412-default

# 3. Registrar o carregamento automático no perfil
echo "source /usr/lib/openfoam/openfoam2412/etc/bashrc" >> ~/.bashrc
source ~/.bashrc
```

---

### 2. Instalando o ParaView no Ubuntu

Instale o pacote com um comando simples:

```bash
sudo apt install -y paraview
```

> [!IMPORTANT]
> Caso a tela de configuração de fuso horário seja exibida durante o processo:
> * Escolha a opção correspondente à região **America** (geralmente opção `2`).
> * Em seguida, informe a cidade de referência (por exemplo, opção `22` para **Bahia**).

Para abrir a ferramenta:
```bash
paraview &
```

---

### 3. Instalando o Salome no Ubuntu

No Linux, o Salome é distribuído em pacotes *self-contained* (que já vêm com todas as dependências embutidas):

1. Baixe o pacote correspondente à sua distribuição no site oficial: [salome-platform.org](https://www.salome-platform.org/?page_id=2430) (arquivo com extensão `.tar.gz`).
2. Abra o terminal na pasta onde o download foi salvo (por exemplo, `~/Downloads`) e extraia o conteúdo:
   ```bash
   cd ~/Downloads
   tar -xvf SALOME-*.tar.gz
   ```
3. Acesse a pasta extraída e inicie o executável:
   ```bash
   ./salome
   ```
   *(Recomenda-se mover o diretório descompactado para a sua pasta pessoal `/home/usuario` ou `/opt` para manter o sistema organizado).*

---

# 🛠️ Solução de Problemas Comuns

### 1. Erro de Virtualização ao iniciar o WSL no Windows
* **Sintoma:** Ao rodar `wsl --install`, o terminal acusa: *"Virtualization support is disabled in firmware"* ou *"A virtualização baseada em hardware está desabilitada"*.
* **Causa:** A tecnologia de virtualização do processador está desligada nas configurações da placa-mãe.
* **Solução:** Reinicie o computador e acesse o menu da **BIOS/UEFI** (geralmente pressionando teclas como `F2`, `F10`, `F12` ou `Del` logo ao ligar). Procure pela seção de CPU ou Segurança e habilite a opção **Intel Virtualization Technology (VT-x)** ou **AMD SVM/AMD-V**. Salve e reinicie.

---

### 2. A interface gráfica do ParaView não abre no WSL
* **Sintoma:** O comando `paraview` retorna erros como `Error: Can't open display` ou `Cannot connect to X Server`.
* **Solução:**
  1. No Windows 10/11, garanta que o recurso de suporte a janelas Linux (**WSLg**) está atualizado executando no Prompt de Comando do Windows:
     ```cmd
     wsl --update
     ```
  2. **Alternativa recomendada:** Caso prefira máxima aceleração de GPU na renderização, você também pode instalar o [ParaView para Windows](https://www.paraview.org/download/). Nesse fluxo de trabalho, você roda as simulações no terminal WSL e visualiza os arquivos de resultados abrindo o ParaView do próprio Windows.

---

### 3. "Comando simpleFoam não encontrado"
* **Sintoma:** Ao tentar rodar comandos como `simpleFoam`, `blockMesh` ou `paraFoam`, o terminal retorna `command not found`.
* **Solução:** As variáveis de ambiente do OpenFOAM não foram carregadas na sessão atual. Execute manualmente no terminal:
  ```bash
  source /usr/lib/openfoam/openfoam2412/etc/bashrc
  ```
  Para evitar ter que digitar isso toda vez, confirme se a linha acima está salva no fim do seu arquivo `~/.bashrc`.

---

### 4. Salome fecha inesperadamente ou falha ao inicializar a área 3D no Linux
* **Sintoma:** O Salome fecha sozinho logo ao carregar os módulos `Geometry` ou `Mesh`, exibindo avisos de OpenGL.
* **Solução:** O Salome requer suporte robusto a aceleração gráfica 3D. Em computadores com placas dedicadas (especialmente NVIDIA), certifique-se de que os drivers de vídeo oficiais proprietários estão em uso por meio do utilitário **"Drivers Adicionais"** (*Additional Drivers*) do Ubuntu.
