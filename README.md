![GitHub License](https://img.shields.io/github/license/stelacalmon/https%3A%2F%2Fgithub.com%2Fstelacalmon%2Fimpressora-termica)


# Java - configurando uma impressora térmica
Tutorial para configuração de uma impressora térmica (cupom fiscal) usando a linguagem Java.
## Autor 
Stela Calmon

## Introdução

Nesta atividade aprendemos como uma impressora térmica pode ser controlada por um programa em Java utilizando comandos ESC/POS. Esses comandos permitem alterar o formato da impressão, alinhar textos, aumentar o tamanho da fonte, ativar negrito, avançar o papel e realizar o corte automático.

## Conexão com a impressora

A comunicação foi feita através de uma conexão de rede utilizando um **Socket**.

```java
Socket impressora = new Socket("xx.xx.xx.xx", 9100);
OutputStream saida = impressora.getOutputStream();
```

* **Socket:** estabelece a conexão entre o computador e a impressora.
* **IP:** `xx.xx.xx.xx`
* **Porta:** `9100` (porta padrão de muitas impressoras térmicas de rede).
* **OutputStream:** envia os comandos e textos para a impressora.

---

## Inicialização da impressora

Antes de enviar qualquer texto, é necessário inicializar a impressora.

```java
saida.write(new byte[]{0x1B, 0x40});
```

Esse comando corresponde ao **ESC @**, responsável por restaurar as configurações padrão da impressora.

---

## Impressão de data e hora

Utilizamos as bibliotecas do Java para obter a data e a hora atuais.

```java
LocalDateTime.now();
DateTimeFormatter.ofPattern("dd/MM/yyyy HH:mm:ss");
```

Depois, enviamos o texto para a impressora utilizando a codificação **CP850**, que permite imprimir corretamente caracteres acentuados.

```java
saida.write(datahora.getBytes("CP850"));
```

---

## Comandos ESC/POS aprendidos

### Alterar tamanho da fonte

```java
saida.write(new byte[]{0x1D, 0x21, 0x11});
```

Aumenta o tamanho da fonte.

Para voltar ao tamanho normal:

```java
saida.write(new byte[]{0x1D, 0x21, 0x00});
```

---

### Alinhamento

Esquerda:

```java
0x1B 0x61 0x00
```

Centro:

```java
0x1B 0x61 0x01
```

Direita:

```java
0x1B 0x61 0x02
```

---

### Negrito

Ativar:

```java
0x1B 0x45 0x01
```

Desativar:

```java
0x1B 0x45 0x00
```

---

### Avançar papel

```java
0x1B 0x64 0x05
```

Avança cinco linhas antes do corte.

---

### Corte automático

```java
0x1D 0x56 0x00
```

Realiza o corte do papel após a impressão.

---

## Fluxo do programa

1. Conectar na impressora pela rede.
2. Inicializar a impressora.
3. Obter a data e a hora atuais.
4. Imprimir a data e a hora.
5. Alterar tamanho e alinhamento da fonte.
6. Imprimir o título.
7. Ativar e desativar o negrito.
8. Imprimir os demais textos.
9. Avançar o papel.
10. Cortar o papel.
11. Enviar todos os dados (`flush()`).
12. Fechar a conexão.

---

## O que aprendemos

* Como conectar uma impressora térmica via **Socket**.
* Como utilizar a porta **9100** para comunicação.
* Como enviar dados usando **OutputStream**.
* O que é a linguagem de comandos **ESC/POS**.
* Como alterar tamanho da fonte, alinhamento e negrito.
* Como imprimir data e hora automaticamente.
* Como avançar o papel e realizar o corte automático.
* Como utilizar a codificação **CP850** para imprimir caracteres especiais em português.

## Conclusão

Durante esta atividade aprendemos os conceitos básicos da comunicação com uma impressora térmica utilizando Java. Também conhecemos os principais comandos ESC/POS para controlar a formatação da impressão e entendemos como enviar informações diretamente para a impressora por meio de uma conexão de rede.
