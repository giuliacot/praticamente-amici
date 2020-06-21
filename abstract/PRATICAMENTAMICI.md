## Abstract

`Al tempo degli applicativi web, dei framework che spadroneggiano su una industria in tumulto, gli sviluppatori web invocano il soccorso di un eroe per riconquistare la libertà. Finalmente arriva CSS in JS l'invincibile...`. No ok sto esagerando!
Uno contro l'altro ma praticamente amici, un viaggio su come l'uno (CSS) è diventato il migliore amico dell'altro (JS).

### Presentazione

- Slide 1

### Intro

Un film anni 80, una storia di solidarietà tra Milano e Roma, due mondi contrapposti allora come oggi.

In questa dicotomia, un giovane nipote eredita dal proprio zio l'azienda di carni in scatola. Il giovane non ha alcun interesse nell'impreditoria, la sua passione è il giardinaggio, i fiori.
Alla morte dello zio, arrivano improvvisamente tutte le responsabilità di un imprenditore. Gli viene chiesto di andare a Roma per concludere un affare di vitale importanza per l'azienda, portanto con se una ingente quantità di contanti (trattasi di una mazzetta).
Giunto a Roma incontra in aeroporto Er Monnezza, che lo catapulta in quella Roma fatta di espedienti e di bucatini all'amatriciana.

Accade che la valigetta viene persa, il giovane sospetta subito che sia Er Monnezza il ladro. Concludendo senza entrare nei dettagli, Er monnezza innocente fa di tutto per aiutare il giovane e con la sua scaltrezza riuscirà a recuperare i soldi e salvare l'azienda di famiglia.

Premessa utilizzerò tutto i cliche e stereotipi fin qui consolidati riguardo il CSS e il JS.

- **Slide 2** Identikit CSS

  Renato Pozzetto in Franco Colombo, un giovane che vive nel suo mondo fatto di fiori.

  Il nostro caro CSS è un po' il nostro Franco Colombo, ha una passione ben precisa, è "semplice". Ma i tempi cambiano e nel contesto degli applicativi web rimane importantissimo mitigare i problemi che insorgono in una codebase importante.

- **Slide 3** Identikit JS (Er Monnezza)

  Quinto Cecioni detto Er Monnezza è un po' JS, vive di espedienti, poi tutte quelle spillette sono chiaramente la pletora di librerie, framework sviluppate attorno a lui. Er monnezza è scaltro ma decisamente un buzzurro (si sto definendo buzzurro anche il JS).

- **Slide 4** Una metafora perfetta per descrivere CSS in JS. (Una storia a lieto fine)
  La conclusione del film è che l'uno aiuta l'altro a risolvere le avversità. Franco recupera i suoi soldi ed ottiene il favore senza sganciare la mazzetta tramite l'arguzia di Er monnezza, e Er monnezza può smettere di vivere come un ladruncolo, visto che Franco alla fine regala i soldi necessari per la mazzetta a lui.

---

- **Slide 5** La missione (come nasce questo nuovo paradigma)
  Il CSS nella gestione di grandi codebase (ribadiamo) fa emergere alcune debolezze.

- Maintainance
- Global Namespace
- Programmabilità
- Dead code elimination
- Risoluzione non deterministica (?) generalmente causata nell'incapacità di sfruttare il Cascading effect

Si è vissuto con il terrore del refactoring del css, perchè in grandi progetti spesso l'unica opzione è creare classi o specificità sempre maggiori fino a giungere addirittura ad usare `!important.` Maintainance

Soluzioni:

- CSS methodologies (error prone, profetizzazione)
- Preprocessing (partial scss - modules)

One of the primary benefits of CSS in JS is that it keeps styles encapsulated with markup and avoids specificity concerns.
https://www.styletron.org/concepts

- **Slide n** Ma cosa significa un po' CSS nel JS

  Il nome non ci da molte altre interpretazioni, metto del CSS dentro a del javascript. In realtà alcune soluzioni, come vedremo poi, determinano un insieme di operazioni non necessarie sul file js ma non intendono scrivere letterlamente del CSS dentro al javascript. Per cui potremmo intendere questo gruppo di tecnologie come una serie di operazioni sul CSS tramite Javascript aiutando la separazione e la creazione di uno stile contestuale all'astrazione componente.

  https://media.giphy.com/media/3o7P4F86TAI9Kz7XYk/giphy.gif

- **Slide 6** Microrganismo chiamato componente ❌

Immaginiamo ogni elemento del nostro design system come una cellula.

Style
Structure
Behaviour

CSS-in-JS may be limited to React-like component-based frameworks. Other popular frameworks such as Vue, Svelte, or Angular use other colocation strategies to give developers similar benefits (like scoped CSS and tree-shakeable CSS).

https://www.infoq.com/news/2020/01/css-cssinjs-performance-cost/

- **Slide 7** Punti di forza CSS in JS
- Confidenza cioè paintless maintainance
- Tools di minificazione, di dead code css, post processori autoprefixer inclusi
- Un unico tool - riduzione di switch context no extra tooling

-> Critical css out of the box

- **Slide 8** Client API

A livello di interfaccia API le varie librerie si sono copiate l'un l'altra le soluzioni.
Le ultime arrivate ringraziano esplicitamente di essersi ispirati dalle idee di altre famose librerie Styled Components, Glamouros.

Ogni libreria ha però le implementa in maniera differente.

Le API più utilizzate:

- The css Prop
- css template literale
- Styled Helper to build React components. It allows you to write your components in a similar syntax as `styled-components`
- Object style
- Composition (Mostrare come si possa sovvertire il cascading)

Using Emotion’s composition, you don’t have to think about the order that styles were created because the styles are merged in the order that you use them.

Si sfrutta il nested composition tramite preprocessori light come stylis

Polished

-> Non devi più preoccuparti della creazioni di un sistema di classi BEM ecc ci si concentra sullo stile del componente

Fela -> lo stile è connesso con lo stato del componente

- **Slide n** CSS()
  The css function accepts styles as a template literal, object, or array of objects and returns a class name. It is the foundation of emotion.

- **Slide n** CSS prop

```jsx
const Panel = ({ children }) => {
  return (
    <div
      css={{
        display: "grid",
        gridTemplateColumns: "1fr 1fr 1fr",
        gap: " 0.5rem",
      }}
    >
      {children}
    </div>
  );
};
```

- **Slide n** CSS string

```jsx
const Card = ({ children, color, bg, gradient }) => {
  const style = css`
    color: ${color};
    background: ${bg};
    ${gradient &&
    `background-image: linear-gradient(to top right, ${darken(
      0.15,
      bg
    )}, ${bg});`}
    border: 10px solid #e8ac5f;
    max-width: 250px;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    border-radius: 4px;
    label: emotion-card;
  `;

  return <div css={style}>{children}</div>;
};
```

- **Slide n** Styled

```jsx
const MainTitle = styled.h1({
  fontSize: generateFontSize({ measure: MEASURES["biggest"] }),
  fontFamily: TITLE_FONT,
  fontWeight: "900",
  marginBottom: "1rem",
  textAlign: "left",
});
```

- **Slide n** cx() composizione
  In emotion si parla anche di Composition, cioè la possibilità di avere stile componibile, dove il valore di come viene applicato lo stile è nel modo in cui sono dichiarate le classi, da destra verso sinistra.

```jsx
const ColorfulBg = css`
  background-color: violet;
`;

<CustomTitle
  css={css`
    background-color: #eee;
    ${ColorfulBg};
  `}
  color={name}
>
  CSS in JS libraries
</CustomTitle>;
```

Fondamentale sottolineare che questa funzione permette di sovvertere le regole del CSS
Le classi avranno valore per quelle che verrano elencate nel componente

- **Slide n** Settings CSS in JS

Alcune funzionalità sono supportate solo tramite appositi plugin babel, come per esempio:

- **Slide n** Emotion

CSS in JS ha bisogno di determinate configurazioni oppure si ha bisogno di babel e/o webpack che si andranno ad occupare delle operazioni di bundling.

Nel caso di emotion, alcune feature non necessitano di ulteriori setting, come babel ma per abilitare alcune funzionalità è richiesto, come ad esempio il targetizzare i componenti come comune classi css:

`for emotion components to be targeted like regular CSS selectors when using babel-plugin-emotion.`

```jsx
import styled from "@emotion/styled";

const Child = styled.div`
  color: red;
`;

const Parent = styled.div`
  ${Child} {
    color: green;
  }
`;

render(
  <div>
    <Parent>
      <Child>Green because I am inside a Parent</Child>
    </Parent>
    <Child>Red because I am not inside a Parent</Child>
  </div>
);
```

```jsx
/** @jsx jsx */
import { jsx, css } from "@emotion/core";
import { darken } from "polished";
import { Small } from "./Typography";

const Card = ({ children, color, bg, gradient }) => {
  const style = css`
    color: ${color};
    background: ${bg};
    ${
      gradient &&
      `background-image: linear-gradient(to top right, ${darken(
        0.25,
        bg
      )}, ${bg});`
    }
    box-shadow: 20px 20px ${darken(0.45, bg)};
    max-width: 250px;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    transition: 0.3s ease-in-out all;
    &:hover {
      transform: translateY(-15px);
    }
    label: card; // Emotion properties to get random class and card
  `;

  return <div css={style}>{children}</div>;
};
```

- Mostro come funziona emotion, cioè la classe generata, l'utilizzo di jsx che va a sostituire il react create element !
- lo stile nel dom

- **Slide n** Emotion vs Linaria

  Linaria utilizza custom properties

  `“Zero-runtime” meaning you author your styles in a CSS-in-JS syntax, but what is produced is .css files like any other CSS preprocessor would produce.`

- contro non funge sui browser più vecchi
- super light weight

* **Slide n** CSS modules
  Per alcuni non è considerata una soluzione propriamente CSS nel JS, perchè?

* Elimina il problema del global namespace tramite l'utilizzo di generazione di classi hash
* Mantiene inalterate però la sintassi del CSS e la sua separazione rispetto al componente in un file esterno

https://glenmaddern.com/articles/css-modules

# due tipi di libreria css in js run time (emotion styled) e quelle zero run time (css modules & linaria)

What happens

- Parses the styled component’s tagged template’s CSS rules.
- Generates the new CSS class name (or checks whether it should retain the existing one).
- Preprocesses the styles with stylis.
- Injects the preprocessed CSS into the associated <style> tag in the HTML <head>.

- **Slide N** CSS in JS run time steps analysis

You see, each time you render a single styled component using styled-components or emotion, apart from the obvious React Component that gets created, an additional Context.Consumer is added in order to allow the runtime script (that most CSS-in-JS libraries depend upon) to properly manage the generated styling rules.

All in all, when you create a styled component in an app with a theme, 3 components get created: the obvious StyledXXX component and two (2) additional consumer components. Don’t be too scared, React does its work fast and this won’t be too much of an issue most of the times, but what if we compose multiple styled components in order to create a more complex component?

most CSS-in-JS libraries depend on a runtime that helps them dynamically update the styles of a component.

When the associated React component gets rendered, styled-components:

This library follows a similar approach, with minor differences. Again, it parses the tagged template, preprocesses it with stylis and updates the corresponding style tag. One key difference is that emotion always wraps all components with a ThemeContext.Consumer regardless of whether they are using a theme or not (which explains the screenshots above). Funnily enough, even though it renders more consumer components, it still outperforms styled-components, which denotes that the number of consumer isn’t the biggest contributor to a slow render.

It should also be mentioned, that the all the style tags that get added for each component never get removed at all. This is because the overhead associated with a DOM removal (e.g. browser reflows) is higher than the overhead of just keeping them there.

`This shifts the tool into a totally different category. It’s a developer tool only, rather than a tool where the user of the website pays the price of using it.`
