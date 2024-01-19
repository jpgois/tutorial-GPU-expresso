# Passo-a-passo-GPU-super-expresso

Passo-a-passo para submeter um job para o Cluster Carbono utilizando Miniconda, Pytorch e CUDA. Talvez não sejam necessários todos estes passos. Então, fiquem a vontade para entrarem em contato para aprimorarmos este documento. 

1. Logar na HPC

```bash
ssh [seulogin]@hpc.ufabc.edu.br
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


9. voce consegue ver as partições que o Cluster contém com o comando:

```bash
sinfo
 ```

10. No caso da Carbono, duas delas tem GPUs, que são as partições **metano** (A30), e a etileno** (A40)

11. Utilizei o seguinte código em Python, que salvei como **checktorchgpu.py** para testar se a GPU estava visível
```python
import torch

# Verificar se há GPUs disponíveis
if torch.cuda.is_available():
    # Exibir informações sobre as GPUs
    for i in range(torch.cuda.device_count()):
        print("Nome da GPU:", torch.cuda.get_device_name(i))
        print("Disponível para uso:", torch.cuda.is_available())
        print("Memória total:", torch.cuda.get_device_properties(i).total_memory)
else:
    print("Nenhuma GPU encontrada.")
```
12. E o seguinte script bash, que salvei como  **scriptTorchGPU.sh**, para submeter este código para o Cluster

```bash
#!/bin/bash
#SBATCH --job-name=jpg_testGPU
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=2
#SBATCH --gres=gpu:2
#SBATCH --output testGPU-%j.out
#SBATCH --error testGPU-%j.err
#SBATCH --partition metano


module load cuda/12.2-gcc-12.2.0-cexgeyz

# ACTIVATE ANACONDA

eval "$(conda shell.bash hook)"

conda activate basetorch

# RUN BENCHMARK
python3 -u checktorchgpu.py
```

13. Para rodar este código, fiz:
```bash
 sbatch scriptTorchGPU.sh
```

**Agradecimentos:** Este passo-a-passo teve como ponto de partida o Tutorial do [Oliveiras96](https://github.com/Oliveiras96/Tutorial-espresso-2023). 
