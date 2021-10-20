---
title: Clean code Tips 01
author: Victor Canseco
layout: post
---

# Clean Code Tips - 01

## Usar nombres significativos

Hola, bienvenidos a este nuevo blog. Comenzar&eacute; con uno de los puntos m&aacute;s dificiles al momento de comenzar a programar y es el **Poner nombre** a tus variables, clases, arreglos, etc.

Y vaya que es un martirio comenzar a ponerles nombre, sin embargo uno de los primeros tips para tener un c&oacute;digo limpio y claro es nombrar correctamente cada elemento que se utilice.

Para ello no te quiebres la cabeza cuando recien empiezas a codificar. Los expertos recomiendan que primero escribas el codigo como va, sin importar los nombres; puesto que en este proceso no se tienen claras las ideas y mucho menos tienes visible todo el alcance del codigo.}

Asi que primero codifica como gustes.

´´´ cs
public static string Get(string input)
{
  char[] arr = new char[input.Length];
  int i = input.Length - 1;
  foreach (var e in input)
  {
    arr[i] = e;
    i--;
  }

  return new String(arr);
}
´´´


