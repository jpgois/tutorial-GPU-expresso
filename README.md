# Passo-a-passo-GPU-super-expresso

Passo-a-passo para submeter um job para o Cluster Carbono utilizando Miniconda, Pytorch e CUDA. Talvez não sejam necessários todos estes passos. Então, fiquem a vontade para entrarem em contato para aprimorarmos este documento. 

1. Logar na HPC

```bash
ssh [seuloging]@hpc.ufabc.edu.br
```

2. Logar na Carbono

```bash
ssh carbono.ufabc.int.br
```

3. Uma vez dentro da Carbono, precisamos acessar o Miniconda para construirmos os ambientes. 

4. Utilizei o Miniconda que está disponível como módulo.
5.  Consulte os módulos com o comando
```bash
module avail
```
6. Carreguei o Módulo Miniconda:
```bash
module load miniconda3/22.11.1-gcc-12.2.0-qnqpe5k
```
7. Inicializei o Conda
```bash
conda init
```
 8. Saí e entrei novamente no cluster e notei que o Conda já estava sendo carregado:
```bash
(base) [meulogin@carbono ~]$
 ```
9. Criei um env específico para teste com GPU utilizando o Pytorch, com a seguinte sequência de comandos:
```bash
conda env list
conda create --name basetorch
conda activate basetorch
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```


9.   voce consegue ver os nós que ela contém:

```bash
sinfo
 ```



**Agradecimentos:** Este passo-a-passo teve como ponto de partida o Tutorial do [Oliveiras96](https://github.com/Oliveiras96/Tutorial-espresso-2023). 
