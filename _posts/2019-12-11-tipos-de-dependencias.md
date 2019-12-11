---
layout: post
title: "[Técnico] Tipos de Dependências"
published: true
categories: [blog, técnico]
tags: [dependência, .NET]
---

![capa](/assets/dependency.jpg)

Todos os softwares têm dependências. É assim que começa o capítulo sobre dependências e sobreposição, do livro Adaptive Code via C#. Isto é um fato! E neste artigo pretendo sintetizar o conteúdo apresentado neste capítulo do livro de modo que você termine ele sendo capaz de, não só saber os tipos de dependências que existem nos projetos que fizemos, como também identificar esses fenômenos.

Mas, primeiramente, o que é uma dependência? Trazendo para o mundo real, dependência 
é quando duas entidades possuem uma relação entre elas, onde uma não consegue realizar uma ação (ou até mesmo existir) sem a outra. Ou seja, é quando o meu irmão mais novo precisa pedir uns trocados para o meu pai quando ele quer ir ao cinema. 

Em nossos projetos não é diferente, quantas vezes só importamos aquela nossa famosa biblioteca de estimação, normalmente chamada de utils, para nosso projeto atual? Ou criamos nosso projeto em cima de algum framework? Ou então aquele pacote capaz de serializar objetos?

Cada um desses casos citados acima representa um tipo de dependência.

## Dependência First-Party

Apresentada no livro como dependência first-party, esse fenômeno é a relação entre entidades manipuláveis do seu projeto. Por exemplo, relação entre duas classes, relação entre dois métodos, ou entre duas classes de projetos diferentes - mas presentes na mesma solução (quando utilizado o Visual Studio). 

Um exemplo de dependência first-part é o seguinte código: 

```c#
using System;

namespace FirstPartyDependency
{
    class Program
    {
        static void Main(string[] args)
        {
            var writer = new WriteHelloWorld();
            writer.Write();
        }
    }

    public class WriteHelloWorld
    {
        public void Write()
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

Para escrever HelloWorld no console, a classe Program necessita chamar o método Write da classe **WriteHelloWorld**. Assim, criando uma dependência entre as duas entidades, onde Program depende de WriteHelloWorld.

## Dependência de Framework

O projeto acima, além de conter uma dependência first-party, também contém uma dependência de framework. Neste caso, por exemplo, o projeto depende do framework .NET Core 2.2 e utiliza um método WriteLine para mostrar a mensagem no console. Este método pertence ao namespace **System**, que está declarado no topo do arquivo.

## Dependência Third-Party

Ao longo que nosso projeto cresce, mais surge a necessidade de focar no coração do negócio e com isso, evitar reconstruir a roda - que na maioria dos casos leva muito tempo. 

Com esse intuito, nós buscamos pacotes de terceiros para resolver problemas pontuais em nosso código. Esses códigos normalmente não podem ser alterados, pois não possuímos o fonte, e a dependência criada ao utilizar esses pacotes é chamada de dependência third-party.

O código abaixo exemplifica essa dependência:


```c#
using Newtonsoft.Json;
using System;

namespace ThirdPartyDependency
{
    class Program
    {
        static void Main(string[] args)
        {
            Message message = new Message
            {
                Autor = "Foo",
                Content = "Hello world"
            };

            var messageString = JsonConvert.SerializeObject(message);
            Console.WriteLine(messageString);
        }
    }

    public class Message
    {
        public string Content { get; set; }
        public string Autor { get; set; }
    }
}
```
Nosso projeto acima precisa serializar um objeto e mostrá-lo no console. Para isso foi utilizado o método **JsonConvert.SerializeObject** do pacote Newtonsoft.Json. Não temos acesso ao código fonte, e essa classe também não está presente no framework utilizado, assim temos o cenário de uma dependência third-party.

## Conclusões

Ter dependência em seu código não significa que ele seja ruim, que você deve reescrever tudo. Não! Ruim é não ter conhecimento das dependências existentes e não gerenciá-las com o devido cuidado.

O uso desmazelado das dependências podem acarretar em dependências cíclicas, e isso, a longo prazo, pode matar um projeto inteiro! Falaremos mais sobre isso no futuro.

Muito obrigado!
