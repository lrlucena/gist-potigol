# Árvore Binária de Expressão

```mermaid
stateDiagram-v2
   direction LR
   1 --> 2
   2 --> +
   + --> 3
   3 --> *
```

```mermaid
flowchart TB
  + --> 2
  + --> *
  * --> 1
  * --> 4

```

```mermaid
classDiagram
    class Nó {
      << abstrato >>
      ~ eval() Inteiro
      ~ tamanho() Inteiro
      ~ prefixo() Texto
      ~ infixo() Texto
      ~ posfixo() Texto
    }
    
    class Operaçao {
      << abstrato >>
      ~ simbolo() Texto
      prefixo() Texto
      infixo() Texto
      posfixo() Texto
    }
    
    Nó <-- Operaçao : esquerda
    Nó <|.. Operaçao
    Nó <-- Operaçao : direita
        
    Nó <|.. Valor
    
    class Soma {
      símbolo = "+"
      Soma(Nó, Nó)
      eval() Inteiro
    }
    
    class Subtraçao {
      símbolo = "-"
      Subtraçao(Nó, Nó)
      eval() Inteiro
    }
    
    class Multiplicaçao {
      símbolo = "*"
      Multiplicaçao(Nó, Nó)
      eval() Inteiro
    }

    class Divisao {
      símbolo = "/"
      Divisao(Nó, Nó)
      eval() Inteiro
    }
    
    Operaçao <|.. Soma
    Operaçao <|.. Subtraçao
    Operaçao <|.. Multiplicaçao
    Operaçao <|.. Divisao
    
    class Valor {
      Inteiro tamanho = 1
      Valor(Inteiro)
      prefixo() Texto
      infixo() Texto
      posfixo() Texto
    }

```

```scala
tipo abstrato Nó
  eval(): Inteiro
  tamanho(): Inteiro
  prefixo(): Texto
  infixo(): Texto
  posfixo(): Texto
fim

tipo abstrato Operação: Nó
  esquerda(): Nó
  direita() : Nó
  símbolo() : Texto
  tamanho() = 1 + esquerda.tamanho + direita.tamanho
  prefixo() = "{símbolo} {esquerda.prefixo} {direita.prefixo}"
  infixo()  = "({esquerda.infixo} {símbolo} {direita.infixo})"
  posfixo() = "{esquerda.posfixo} {direita.posfixo} {símbolo}"
fim

tipo Valor: Nó
  eval: Inteiro
  tamanho = 1
  prefixo, infixo, posfixo = eval.texto
fim

tipo Soma: Operação
  esquerda, direita: Nó
  símbolo = "+"
  eval() = esquerda.eval + direita.eval
fim

tipo Subtração: Operação
  esquerda, direita: Nó
  símbolo = "-"
  eval() = esquerda.eval - direita.eval
fim

tipo Multiplicação: Operação
  esquerda, direita: Nó
  símbolo = "*"
  eval() = esquerda.eval * direita.eval
fim

tipo Divisão: Operação
  esquerda, direita: Nó
  símbolo = "/"
  eval() = esquerda.eval div direita.eval
fim

arvore(pilha: Lista[Texto]): Nó =
  se ["+", "-", "*", "/"].contem(pilha.cabeça) então
    operação = escolha pilha.cabeça
      caso "+" => Soma
      caso "-" => Subtração
      caso "*" => Multiplicação
      caso "/" => Divisão
    fim
    direita  = arvore(pilha.cauda)
    esquerda = arvore(pilha.cauda.descarte(direita.tamanho))
    operação(esquerda, direita)
  senão
    Valor(pilha.cabeça.inteiro)
  fim

pilha = leia_texto.divida(" ").inverta
a = arvore(pilha)
escreva a
escreva "Infixo : {a.infixo}"
escreva "Prefixo: {a.prefixo}"
escreva "Posfixo: {a.posfixo}"
escreva "Eval   : {a.eval}"
```
