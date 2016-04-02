How to Structure Bookshelf.js Models
Published Nov 30th, 2015

I’m starting my second project using the excellent Bookshelf.js ORM (for relational databases) for Node.js and I’ve noticed that unless you’re a JavaScript pro it’s easy to screw up the structure of your models and get circular dependency errors. I love Bookshelf but the documentation has actually gotten worse recently so I decided I’d document how to structure your models in different files the correct way using Bookshelf.js.

The problem

When you’re developing in any language it’s always a good idea to modularize your code. Bookshelf models are no exception to this rule. Unfortunately, the first time I used this neat package I threw my hands up and ended up defining all of my models in one file, in one big exported object. It wasn’t modular. The reason I did this was because of circular dependency errors.

It’s likely that you’ll need to do something like this with your code at some point (example taken directly from the Bookshelf wiki)

# Как стурктурировать модели при работе с Bookshelf.js.

Начав мой второй проект я решил попробовать Bookshelf.js (это замечательная ORM для Node.js).
