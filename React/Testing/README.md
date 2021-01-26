# Testing en React

<h2> ¿Qué es TDD? </h2>

<p>
Desarrollo guiado por pruebas de software, o Test-driven development (TDD) es una práctica de ingeniería de software que involucra otras dos prácticas: Escribir las pruebas primero (Test First Development) y Refactorización (Refactoring). Para escribir las pruebas generalmente se utilizan las pruebas unitarias
</p>

<p><strong>RED</strong>: El test fallará </p>
<p><strong>GREEN</strong>: El test pasa (requisitos minimos)</p>
<p><strong>REFACTOR</strong>: Se refactoriza y se aplican buenas prácticas</p>

<hr>
<h2>¿Qué es JEST?</h2>
<p>
    Es un framework de testing para Javascript. 
</p>
<p>Caracteristicas: 0 CONFIG, Isolado (los test pueden correrse de forma independiente y paralela), Fácil de Mockear</p>

<h3>Métodos globales de JEST </h3>
<p>Se debe crear una carpeta __tests__ dentro del proyecto</p>

example_globals.test.js
<small> Los test deben ser .test o .spec para que sean detectados </small>

```javascript
test("description", () => {
  expect(true).toBe(true);
});
test("description 2 ", () => {
  expect(true).toBe(true);
});
```

<p>Pueden correrse varios test dentro del mismo archivo e incluso los test pueden agruparse dentro de un describe </p>

```javascript
// agrupando el código
describe("Agrupando los tests", () => {
  test("example 1", () => {
    expect(true).toBe(true);
  });
});
```

<small>example_matchers.test.js</small>

```javascript
describe("matchers", () => {
  test("toBe", () => {
    //test de primitivos
    expect(true).toBe(true);
  });
  test("toEqual", () => {
    // test de valores complejos
    const data = { one: 1 };
    expect(data).toEqual({ one: 1 });
  });
  test("not", () => {
    // negacion del valor
    expect(true).not.toBe(false);
  });
  test("string", () => {
    // string y expresiones regulares
    expect("team").not.toBe(/team/i);
  });
});
```

<h3>Setup & teardown </h3>
<small>example_setup_teardown.test.js</small>

```javascript
describe("setup and teardown example", () => {
  // beforeAll se ejecuta antes de todos los test -> preparar los test
  beforeAll(() => {
    console.log("BeforeAll");
  });

  // beforeAll se ejecuta antes de correr cada test
  beforeEach(() => {
    console.log("beforeEach");
  });

  // se ejecuta despues de correr todos los tests
  afterAll(() => {
    console.log("afterAll()");
  });
  test("example 1", () => {});
});
```

<h3>Mocks y llamadas HTTP </h3>
<p>Los Mocks, “son objetos preprogramados con expectativas que conforman la especificación de lo que se espera que reciban las llamadas”</p>
<small>example_mocks.test.js</small>

```javascript
test("first example", () => {
  // nos retorna un mock
  const myMock = jest.fn();
  // nos aseguramos que el mock fue llamado myMock()
  expect(myMock).toHaveBeenCalled();
});
test("evaluando el mock", () => {
  const myMock = jest.fn();
  const result1 = myMock();
  expect(result1).toBe(true);
});
test("evaluando el mock que retorna info", () => {
  // seteo el valor que retorna
  const myMock = jest.fn().mockReturnValueOnce(true);
  const result1 = myMock();
  expect(result1).toBe(true);
});
test("evaluando el mock que retorna multiple info", () => {
  // seteo el valor que retorna
  const myMock = jest
    .fn()
    .mockReturnValueOnce(true)
    .mockReturnValueOnce("hello world");
  const result1 = myMock();
  const result2 = myMock();

  expect(result1).toBe(true);
  expect(result1).toBe("hello world");
});
```

<hr>
<h3>React testing library </h3>
<small>hello-world.js</small>

```javascript
export default const HelloWorld = () => {
    return (
        <h1>Hello World</h1>
    )
}
```

<h4>Test básico de componentes</h4>

<small>hello-world.test.js</small>

```javascript
import { render, screen } from "@testing-library/react";

import { HelloWorld } from "./hello-world";
test("should render hello-world", () => {
  const { debug, getByText } = render(<HelloWorld />);
  debug(); // muestra el contenido de la pantalla
  // verificamos que exista el elemento en pantalla
  const title = getByText(/hello world/i);

  // assert que extendemos gracias a RTL
  expect(title).toBeInTheDocument();
});
```

<h4>Test de eventos </h4>
<small>counter.js</small>

```javascript
import {useState} from 'react';
export default const Counter = () => {
    const [count,setCount] = useState(0);
    return (
        <>
            <button onClick={() => setCount(count+1)}>Click me</button>
            <p>Clicked times : {count}</p>
        </>
    )
}
```

<small>counter.test.js</small>

```javascript
import { render, screen, fireEvent } from "@testing-library/react";
import Counter from "./Counter";

describe("test en counter", () => {
  test("display zero initial count", () => {
    const { getByText } = render(<Counter />);
    const result = getByText(/clicked times : 0/i);
    expect(result).toBeInTheDocument();
  });
  test("display new counter after one click", () => {
    const { getByText } = render(<Counter />);
    // en caso que coincidan los strings de un texto se puede matchear por selector
    const button = getByText(/click me/i, { selector: "button" });
    fireEvent.click();
    const result = getByText(/clicked times: 1/i);
    expect(result).toBeInTheDocument();
  });
});
```

<hr>

<h3>Vamos a aplicar TDD a un formulario con una llamada a una API</h3>

<p>Paritmos de una historia de usuario</p>

<ul>
    <li>Debe haber una página de formulario de creación de producto</li>
    <li>El formulario debe tener los campos: nombre, talla, tipo (electrónico,mobiliario, ropa) y un botón de envío.</li>
    <li>Todos los campos son obligatorios</li>
    <li>etc</li>
</ul>

<p>Lo que caracteriza a la metodología TDD es que primero se desarrollan los test en base a las historias de usuario</p>

<small>form.js</small>

<small>form.test.js</small>

```javascript
import { screen, render } from "@testing-library/react";
import { Form } from "./Form";
describe("when the form is mounted", () => {
  it("there must be a create product form page", () => {
    const { queryByText, getByRole } = render(<Form />);
    expect(queryByText(/create product/i)).toBeInTheDocument();
    // una forma de refactorizar esto para que sea más escalable:

    expect(
      getByRole("heading", { name: /create product/i })
    ).toBeInTheDocument();
  });
});
```

<p>Nota: La diferencia entre query y get es que con query el No Match no devuelve un error </p>

<p>Nota: Siempre es mejor filtrar por roles ya que se teste la forma en la que el usuario interactua con la aplicacion. El role garantiza que se cumpla el arbol de accesibilidad. Ejemplo: getByRole('button',{name : /submit/i})</p>
