# Glossário

1. [Introdução](#1-introdução)
2. [Orientações](#2-orientações)
3. [Só código](#3-só-código)
4. [Sobre mim (resumido)](#4-sobre-mim)
5. [Versão em Inglês](https://github.com/Samu3lb0az/testewriupVolati_ingles.git)

## **1. Introdução**

**Objetivo**: Este writeup foi desenvolvido para auxiliar na **compreensão das tarefas** do lab (**Nome do Lab**), oferecendo um guia prático de execução diretamente na máquina virtual do TryHackMe. O ambiente já inclui todos os requisitos pré-instalados (Python 3, ferramentas do **Volatility** e arquivos necessários), permitindo que você se concentre na análise forense sem preocupações com configurações.

**Ferramenta**: Volatility 3.

## 2. Orientações

As orientações se passam na task 10 do módulo Volatility da plataforma TryHackMe.

É importante ressaltar que todas as atividades da task 10 são feitas na máquina do próprio TryHackMe, sendo possível ligá-la na task 3.

---

**Enunciado: Qual é a versão de compilação da máquina host no Caso 001?**

Esse desafio é possível de ser feito a partir do que foi passado nas tasks anteriores. Eu utilizei o plugin *‘info’*, que vem depois da indicação de qual é o sistema operacional da memória. Neste caso, usaremos o *‘windows.info’*, ficando:

```python
vol -f /Scenarios/Investigations/Investigation-1.vmem windows.info
```

Uma dica que recebi de um amigo é que o próprio TryHackMe já fornece quantos caracteres e se haverá um caractere especial, assim facilitando a procura da resposta correta. Com essa pequena dica, consegui achar diversas respostas apenas analisando o que o plugin me trazia.

---

**Enunciado: Em que momento o arquivo de memória foi adquirido no Caso 001?**

Nesse desafio, utilizei o mesmo plugin do anterior. Em resumo, achei a resposta com base nos conhecimentos que o próprio módulo nos proporciona, e análise do que é mostrado após executar o plugin. A dica que mencionei anteriormente ajudou também a encontrar esta resposta.

```python
vol -f /Scenarios/Investigations/Investigation-1.vmem windows.info
```

---

**Enunciado: Qual processo pode ser considerado suspeito no Caso 001?
Observação: determinados caracteres especiais podem não estar visíveis na VM fornecida. Ao copiar e colar, eles ainda estarão presentes.**

Nesse desafio, já passamos a utilizar outro plugin, sendo ele o *‘pslist’*, algo que também é simples e que demanda interpretação e identificação do que se pede. Essa observação que o TryHackMe traz sobre caracteres especiais é mais sobre copiar e colar da VM para o campo de resposta. Então, neste desafio, é interessante escrever por extenso o processo. O plugin é:

```python
vol -f /Scenarios/Investigations/Investigation-1.vmem windows.pslist
```

---

**Enunciado: Qual é o processo pai do processo suspeito no Caso 001?**

Esse desafio também está relacionado com o plugin anterior, sendo bem simples de encontrar o que se pede, no meu caso apenas analisando o que o plugin me mostrou:

```python
vol -f /Scenarios/Investigations/Investigation-1.vmem windows.pslist
```

---

**Enunciado: Qual é o PID do processo suspeito no Caso 001?**

Neste desafio, também usamos o mesmo plugin e lógica para fazer:

```python
vol -f /Scenarios/Investigations/Investigation-1.vmem windows.pslist
```

---

**Enunciado: Qual é o PID do processo pai no Caso 001?**

E, por fim, este também:

```python
vol -f /Scenarios/Investigations/Investigation-1.vmem windows.pslist
```

---

**Enunciado: Qual user-agent foi empregado pelo adversário no Caso 001?**

Este já foi mais complicado. Hesitei por algumas vezes em usar a dica do TryHackMe, tentei procurar sobre alguns outros plugins, até que decidi usar a dica. Aqui vamos usar mais de um plugin, sendo o primeiro:

```python
vol -f /Scenarios/Investigations/Investigation-1.vmem windows.memmap --pid 1640 --dump
```

Utilizamos o *‘memmap’* junto com *‘--pid 1640’*, o PID do processo suspeito, e *‘--dump’*, que nos informará dezenas de informações. Após parar de mostrar todas as informações, usamos este comando:

```python
strings *.dmp | grep -i "user-agent"
```

E nos dará a resposta.

---

**Enunciado: O Chase Bank foi um dos domínios bancários suspeitos encontrados no Caso 001? (Y/N)**

A resposta tem a ver com o desafio anterior, pois como fizemos um dump no PID do processo malicioso, entendemos que a resposta é SIM, no caso “Y”.

---

**Enunciado: Qual processo suspeito está sendo executado no PID 740 no Caso 002?**

Neste também é algo simples, que com o plugin correto e uma boa análise já é possível encontrar a resposta correta. Apenas se atente, pois nesse desafio o arquivo de memória foi mudado:

```python
vol -f /Scenarios/Investigations/Investigation-2.raw windows.pslist
```

---

**Enunciado: Qual é o caminho completo do binário suspeito no PID 740 no Caso 002?**

Este já é mais complexo, pois envolve outro plugin com algo a mais, especificamente um *‘grep’* mandando ele buscar algo específico, ficando:

```python
vol -f /Scenarios/Investigations/Investigation-2.raw windows.dlllist | grep 740
```

Aqui foi utilizado o *‘dlllist’* para listar recursos do sistema e com o *‘grep’* filtrar exatamente através do PID do processo suspeito achado no desafio anterior.

---

**Enunciado: Qual é o processo pai do PID 740 no Caso 002?**

Nesse desafio, utilizei o mesmo plugin do primeiro desafio da segunda investigação, e também foi utilizado o mesmo tipo de raciocínio e análise para achar a resposta:

```python
vol -f /Scenarios/Investigations/Investigation-2.raw windows.pslist
```

---

**Enunciado: Qual é o PID do processo pai suspeito conectado ao descriptografador no Caso 002?**

Neste desafio, foi utilizado o mesmo tipo de plugin que no anterior:

```python
vol -f /Scenarios/Investigations/Investigation-2.raw windows.pslist
```

---

**Enunciado: Pelas nossas informações atuais, que malware está presente no sistema no Caso 002?**

Nesse desafio, foi utilizado o mesmo código do anterior apenas para averiguar o processo correto, mas, como já sabemos que o processo é o “@WanaDecryptor@”, deduzi que o malware fosse o WannaCry.

---

**Enunciado: Qual DLL é carregada pelo descriptografador usado para criação de soquete no Caso 002?**

Nesse utilizamos novamente um plugin que já usamos, com a junção de outro. A única questão é saber qual processo está pedindo. O que me ajudou muito a achar qual o processo correto foi a orientação dos caracteres no campo de resposta do TryHackMe. Plugin:

```python
vol -f /Scenarios/Investigations/Investigation-2.raw windows.dlllist | grep 740
```

---

**Enunciado: Que mutex pode ser encontrado como um indicador conhecido do malware em questão no Caso 002?**

Nesse eu cheguei a pesquisar algum plugin ou outro tipo de informação que podia ajudar, e então parti para a dica:

```python
vol -f /Scenarios/Investigations/Investigation-2.raw windows.handles | grep 1940
```

Sendo o plugin utilizado o ‘handles’, e o ‘grep’. Após toda a informação trazida após o plugin ser executado, é possível achar dois elementos, sendo um deles a resposta:

![image.png](attachment:36f3a7d7-e564-4db2-8b19-fa0b3d0790de\:image.png)

---

**Enunciado: Qual plugin poderia ser usado para identificar todos os arquivos carregados do diretório de trabalho do malware no Caso 002?**

Este não teve como, a parti para a pesquisa. Não tem muito segredo — uma breve busca já te traz a resposta.

---

## 3. Só código

Códigos usados:

```bash
vol -f /Scenarios/Investigations/Investigation-1.vmem windows.info
```

```bash
vol -f /Scenarios/Investigations/Investigation-1.vmem windows.pslist
```

```bash
vol -f /Scenarios/Investigations/Investigation-1.vmem windows.memmap --pid 1640 --dump
```

```bash
strings *.dmp | grep -i "user-agent"
```

```bash
vol -f /Scenarios/Investigations/Investigation-2.raw windows.pslist
```

```bash
vol -f /Scenarios/Investigations/Investigation-2.raw windows.dlllist | grep 740
```


*Recomendo revisar o tópico “Orientações” para evitar conflitos ou mal-entendidos.*

---

## 4. Sobre mim

*Resumo das informações sobre o autor.*
