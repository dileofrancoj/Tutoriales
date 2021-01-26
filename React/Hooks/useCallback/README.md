# useCallback

## Retorna un callback memorizado

```
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

<p>Este hook devuelve una versión memorizada del callback que solo cambia si una o más dependencias cambian</p>

<small>
* El hook useCallback sirve para mejorar el rendimiento de nuestros componentes en base a memorizar funciones que se usan como callbacks.
</small>

<p>Este hook es útil en el escenario en el que se pasan funciones por props a componentes hijos</p>
<hr>

<p>Tener en cuenta que el render de un componente implica la re-renderización del jsx retornado Y la creacion de variables y funciones previas al return. Por lo que, a la hora de hacer un cambio de estado, las funciones se crearan nuevamente a menos que estén memorizadas</p>

<h3>¿Debo aplicar useCallback todo el tiempo?</h3>

<p>La respuest es NO.</p>
<p>Memorizar funciones tiene un costo y claramente impacta en el rendimiento de la aplicacion</p>