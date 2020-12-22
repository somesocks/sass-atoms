# sass-atoms

a flexible kit for designing and materializing atomic utility classes, as a base for a sass/css framework.

## why?

The motivation behind any SASS/CSS framework is to eliminate the paradox of choice in styling, and allow a team to put together a consistent, high-quality UI with less effort.  However, a common problem with many CSS frameworks today is that they are _too_ limited.  Frequently, design systems need to be modified and extended in non-standard ways due to a lack flexibilty in the base of the framework:

- "I need an xxl breakpoint, but framework X only supports sm -> xl"
- "How do I add a custom font family to framework X?"

`sass-atoms` addresses this by adding a large number of well-designed 'atoms' (utility classes) that you can use to customize an existing framework, or use as a foundation when creating your own components.

## how to use it

Install the `sass-atoms` package via npm, and your sass code add `@import 'sass-atoms/index.scss'`;

`sass-atoms`, by default, creates _no_ classes, so there is no size overhead to importing it in your code.  All utility classes are generated as a sass placeholder classes (https://sass-lang.com/documentation/style-rules/placeholder-selectors).

When you want to use a utlilty class, you can extend it, like so:

```
  .banner {
    @extend
      %w-p12,
      %d-inline-block,
      %box-border,
      %p-2,
      %p-3--md,
      %p-4--lg,
    ;
  }
```

This banner will have a 100% width, an inline-block display, and different internal padding based on the current breakpoint.

When compiled, this resulting CSS for this banner component looks like:

```
  .banner {
    box-sizing: border-box;
  }

  .banner {
    display: inline-block;
  }

  .banner {
    padding: 1rem;
  }

  @media (min-width: 768px) {
    .banner:not(:root):not(:root) {
      padding: 1.5rem;
    }
  }

  @media (min-width: 992px) {
    .banner:not(:root):not(:root):not(:root) {
      padding: 2rem;
    }
  }

  .banner {
    width: 100%;
  }
```

Due to the way the `@extend` rule works, the result will be a number of small selectors rather than a single large one, but the resulting CSS is still performant and highly compressible.  

If you want to add options to this banner, you can do so by _materializing_ the utility placeholders as real classes for the banner, like so:

```
  .banner {
    @extend
      %w-p12,
      %d-inline-block,
      %box-border,
      %p-2,
      %p-3--md,
      %p-4--lg,
    ;
  }

  $background-options: (
    'bg-primary-100',
    'bg-primary-900',
    'bg-secondary-100',
    'bg-secondary-900',
  );

  // use the `materialize` mixin to generate real classes from placeholders
  // the second argument is a selector to prefix to the class, to add specificity
  @include materialize($background-options, '.banner');
```

The resulting CSS looks like this:
```
.banner.bg-primary-100 {
  background-color: #E0E7FF;
}

.banner.bg-primary-900 {
  background-color: #312E81;
}

.banner.bg-secondary-100 {
  background-color: #DBEAFE;
}

.banner.bg-secondary-900 {
  background-color: #1E3A8A;
}

.banner {
  box-sizing: border-box;
}

.banner {
  display: inline-block;
} 

.banner {
  padding: 1rem;
}

@media (min-width: 768px) {
  .banner:not(:root):not(:root) {
    padding: 1.5rem;
  }
}

@media (min-width: 992px) {
  .banner:not(:root):not(:root):not(:root) {
    padding: 2rem;
  }
}

.banner {
  width: 100%;
}
```

The `materialize` mixin also accepts a map, if you want to alias certain utility classes:

```
  .banner {
    @extend
      %w-p12,
      %d-inline-block,
      %box-border,
      %p-2,
      %p-3--md,
      %p-4--lg,
    ;
  }

  $background-options: (
    'bg-primary-light': 'bg-primary-100',
    'bg-primary-dark': 'bg-primary-900',
    'bg-secondary-light': 'bg-secondary-100',
    'bg-secondary-dark': 'bg-secondary-900',
  );

  // use the `materialize` mixin to generate real classes from placeholders
  // the second argument is a selector to prefix to the class, to add specificity
  @include materialize($background-options, '.banner');
```

Will compile to:
```
.banner.bg-primary-light {
  background-color: #E0E7FF;
}

.banner.bg-primary-dark {
  background-color: #312E81;
}

.banner.bg-secondary-light {
  background-color: #DBEAFE;
}

.banner.bg-secondary-dark {
  background-color: #1E3A8A;
}

.banner {
  box-sizing: border-box;
}

.banner {
  display: inline-block;
}

.banner {
  padding: 1rem;
}

@media (min-width: 768px) {
  .banner:not(:root):not(:root) {
    padding: 1.5rem;
  }
}

@media (min-width: 992px) {
  .banner:not(:root):not(:root):not(:root) {
    padding: 2rem;
  }
}

.banner {
  width: 100%;
}
```

You can also use this to add all default atoms to a class, but this will result in a larger output:

```
  .banner {
    @extend
      %w-p12,
      %d-inline-block,
      %box-border,
      %p-2,
      %p-3--md,
      %p-4--lg,
    ;
  }

  // use the `materialize` mixin to generate real classes from placeholders
  // the second argument is a selector to prefix to the class, to add specificity
  @include materialize($background-color-atoms, '.banner');
```

## atoms reference

### `$align-content-atoms`
`align-content-start align-content-end align-content-center align-content-between align-content-around align-content-evenly`

### `$align-content-atoms--breakpoints`
`align-content-start--sm align-content-start--md align-content-start--lg align-content-start--xl align-content-end--sm align-content-end--md align-content-end--lg align-content-end--xl align-content-center--sm align-content-center--md align-content-center--lg align-content-center--xl align-content-between--sm align-content-between--md align-content-between--lg align-content-between--xl align-content-around--sm align-content-around--md align-content-around--lg align-content-around--xl align-content-evenly--sm align-content-evenly--md align-content-evenly--lg align-content-evenly--xl`

### `$align-content-atoms--states`
`align-content-start--active align-content-start--hover align-content-start--focus align-content-start--disabled align-content-start--enabled align-content-end--active align-content-end--hover align-content-end--focus align-content-end--disabled align-content-end--enabled align-content-center--active align-content-center--hover align-content-center--focus align-content-center--disabled align-content-center--enabled align-content-between--active align-content-between--hover align-content-between--focus align-content-between--disabled align-content-between--enabled align-content-around--active align-content-around--hover align-content-around--focus align-content-around--disabled align-content-around--enabled align-content-evenly--active align-content-evenly--hover align-content-evenly--focus align-content-evenly--disabled align-content-evenly--enabled`

### `$align-items-atoms`
`align-items-start align-items-end align-items-center align-items-baseline align-items-stretch`

### `$align-items-atoms--breakpoints`
`align-items-start--sm align-items-start--md align-items-start--lg align-items-start--xl align-items-end--sm align-items-end--md align-items-end--lg align-items-end--xl align-items-center--sm align-items-center--md align-items-center--lg align-items-center--xl align-items-baseline--sm align-items-baseline--md align-items-baseline--lg align-items-baseline--xl align-items-stretch--sm align-items-stretch--md align-items-stretch--lg align-items-stretch--xl`

### `$align-items-atoms--states`
`align-items-start--active align-items-start--hover align-items-start--focus align-items-start--disabled align-items-start--enabled align-items-end--active align-items-end--hover align-items-end--focus align-items-end--disabled align-items-end--enabled align-items-center--active align-items-center--hover align-items-center--focus align-items-center--disabled align-items-center--enabled align-items-baseline--active align-items-baseline--hover align-items-baseline--focus align-items-baseline--disabled align-items-baseline--enabled align-items-stretch--active align-items-stretch--hover align-items-stretch--focus align-items-stretch--disabled align-items-stretch--enabled`

### `$align-self-atoms`
`align-self-auto align-self-start align-self-end align-self-center align-self-stretch`

### `$align-self-atoms--breakpoints`
`align-self-auto--sm align-self-auto--md align-self-auto--lg align-self-auto--xl align-self-start--sm align-self-start--md align-self-start--lg align-self-start--xl align-self-end--sm align-self-end--md align-self-end--lg align-self-end--xl align-self-center--sm align-self-center--md align-self-center--lg align-self-center--xl align-self-stretch--sm align-self-stretch--md align-self-stretch--lg align-self-stretch--xl`

### `$align-self-atoms--states`
`align-self-auto--active align-self-auto--hover align-self-auto--focus align-self-auto--disabled align-self-auto--enabled align-self-start--active align-self-start--hover align-self-start--focus align-self-start--disabled align-self-start--enabled align-self-end--active align-self-end--hover align-self-end--focus align-self-end--disabled align-self-end--enabled align-self-center--active align-self-center--hover align-self-center--focus align-self-center--disabled align-self-center--enabled align-self-stretch--active align-self-stretch--hover align-self-stretch--focus align-self-stretch--disabled align-self-stretch--enabled`

### `$background-clip-atoms`
`bg-clip-border bg-clip-padding bg-clip-content bg-clip-text`

### `$background-clip-atoms--breakpoints`
`bg-clip-border--sm bg-clip-border--md bg-clip-border--lg bg-clip-border--xl bg-clip-padding--sm bg-clip-padding--md bg-clip-padding--lg bg-clip-padding--xl bg-clip-content--sm bg-clip-content--md bg-clip-content--lg bg-clip-content--xl bg-clip-text--sm bg-clip-text--md bg-clip-text--lg bg-clip-text--xl`

### `$background-clip-atoms--states`
`bg-clip-border--active bg-clip-border--hover bg-clip-border--focus bg-clip-border--disabled bg-clip-border--enabled bg-clip-padding--active bg-clip-padding--hover bg-clip-padding--focus bg-clip-padding--disabled bg-clip-padding--enabled bg-clip-content--active bg-clip-content--hover bg-clip-content--focus bg-clip-content--disabled bg-clip-content--enabled bg-clip-text--active bg-clip-text--hover bg-clip-text--focus bg-clip-text--disabled bg-clip-text--enabled`

### `$background-color-atoms`
`bg-primary-50 bg-primary-100 bg-primary-200 bg-primary-300 bg-primary-400 bg-primary-500 bg-primary-600 bg-primary-700 bg-primary-800 bg-primary-900 bg-secondary-50 bg-secondary-100 bg-secondary-200 bg-secondary-300 bg-secondary-400 bg-secondary-500 bg-secondary-600 bg-secondary-700 bg-secondary-800 bg-secondary-900 bg-link-50 bg-link-100 bg-link-200 bg-link-300 bg-link-400 bg-link-500 bg-link-600 bg-link-700 bg-link-800 bg-link-900 bg-muted-50 bg-muted-100 bg-muted-200 bg-muted-300 bg-muted-400 bg-muted-500 bg-muted-600 bg-muted-700 bg-muted-800 bg-muted-900 bg-success-50 bg-success-100 bg-success-200 bg-success-300 bg-success-400 bg-success-500 bg-success-600 bg-success-700 bg-success-800 bg-success-900 bg-warning-50 bg-warning-100 bg-warning-200 bg-warning-300 bg-warning-400 bg-warning-500 bg-warning-600 bg-warning-700 bg-warning-800 bg-warning-900 bg-error-50 bg-error-100 bg-error-200 bg-error-300 bg-error-400 bg-error-500 bg-error-600 bg-error-700 bg-error-800 bg-error-900 bg-gray-50 bg-gray-100 bg-gray-200 bg-gray-300 bg-gray-400 bg-gray-500 bg-gray-600 bg-gray-700 bg-gray-800 bg-gray-900`

### `$background-color-atoms--breakpoints`
`bg-primary-50--sm bg-primary-50--md bg-primary-50--lg bg-primary-50--xl bg-primary-100--sm bg-primary-100--md bg-primary-100--lg bg-primary-100--xl bg-primary-200--sm bg-primary-200--md bg-primary-200--lg bg-primary-200--xl bg-primary-300--sm bg-primary-300--md bg-primary-300--lg bg-primary-300--xl bg-primary-400--sm bg-primary-400--md bg-primary-400--lg bg-primary-400--xl bg-primary-500--sm bg-primary-500--md bg-primary-500--lg bg-primary-500--xl bg-primary-600--sm bg-primary-600--md bg-primary-600--lg bg-primary-600--xl bg-primary-700--sm bg-primary-700--md bg-primary-700--lg bg-primary-700--xl bg-primary-800--sm bg-primary-800--md bg-primary-800--lg bg-primary-800--xl bg-primary-900--sm bg-primary-900--md bg-primary-900--lg bg-primary-900--xl bg-secondary-50--sm bg-secondary-50--md bg-secondary-50--lg bg-secondary-50--xl bg-secondary-100--sm bg-secondary-100--md bg-secondary-100--lg bg-secondary-100--xl bg-secondary-200--sm bg-secondary-200--md bg-secondary-200--lg bg-secondary-200--xl bg-secondary-300--sm bg-secondary-300--md bg-secondary-300--lg bg-secondary-300--xl bg-secondary-400--sm bg-secondary-400--md bg-secondary-400--lg bg-secondary-400--xl bg-secondary-500--sm bg-secondary-500--md bg-secondary-500--lg bg-secondary-500--xl bg-secondary-600--sm bg-secondary-600--md bg-secondary-600--lg bg-secondary-600--xl bg-secondary-700--sm bg-secondary-700--md bg-secondary-700--lg bg-secondary-700--xl bg-secondary-800--sm bg-secondary-800--md bg-secondary-800--lg bg-secondary-800--xl bg-secondary-900--sm bg-secondary-900--md bg-secondary-900--lg bg-secondary-900--xl bg-link-50--sm bg-link-50--md bg-link-50--lg bg-link-50--xl bg-link-100--sm bg-link-100--md bg-link-100--lg bg-link-100--xl bg-link-200--sm bg-link-200--md bg-link-200--lg bg-link-200--xl bg-link-300--sm bg-link-300--md bg-link-300--lg bg-link-300--xl bg-link-400--sm bg-link-400--md bg-link-400--lg bg-link-400--xl bg-link-500--sm bg-link-500--md bg-link-500--lg bg-link-500--xl bg-link-600--sm bg-link-600--md bg-link-600--lg bg-link-600--xl bg-link-700--sm bg-link-700--md bg-link-700--lg bg-link-700--xl bg-link-800--sm bg-link-800--md bg-link-800--lg bg-link-800--xl bg-link-900--sm bg-link-900--md bg-link-900--lg bg-link-900--xl bg-muted-50--sm bg-muted-50--md bg-muted-50--lg bg-muted-50--xl bg-muted-100--sm bg-muted-100--md bg-muted-100--lg bg-muted-100--xl bg-muted-200--sm bg-muted-200--md bg-muted-200--lg bg-muted-200--xl bg-muted-300--sm bg-muted-300--md bg-muted-300--lg bg-muted-300--xl bg-muted-400--sm bg-muted-400--md bg-muted-400--lg bg-muted-400--xl bg-muted-500--sm bg-muted-500--md bg-muted-500--lg bg-muted-500--xl bg-muted-600--sm bg-muted-600--md bg-muted-600--lg bg-muted-600--xl bg-muted-700--sm bg-muted-700--md bg-muted-700--lg bg-muted-700--xl bg-muted-800--sm bg-muted-800--md bg-muted-800--lg bg-muted-800--xl bg-muted-900--sm bg-muted-900--md bg-muted-900--lg bg-muted-900--xl bg-success-50--sm bg-success-50--md bg-success-50--lg bg-success-50--xl bg-success-100--sm bg-success-100--md bg-success-100--lg bg-success-100--xl bg-success-200--sm bg-success-200--md bg-success-200--lg bg-success-200--xl bg-success-300--sm bg-success-300--md bg-success-300--lg bg-success-300--xl bg-success-400--sm bg-success-400--md bg-success-400--lg bg-success-400--xl bg-success-500--sm bg-success-500--md bg-success-500--lg bg-success-500--xl bg-success-600--sm bg-success-600--md bg-success-600--lg bg-success-600--xl bg-success-700--sm bg-success-700--md bg-success-700--lg bg-success-700--xl bg-success-800--sm bg-success-800--md bg-success-800--lg bg-success-800--xl bg-success-900--sm bg-success-900--md bg-success-900--lg bg-success-900--xl bg-warning-50--sm bg-warning-50--md bg-warning-50--lg bg-warning-50--xl bg-warning-100--sm bg-warning-100--md bg-warning-100--lg bg-warning-100--xl bg-warning-200--sm bg-warning-200--md bg-warning-200--lg bg-warning-200--xl bg-warning-300--sm bg-warning-300--md bg-warning-300--lg bg-warning-300--xl bg-warning-400--sm bg-warning-400--md bg-warning-400--lg bg-warning-400--xl bg-warning-500--sm bg-warning-500--md bg-warning-500--lg bg-warning-500--xl bg-warning-600--sm bg-warning-600--md bg-warning-600--lg bg-warning-600--xl bg-warning-700--sm bg-warning-700--md bg-warning-700--lg bg-warning-700--xl bg-warning-800--sm bg-warning-800--md bg-warning-800--lg bg-warning-800--xl bg-warning-900--sm bg-warning-900--md bg-warning-900--lg bg-warning-900--xl bg-error-50--sm bg-error-50--md bg-error-50--lg bg-error-50--xl bg-error-100--sm bg-error-100--md bg-error-100--lg bg-error-100--xl bg-error-200--sm bg-error-200--md bg-error-200--lg bg-error-200--xl bg-error-300--sm bg-error-300--md bg-error-300--lg bg-error-300--xl bg-error-400--sm bg-error-400--md bg-error-400--lg bg-error-400--xl bg-error-500--sm bg-error-500--md bg-error-500--lg bg-error-500--xl bg-error-600--sm bg-error-600--md bg-error-600--lg bg-error-600--xl bg-error-700--sm bg-error-700--md bg-error-700--lg bg-error-700--xl bg-error-800--sm bg-error-800--md bg-error-800--lg bg-error-800--xl bg-error-900--sm bg-error-900--md bg-error-900--lg bg-error-900--xl bg-gray-50--sm bg-gray-50--md bg-gray-50--lg bg-gray-50--xl bg-gray-100--sm bg-gray-100--md bg-gray-100--lg bg-gray-100--xl bg-gray-200--sm bg-gray-200--md bg-gray-200--lg bg-gray-200--xl bg-gray-300--sm bg-gray-300--md bg-gray-300--lg bg-gray-300--xl bg-gray-400--sm bg-gray-400--md bg-gray-400--lg bg-gray-400--xl bg-gray-500--sm bg-gray-500--md bg-gray-500--lg bg-gray-500--xl bg-gray-600--sm bg-gray-600--md bg-gray-600--lg bg-gray-600--xl bg-gray-700--sm bg-gray-700--md bg-gray-700--lg bg-gray-700--xl bg-gray-800--sm bg-gray-800--md bg-gray-800--lg bg-gray-800--xl bg-gray-900--sm bg-gray-900--md bg-gray-900--lg bg-gray-900--xl`

### `$background-color-atoms--states`
`bg-primary-50--active bg-primary-50--hover bg-primary-50--focus bg-primary-50--disabled bg-primary-50--enabled bg-primary-100--active bg-primary-100--hover bg-primary-100--focus bg-primary-100--disabled bg-primary-100--enabled bg-primary-200--active bg-primary-200--hover bg-primary-200--focus bg-primary-200--disabled bg-primary-200--enabled bg-primary-300--active bg-primary-300--hover bg-primary-300--focus bg-primary-300--disabled bg-primary-300--enabled bg-primary-400--active bg-primary-400--hover bg-primary-400--focus bg-primary-400--disabled bg-primary-400--enabled bg-primary-500--active bg-primary-500--hover bg-primary-500--focus bg-primary-500--disabled bg-primary-500--enabled bg-primary-600--active bg-primary-600--hover bg-primary-600--focus bg-primary-600--disabled bg-primary-600--enabled bg-primary-700--active bg-primary-700--hover bg-primary-700--focus bg-primary-700--disabled bg-primary-700--enabled bg-primary-800--active bg-primary-800--hover bg-primary-800--focus bg-primary-800--disabled bg-primary-800--enabled bg-primary-900--active bg-primary-900--hover bg-primary-900--focus bg-primary-900--disabled bg-primary-900--enabled bg-secondary-50--active bg-secondary-50--hover bg-secondary-50--focus bg-secondary-50--disabled bg-secondary-50--enabled bg-secondary-100--active bg-secondary-100--hover bg-secondary-100--focus bg-secondary-100--disabled bg-secondary-100--enabled bg-secondary-200--active bg-secondary-200--hover bg-secondary-200--focus bg-secondary-200--disabled bg-secondary-200--enabled bg-secondary-300--active bg-secondary-300--hover bg-secondary-300--focus bg-secondary-300--disabled bg-secondary-300--enabled bg-secondary-400--active bg-secondary-400--hover bg-secondary-400--focus bg-secondary-400--disabled bg-secondary-400--enabled bg-secondary-500--active bg-secondary-500--hover bg-secondary-500--focus bg-secondary-500--disabled bg-secondary-500--enabled bg-secondary-600--active bg-secondary-600--hover bg-secondary-600--focus bg-secondary-600--disabled bg-secondary-600--enabled bg-secondary-700--active bg-secondary-700--hover bg-secondary-700--focus bg-secondary-700--disabled bg-secondary-700--enabled bg-secondary-800--active bg-secondary-800--hover bg-secondary-800--focus bg-secondary-800--disabled bg-secondary-800--enabled bg-secondary-900--active bg-secondary-900--hover bg-secondary-900--focus bg-secondary-900--disabled bg-secondary-900--enabled bg-link-50--active bg-link-50--hover bg-link-50--focus bg-link-50--disabled bg-link-50--enabled bg-link-100--active bg-link-100--hover bg-link-100--focus bg-link-100--disabled bg-link-100--enabled bg-link-200--active bg-link-200--hover bg-link-200--focus bg-link-200--disabled bg-link-200--enabled bg-link-300--active bg-link-300--hover bg-link-300--focus bg-link-300--disabled bg-link-300--enabled bg-link-400--active bg-link-400--hover bg-link-400--focus bg-link-400--disabled bg-link-400--enabled bg-link-500--active bg-link-500--hover bg-link-500--focus bg-link-500--disabled bg-link-500--enabled bg-link-600--active bg-link-600--hover bg-link-600--focus bg-link-600--disabled bg-link-600--enabled bg-link-700--active bg-link-700--hover bg-link-700--focus bg-link-700--disabled bg-link-700--enabled bg-link-800--active bg-link-800--hover bg-link-800--focus bg-link-800--disabled bg-link-800--enabled bg-link-900--active bg-link-900--hover bg-link-900--focus bg-link-900--disabled bg-link-900--enabled bg-muted-50--active bg-muted-50--hover bg-muted-50--focus bg-muted-50--disabled bg-muted-50--enabled bg-muted-100--active bg-muted-100--hover bg-muted-100--focus bg-muted-100--disabled bg-muted-100--enabled bg-muted-200--active bg-muted-200--hover bg-muted-200--focus bg-muted-200--disabled bg-muted-200--enabled bg-muted-300--active bg-muted-300--hover bg-muted-300--focus bg-muted-300--disabled bg-muted-300--enabled bg-muted-400--active bg-muted-400--hover bg-muted-400--focus bg-muted-400--disabled bg-muted-400--enabled bg-muted-500--active bg-muted-500--hover bg-muted-500--focus bg-muted-500--disabled bg-muted-500--enabled bg-muted-600--active bg-muted-600--hover bg-muted-600--focus bg-muted-600--disabled bg-muted-600--enabled bg-muted-700--active bg-muted-700--hover bg-muted-700--focus bg-muted-700--disabled bg-muted-700--enabled bg-muted-800--active bg-muted-800--hover bg-muted-800--focus bg-muted-800--disabled bg-muted-800--enabled bg-muted-900--active bg-muted-900--hover bg-muted-900--focus bg-muted-900--disabled bg-muted-900--enabled bg-success-50--active bg-success-50--hover bg-success-50--focus bg-success-50--disabled bg-success-50--enabled bg-success-100--active bg-success-100--hover bg-success-100--focus bg-success-100--disabled bg-success-100--enabled bg-success-200--active bg-success-200--hover bg-success-200--focus bg-success-200--disabled bg-success-200--enabled bg-success-300--active bg-success-300--hover bg-success-300--focus bg-success-300--disabled bg-success-300--enabled bg-success-400--active bg-success-400--hover bg-success-400--focus bg-success-400--disabled bg-success-400--enabled bg-success-500--active bg-success-500--hover bg-success-500--focus bg-success-500--disabled bg-success-500--enabled bg-success-600--active bg-success-600--hover bg-success-600--focus bg-success-600--disabled bg-success-600--enabled bg-success-700--active bg-success-700--hover bg-success-700--focus bg-success-700--disabled bg-success-700--enabled bg-success-800--active bg-success-800--hover bg-success-800--focus bg-success-800--disabled bg-success-800--enabled bg-success-900--active bg-success-900--hover bg-success-900--focus bg-success-900--disabled bg-success-900--enabled bg-warning-50--active bg-warning-50--hover bg-warning-50--focus bg-warning-50--disabled bg-warning-50--enabled bg-warning-100--active bg-warning-100--hover bg-warning-100--focus bg-warning-100--disabled bg-warning-100--enabled bg-warning-200--active bg-warning-200--hover bg-warning-200--focus bg-warning-200--disabled bg-warning-200--enabled bg-warning-300--active bg-warning-300--hover bg-warning-300--focus bg-warning-300--disabled bg-warning-300--enabled bg-warning-400--active bg-warning-400--hover bg-warning-400--focus bg-warning-400--disabled bg-warning-400--enabled bg-warning-500--active bg-warning-500--hover bg-warning-500--focus bg-warning-500--disabled bg-warning-500--enabled bg-warning-600--active bg-warning-600--hover bg-warning-600--focus bg-warning-600--disabled bg-warning-600--enabled bg-warning-700--active bg-warning-700--hover bg-warning-700--focus bg-warning-700--disabled bg-warning-700--enabled bg-warning-800--active bg-warning-800--hover bg-warning-800--focus bg-warning-800--disabled bg-warning-800--enabled bg-warning-900--active bg-warning-900--hover bg-warning-900--focus bg-warning-900--disabled bg-warning-900--enabled bg-error-50--active bg-error-50--hover bg-error-50--focus bg-error-50--disabled bg-error-50--enabled bg-error-100--active bg-error-100--hover bg-error-100--focus bg-error-100--disabled bg-error-100--enabled bg-error-200--active bg-error-200--hover bg-error-200--focus bg-error-200--disabled bg-error-200--enabled bg-error-300--active bg-error-300--hover bg-error-300--focus bg-error-300--disabled bg-error-300--enabled bg-error-400--active bg-error-400--hover bg-error-400--focus bg-error-400--disabled bg-error-400--enabled bg-error-500--active bg-error-500--hover bg-error-500--focus bg-error-500--disabled bg-error-500--enabled bg-error-600--active bg-error-600--hover bg-error-600--focus bg-error-600--disabled bg-error-600--enabled bg-error-700--active bg-error-700--hover bg-error-700--focus bg-error-700--disabled bg-error-700--enabled bg-error-800--active bg-error-800--hover bg-error-800--focus bg-error-800--disabled bg-error-800--enabled bg-error-900--active bg-error-900--hover bg-error-900--focus bg-error-900--disabled bg-error-900--enabled bg-gray-50--active bg-gray-50--hover bg-gray-50--focus bg-gray-50--disabled bg-gray-50--enabled bg-gray-100--active bg-gray-100--hover bg-gray-100--focus bg-gray-100--disabled bg-gray-100--enabled bg-gray-200--active bg-gray-200--hover bg-gray-200--focus bg-gray-200--disabled bg-gray-200--enabled bg-gray-300--active bg-gray-300--hover bg-gray-300--focus bg-gray-300--disabled bg-gray-300--enabled bg-gray-400--active bg-gray-400--hover bg-gray-400--focus bg-gray-400--disabled bg-gray-400--enabled bg-gray-500--active bg-gray-500--hover bg-gray-500--focus bg-gray-500--disabled bg-gray-500--enabled bg-gray-600--active bg-gray-600--hover bg-gray-600--focus bg-gray-600--disabled bg-gray-600--enabled bg-gray-700--active bg-gray-700--hover bg-gray-700--focus bg-gray-700--disabled bg-gray-700--enabled bg-gray-800--active bg-gray-800--hover bg-gray-800--focus bg-gray-800--disabled bg-gray-800--enabled bg-gray-900--active bg-gray-900--hover bg-gray-900--focus bg-gray-900--disabled bg-gray-900--enabled`

### `$background-position-atoms`
`bg-top-left bg-top bg-top-right bg-left bg-center bg-right bg-bottom-left bg-bottom bg-bottom-right`

### `$background-position-atoms--breakpoints`
`bg-top-left--sm bg-top-left--md bg-top-left--lg bg-top-left--xl bg-top--sm bg-top--md bg-top--lg bg-top--xl bg-top-right--sm bg-top-right--md bg-top-right--lg bg-top-right--xl bg-left--sm bg-left--md bg-left--lg bg-left--xl bg-center--sm bg-center--md bg-center--lg bg-center--xl bg-right--sm bg-right--md bg-right--lg bg-right--xl bg-bottom-left--sm bg-bottom-left--md bg-bottom-left--lg bg-bottom-left--xl bg-bottom--sm bg-bottom--md bg-bottom--lg bg-bottom--xl bg-bottom-right--sm bg-bottom-right--md bg-bottom-right--lg bg-bottom-right--xl`

### `$background-position-atoms--states`
`bg-top-left--active bg-top-left--hover bg-top-left--focus bg-top-left--disabled bg-top-left--enabled bg-top--active bg-top--hover bg-top--focus bg-top--disabled bg-top--enabled bg-top-right--active bg-top-right--hover bg-top-right--focus bg-top-right--disabled bg-top-right--enabled bg-left--active bg-left--hover bg-left--focus bg-left--disabled bg-left--enabled bg-center--active bg-center--hover bg-center--focus bg-center--disabled bg-center--enabled bg-right--active bg-right--hover bg-right--focus bg-right--disabled bg-right--enabled bg-bottom-left--active bg-bottom-left--hover bg-bottom-left--focus bg-bottom-left--disabled bg-bottom-left--enabled bg-bottom--active bg-bottom--hover bg-bottom--focus bg-bottom--disabled bg-bottom--enabled bg-bottom-right--active bg-bottom-right--hover bg-bottom-right--focus bg-bottom-right--disabled bg-bottom-right--enabled`

### `$background-repeat-atoms`
`bg-repeat bg-no-repeat bg-repeat-x bg-repeat-y`

### `$background-repeat-atoms--breakpoints`
`bg-repeat--sm bg-repeat--md bg-repeat--lg bg-repeat--xl bg-no-repeat--sm bg-no-repeat--md bg-no-repeat--lg bg-no-repeat--xl bg-repeat-x--sm bg-repeat-x--md bg-repeat-x--lg bg-repeat-x--xl bg-repeat-y--sm bg-repeat-y--md bg-repeat-y--lg bg-repeat-y--xl`

### `$background-repeat-atoms--states`
`bg-repeat--active bg-repeat--hover bg-repeat--focus bg-repeat--disabled bg-repeat--enabled bg-no-repeat--active bg-no-repeat--hover bg-no-repeat--focus bg-no-repeat--disabled bg-no-repeat--enabled bg-repeat-x--active bg-repeat-x--hover bg-repeat-x--focus bg-repeat-x--disabled bg-repeat-x--enabled bg-repeat-y--active bg-repeat-y--hover bg-repeat-y--focus bg-repeat-y--disabled bg-repeat-y--enabled`

### `$background-size-atoms`
`bg-auto bg-cover bg-contain`

### `$background-size-atoms--breakpoints`
`bg-auto--sm bg-auto--md bg-auto--lg bg-auto--xl bg-cover--sm bg-cover--md bg-cover--lg bg-cover--xl bg-contain--sm bg-contain--md bg-contain--lg bg-contain--xl`

### `$background-size-atoms--states`
`bg-auto--active bg-auto--hover bg-auto--focus bg-auto--disabled bg-auto--enabled bg-cover--active bg-cover--hover bg-cover--focus bg-cover--disabled bg-cover--enabled bg-contain--active bg-contain--hover bg-contain--focus bg-contain--disabled bg-contain--enabled`

### `$border-color-atoms`
`border-primary-50 border-primary-100 border-primary-200 border-primary-300 border-primary-400 border-primary-500 border-primary-600 border-primary-700 border-primary-800 border-primary-900 border-secondary-50 border-secondary-100 border-secondary-200 border-secondary-300 border-secondary-400 border-secondary-500 border-secondary-600 border-secondary-700 border-secondary-800 border-secondary-900 border-link-50 border-link-100 border-link-200 border-link-300 border-link-400 border-link-500 border-link-600 border-link-700 border-link-800 border-link-900 border-muted-50 border-muted-100 border-muted-200 border-muted-300 border-muted-400 border-muted-500 border-muted-600 border-muted-700 border-muted-800 border-muted-900 border-success-50 border-success-100 border-success-200 border-success-300 border-success-400 border-success-500 border-success-600 border-success-700 border-success-800 border-success-900 border-warning-50 border-warning-100 border-warning-200 border-warning-300 border-warning-400 border-warning-500 border-warning-600 border-warning-700 border-warning-800 border-warning-900 border-error-50 border-error-100 border-error-200 border-error-300 border-error-400 border-error-500 border-error-600 border-error-700 border-error-800 border-error-900 border-gray-50 border-gray-100 border-gray-200 border-gray-300 border-gray-400 border-gray-500 border-gray-600 border-gray-700 border-gray-800 border-gray-900`

### `$border-color-atoms--breakpoints`
`border-primary-50--sm border-primary-50--md border-primary-50--lg border-primary-50--xl border-primary-100--sm border-primary-100--md border-primary-100--lg border-primary-100--xl border-primary-200--sm border-primary-200--md border-primary-200--lg border-primary-200--xl border-primary-300--sm border-primary-300--md border-primary-300--lg border-primary-300--xl border-primary-400--sm border-primary-400--md border-primary-400--lg border-primary-400--xl border-primary-500--sm border-primary-500--md border-primary-500--lg border-primary-500--xl border-primary-600--sm border-primary-600--md border-primary-600--lg border-primary-600--xl border-primary-700--sm border-primary-700--md border-primary-700--lg border-primary-700--xl border-primary-800--sm border-primary-800--md border-primary-800--lg border-primary-800--xl border-primary-900--sm border-primary-900--md border-primary-900--lg border-primary-900--xl border-secondary-50--sm border-secondary-50--md border-secondary-50--lg border-secondary-50--xl border-secondary-100--sm border-secondary-100--md border-secondary-100--lg border-secondary-100--xl border-secondary-200--sm border-secondary-200--md border-secondary-200--lg border-secondary-200--xl border-secondary-300--sm border-secondary-300--md border-secondary-300--lg border-secondary-300--xl border-secondary-400--sm border-secondary-400--md border-secondary-400--lg border-secondary-400--xl border-secondary-500--sm border-secondary-500--md border-secondary-500--lg border-secondary-500--xl border-secondary-600--sm border-secondary-600--md border-secondary-600--lg border-secondary-600--xl border-secondary-700--sm border-secondary-700--md border-secondary-700--lg border-secondary-700--xl border-secondary-800--sm border-secondary-800--md border-secondary-800--lg border-secondary-800--xl border-secondary-900--sm border-secondary-900--md border-secondary-900--lg border-secondary-900--xl border-link-50--sm border-link-50--md border-link-50--lg border-link-50--xl border-link-100--sm border-link-100--md border-link-100--lg border-link-100--xl border-link-200--sm border-link-200--md border-link-200--lg border-link-200--xl border-link-300--sm border-link-300--md border-link-300--lg border-link-300--xl border-link-400--sm border-link-400--md border-link-400--lg border-link-400--xl border-link-500--sm border-link-500--md border-link-500--lg border-link-500--xl border-link-600--sm border-link-600--md border-link-600--lg border-link-600--xl border-link-700--sm border-link-700--md border-link-700--lg border-link-700--xl border-link-800--sm border-link-800--md border-link-800--lg border-link-800--xl border-link-900--sm border-link-900--md border-link-900--lg border-link-900--xl border-muted-50--sm border-muted-50--md border-muted-50--lg border-muted-50--xl border-muted-100--sm border-muted-100--md border-muted-100--lg border-muted-100--xl border-muted-200--sm border-muted-200--md border-muted-200--lg border-muted-200--xl border-muted-300--sm border-muted-300--md border-muted-300--lg border-muted-300--xl border-muted-400--sm border-muted-400--md border-muted-400--lg border-muted-400--xl border-muted-500--sm border-muted-500--md border-muted-500--lg border-muted-500--xl border-muted-600--sm border-muted-600--md border-muted-600--lg border-muted-600--xl border-muted-700--sm border-muted-700--md border-muted-700--lg border-muted-700--xl border-muted-800--sm border-muted-800--md border-muted-800--lg border-muted-800--xl border-muted-900--sm border-muted-900--md border-muted-900--lg border-muted-900--xl border-success-50--sm border-success-50--md border-success-50--lg border-success-50--xl border-success-100--sm border-success-100--md border-success-100--lg border-success-100--xl border-success-200--sm border-success-200--md border-success-200--lg border-success-200--xl border-success-300--sm border-success-300--md border-success-300--lg border-success-300--xl border-success-400--sm border-success-400--md border-success-400--lg border-success-400--xl border-success-500--sm border-success-500--md border-success-500--lg border-success-500--xl border-success-600--sm border-success-600--md border-success-600--lg border-success-600--xl border-success-700--sm border-success-700--md border-success-700--lg border-success-700--xl border-success-800--sm border-success-800--md border-success-800--lg border-success-800--xl border-success-900--sm border-success-900--md border-success-900--lg border-success-900--xl border-warning-50--sm border-warning-50--md border-warning-50--lg border-warning-50--xl border-warning-100--sm border-warning-100--md border-warning-100--lg border-warning-100--xl border-warning-200--sm border-warning-200--md border-warning-200--lg border-warning-200--xl border-warning-300--sm border-warning-300--md border-warning-300--lg border-warning-300--xl border-warning-400--sm border-warning-400--md border-warning-400--lg border-warning-400--xl border-warning-500--sm border-warning-500--md border-warning-500--lg border-warning-500--xl border-warning-600--sm border-warning-600--md border-warning-600--lg border-warning-600--xl border-warning-700--sm border-warning-700--md border-warning-700--lg border-warning-700--xl border-warning-800--sm border-warning-800--md border-warning-800--lg border-warning-800--xl border-warning-900--sm border-warning-900--md border-warning-900--lg border-warning-900--xl border-error-50--sm border-error-50--md border-error-50--lg border-error-50--xl border-error-100--sm border-error-100--md border-error-100--lg border-error-100--xl border-error-200--sm border-error-200--md border-error-200--lg border-error-200--xl border-error-300--sm border-error-300--md border-error-300--lg border-error-300--xl border-error-400--sm border-error-400--md border-error-400--lg border-error-400--xl border-error-500--sm border-error-500--md border-error-500--lg border-error-500--xl border-error-600--sm border-error-600--md border-error-600--lg border-error-600--xl border-error-700--sm border-error-700--md border-error-700--lg border-error-700--xl border-error-800--sm border-error-800--md border-error-800--lg border-error-800--xl border-error-900--sm border-error-900--md border-error-900--lg border-error-900--xl border-gray-50--sm border-gray-50--md border-gray-50--lg border-gray-50--xl border-gray-100--sm border-gray-100--md border-gray-100--lg border-gray-100--xl border-gray-200--sm border-gray-200--md border-gray-200--lg border-gray-200--xl border-gray-300--sm border-gray-300--md border-gray-300--lg border-gray-300--xl border-gray-400--sm border-gray-400--md border-gray-400--lg border-gray-400--xl border-gray-500--sm border-gray-500--md border-gray-500--lg border-gray-500--xl border-gray-600--sm border-gray-600--md border-gray-600--lg border-gray-600--xl border-gray-700--sm border-gray-700--md border-gray-700--lg border-gray-700--xl border-gray-800--sm border-gray-800--md border-gray-800--lg border-gray-800--xl border-gray-900--sm border-gray-900--md border-gray-900--lg border-gray-900--xl`

### `$border-color-atoms--states`
`border-primary-50--active border-primary-50--hover border-primary-50--focus border-primary-50--disabled border-primary-50--enabled border-primary-100--active border-primary-100--hover border-primary-100--focus border-primary-100--disabled border-primary-100--enabled border-primary-200--active border-primary-200--hover border-primary-200--focus border-primary-200--disabled border-primary-200--enabled border-primary-300--active border-primary-300--hover border-primary-300--focus border-primary-300--disabled border-primary-300--enabled border-primary-400--active border-primary-400--hover border-primary-400--focus border-primary-400--disabled border-primary-400--enabled border-primary-500--active border-primary-500--hover border-primary-500--focus border-primary-500--disabled border-primary-500--enabled border-primary-600--active border-primary-600--hover border-primary-600--focus border-primary-600--disabled border-primary-600--enabled border-primary-700--active border-primary-700--hover border-primary-700--focus border-primary-700--disabled border-primary-700--enabled border-primary-800--active border-primary-800--hover border-primary-800--focus border-primary-800--disabled border-primary-800--enabled border-primary-900--active border-primary-900--hover border-primary-900--focus border-primary-900--disabled border-primary-900--enabled border-secondary-50--active border-secondary-50--hover border-secondary-50--focus border-secondary-50--disabled border-secondary-50--enabled border-secondary-100--active border-secondary-100--hover border-secondary-100--focus border-secondary-100--disabled border-secondary-100--enabled border-secondary-200--active border-secondary-200--hover border-secondary-200--focus border-secondary-200--disabled border-secondary-200--enabled border-secondary-300--active border-secondary-300--hover border-secondary-300--focus border-secondary-300--disabled border-secondary-300--enabled border-secondary-400--active border-secondary-400--hover border-secondary-400--focus border-secondary-400--disabled border-secondary-400--enabled border-secondary-500--active border-secondary-500--hover border-secondary-500--focus border-secondary-500--disabled border-secondary-500--enabled border-secondary-600--active border-secondary-600--hover border-secondary-600--focus border-secondary-600--disabled border-secondary-600--enabled border-secondary-700--active border-secondary-700--hover border-secondary-700--focus border-secondary-700--disabled border-secondary-700--enabled border-secondary-800--active border-secondary-800--hover border-secondary-800--focus border-secondary-800--disabled border-secondary-800--enabled border-secondary-900--active border-secondary-900--hover border-secondary-900--focus border-secondary-900--disabled border-secondary-900--enabled border-link-50--active border-link-50--hover border-link-50--focus border-link-50--disabled border-link-50--enabled border-link-100--active border-link-100--hover border-link-100--focus border-link-100--disabled border-link-100--enabled border-link-200--active border-link-200--hover border-link-200--focus border-link-200--disabled border-link-200--enabled border-link-300--active border-link-300--hover border-link-300--focus border-link-300--disabled border-link-300--enabled border-link-400--active border-link-400--hover border-link-400--focus border-link-400--disabled border-link-400--enabled border-link-500--active border-link-500--hover border-link-500--focus border-link-500--disabled border-link-500--enabled border-link-600--active border-link-600--hover border-link-600--focus border-link-600--disabled border-link-600--enabled border-link-700--active border-link-700--hover border-link-700--focus border-link-700--disabled border-link-700--enabled border-link-800--active border-link-800--hover border-link-800--focus border-link-800--disabled border-link-800--enabled border-link-900--active border-link-900--hover border-link-900--focus border-link-900--disabled border-link-900--enabled border-muted-50--active border-muted-50--hover border-muted-50--focus border-muted-50--disabled border-muted-50--enabled border-muted-100--active border-muted-100--hover border-muted-100--focus border-muted-100--disabled border-muted-100--enabled border-muted-200--active border-muted-200--hover border-muted-200--focus border-muted-200--disabled border-muted-200--enabled border-muted-300--active border-muted-300--hover border-muted-300--focus border-muted-300--disabled border-muted-300--enabled border-muted-400--active border-muted-400--hover border-muted-400--focus border-muted-400--disabled border-muted-400--enabled border-muted-500--active border-muted-500--hover border-muted-500--focus border-muted-500--disabled border-muted-500--enabled border-muted-600--active border-muted-600--hover border-muted-600--focus border-muted-600--disabled border-muted-600--enabled border-muted-700--active border-muted-700--hover border-muted-700--focus border-muted-700--disabled border-muted-700--enabled border-muted-800--active border-muted-800--hover border-muted-800--focus border-muted-800--disabled border-muted-800--enabled border-muted-900--active border-muted-900--hover border-muted-900--focus border-muted-900--disabled border-muted-900--enabled border-success-50--active border-success-50--hover border-success-50--focus border-success-50--disabled border-success-50--enabled border-success-100--active border-success-100--hover border-success-100--focus border-success-100--disabled border-success-100--enabled border-success-200--active border-success-200--hover border-success-200--focus border-success-200--disabled border-success-200--enabled border-success-300--active border-success-300--hover border-success-300--focus border-success-300--disabled border-success-300--enabled border-success-400--active border-success-400--hover border-success-400--focus border-success-400--disabled border-success-400--enabled border-success-500--active border-success-500--hover border-success-500--focus border-success-500--disabled border-success-500--enabled border-success-600--active border-success-600--hover border-success-600--focus border-success-600--disabled border-success-600--enabled border-success-700--active border-success-700--hover border-success-700--focus border-success-700--disabled border-success-700--enabled border-success-800--active border-success-800--hover border-success-800--focus border-success-800--disabled border-success-800--enabled border-success-900--active border-success-900--hover border-success-900--focus border-success-900--disabled border-success-900--enabled border-warning-50--active border-warning-50--hover border-warning-50--focus border-warning-50--disabled border-warning-50--enabled border-warning-100--active border-warning-100--hover border-warning-100--focus border-warning-100--disabled border-warning-100--enabled border-warning-200--active border-warning-200--hover border-warning-200--focus border-warning-200--disabled border-warning-200--enabled border-warning-300--active border-warning-300--hover border-warning-300--focus border-warning-300--disabled border-warning-300--enabled border-warning-400--active border-warning-400--hover border-warning-400--focus border-warning-400--disabled border-warning-400--enabled border-warning-500--active border-warning-500--hover border-warning-500--focus border-warning-500--disabled border-warning-500--enabled border-warning-600--active border-warning-600--hover border-warning-600--focus border-warning-600--disabled border-warning-600--enabled border-warning-700--active border-warning-700--hover border-warning-700--focus border-warning-700--disabled border-warning-700--enabled border-warning-800--active border-warning-800--hover border-warning-800--focus border-warning-800--disabled border-warning-800--enabled border-warning-900--active border-warning-900--hover border-warning-900--focus border-warning-900--disabled border-warning-900--enabled border-error-50--active border-error-50--hover border-error-50--focus border-error-50--disabled border-error-50--enabled border-error-100--active border-error-100--hover border-error-100--focus border-error-100--disabled border-error-100--enabled border-error-200--active border-error-200--hover border-error-200--focus border-error-200--disabled border-error-200--enabled border-error-300--active border-error-300--hover border-error-300--focus border-error-300--disabled border-error-300--enabled border-error-400--active border-error-400--hover border-error-400--focus border-error-400--disabled border-error-400--enabled border-error-500--active border-error-500--hover border-error-500--focus border-error-500--disabled border-error-500--enabled border-error-600--active border-error-600--hover border-error-600--focus border-error-600--disabled border-error-600--enabled border-error-700--active border-error-700--hover border-error-700--focus border-error-700--disabled border-error-700--enabled border-error-800--active border-error-800--hover border-error-800--focus border-error-800--disabled border-error-800--enabled border-error-900--active border-error-900--hover border-error-900--focus border-error-900--disabled border-error-900--enabled border-gray-50--active border-gray-50--hover border-gray-50--focus border-gray-50--disabled border-gray-50--enabled border-gray-100--active border-gray-100--hover border-gray-100--focus border-gray-100--disabled border-gray-100--enabled border-gray-200--active border-gray-200--hover border-gray-200--focus border-gray-200--disabled border-gray-200--enabled border-gray-300--active border-gray-300--hover border-gray-300--focus border-gray-300--disabled border-gray-300--enabled border-gray-400--active border-gray-400--hover border-gray-400--focus border-gray-400--disabled border-gray-400--enabled border-gray-500--active border-gray-500--hover border-gray-500--focus border-gray-500--disabled border-gray-500--enabled border-gray-600--active border-gray-600--hover border-gray-600--focus border-gray-600--disabled border-gray-600--enabled border-gray-700--active border-gray-700--hover border-gray-700--focus border-gray-700--disabled border-gray-700--enabled border-gray-800--active border-gray-800--hover border-gray-800--focus border-gray-800--disabled border-gray-800--enabled border-gray-900--active border-gray-900--hover border-gray-900--focus border-gray-900--disabled border-gray-900--enabled`

### `$border-radius-atoms`
`round-0 round-1 round-2 round-3 round-4 round-5 round-6 round-full`

### `$border-radius-atoms--breakpoints`
`round-0--sm round-0--md round-0--lg round-0--xl round-1--sm round-1--md round-1--lg round-1--xl round-2--sm round-2--md round-2--lg round-2--xl round-3--sm round-3--md round-3--lg round-3--xl round-4--sm round-4--md round-4--lg round-4--xl round-5--sm round-5--md round-5--lg round-5--xl round-6--sm round-6--md round-6--lg round-6--xl round-full--sm round-full--md round-full--lg round-full--xl`

### `$border-radius-atoms--states`
`round-0--active round-0--hover round-0--focus round-0--disabled round-0--enabled round-1--active round-1--hover round-1--focus round-1--disabled round-1--enabled round-2--active round-2--hover round-2--focus round-2--disabled round-2--enabled round-3--active round-3--hover round-3--focus round-3--disabled round-3--enabled round-4--active round-4--hover round-4--focus round-4--disabled round-4--enabled round-5--active round-5--hover round-5--focus round-5--disabled round-5--enabled round-6--active round-6--hover round-6--focus round-6--disabled round-6--enabled round-full--active round-full--hover round-full--focus round-full--disabled round-full--enabled`

### `$border-style-atoms`
`border-solid border-dashed border-dotted border-double border-none`

### `$border-style-atoms--breakpoints`
`border-solid--sm border-solid--md border-solid--lg border-solid--xl border-dashed--sm border-dashed--md border-dashed--lg border-dashed--xl border-dotted--sm border-dotted--md border-dotted--lg border-dotted--xl border-double--sm border-double--md border-double--lg border-double--xl border-none--sm border-none--md border-none--lg border-none--xl`

### `$border-style-atoms--states`
`border-solid--active border-solid--hover border-solid--focus border-solid--disabled border-solid--enabled border-dashed--active border-dashed--hover border-dashed--focus border-dashed--disabled border-dashed--enabled border-dotted--active border-dotted--hover border-dotted--focus border-dotted--disabled border-dotted--enabled border-double--active border-double--hover border-double--focus border-double--disabled border-double--enabled border-none--active border-none--hover border-none--focus border-none--disabled border-none--enabled`

### `$border-width-atoms`
`border-0 border-1 border-2 border-3 border-4 border-5 border-6`

### `$border-width-atoms--breakpoints`
`border-0--sm border-0--md border-0--lg border-0--xl border-1--sm border-1--md border-1--lg border-1--xl border-2--sm border-2--md border-2--lg border-2--xl border-3--sm border-3--md border-3--lg border-3--xl border-4--sm border-4--md border-4--lg border-4--xl border-5--sm border-5--md border-5--lg border-5--xl border-6--sm border-6--md border-6--lg border-6--xl`

### `$border-width-atoms--states`
`border-0--active border-0--hover border-0--focus border-0--disabled border-0--enabled border-1--active border-1--hover border-1--focus border-1--disabled border-1--enabled border-2--active border-2--hover border-2--focus border-2--disabled border-2--enabled border-3--active border-3--hover border-3--focus border-3--disabled border-3--enabled border-4--active border-4--hover border-4--focus border-4--disabled border-4--enabled border-5--active border-5--hover border-5--focus border-5--disabled border-5--enabled border-6--active border-6--hover border-6--focus border-6--disabled border-6--enabled`

### `$box-sizing-atoms`
`box-border box-content`

### `$box-sizing-atoms--breakpoints`
`box-border--sm box-border--md box-border--lg box-border--xl box-content--sm box-content--md box-content--lg box-content--xl`

### `$box-sizing-atoms--states`
`box-border--active box-border--hover box-border--focus box-border--disabled box-border--enabled box-content--active box-content--hover box-content--focus box-content--disabled box-content--enabled`

### `$clear-atoms`
`clear-left clear-right clear-both clear-none`

### `$clear-atoms--breakpoints`
`clear-left--sm clear-left--md clear-left--lg clear-left--xl clear-right--sm clear-right--md clear-right--lg clear-right--xl clear-both--sm clear-both--md clear-both--lg clear-both--xl clear-none--sm clear-none--md clear-none--lg clear-none--xl`

### `$clear-atoms--states`
`clear-left--active clear-left--hover clear-left--focus clear-left--disabled clear-left--enabled clear-right--active clear-right--hover clear-right--focus clear-right--disabled clear-right--enabled clear-both--active clear-both--hover clear-both--focus clear-both--disabled clear-both--enabled clear-none--active clear-none--hover clear-none--focus clear-none--disabled clear-none--enabled`

### `$display-atoms`
`d-block d-inline-block d-inline d-flex d-inline-flex d-table d-table-caption d-table-cell d-table-column d-table-column-group d-table-footer-group d-table-header-group d-table-row-group d-table-row d-flow-root d-grid d-inline-grid d-none`

### `$display-atoms--breakpoints`
`d-block--sm d-block--md d-block--lg d-block--xl d-inline-block--sm d-inline-block--md d-inline-block--lg d-inline-block--xl d-inline--sm d-inline--md d-inline--lg d-inline--xl d-flex--sm d-flex--md d-flex--lg d-flex--xl d-inline-flex--sm d-inline-flex--md d-inline-flex--lg d-inline-flex--xl d-table--sm d-table--md d-table--lg d-table--xl d-table-caption--sm d-table-caption--md d-table-caption--lg d-table-caption--xl d-table-cell--sm d-table-cell--md d-table-cell--lg d-table-cell--xl d-table-column--sm d-table-column--md d-table-column--lg d-table-column--xl d-table-column-group--sm d-table-column-group--md d-table-column-group--lg d-table-column-group--xl d-table-footer-group--sm d-table-footer-group--md d-table-footer-group--lg d-table-footer-group--xl d-table-header-group--sm d-table-header-group--md d-table-header-group--lg d-table-header-group--xl d-table-row-group--sm d-table-row-group--md d-table-row-group--lg d-table-row-group--xl d-table-row--sm d-table-row--md d-table-row--lg d-table-row--xl d-flow-root--sm d-flow-root--md d-flow-root--lg d-flow-root--xl d-grid--sm d-grid--md d-grid--lg d-grid--xl d-inline-grid--sm d-inline-grid--md d-inline-grid--lg d-inline-grid--xl d-none--sm d-none--md d-none--lg d-none--xl`

### `$display-atoms--states`
`d-block--active d-block--hover d-block--focus d-block--disabled d-block--enabled d-inline-block--active d-inline-block--hover d-inline-block--focus d-inline-block--disabled d-inline-block--enabled d-inline--active d-inline--hover d-inline--focus d-inline--disabled d-inline--enabled d-flex--active d-flex--hover d-flex--focus d-flex--disabled d-flex--enabled d-inline-flex--active d-inline-flex--hover d-inline-flex--focus d-inline-flex--disabled d-inline-flex--enabled d-table--active d-table--hover d-table--focus d-table--disabled d-table--enabled d-table-caption--active d-table-caption--hover d-table-caption--focus d-table-caption--disabled d-table-caption--enabled d-table-cell--active d-table-cell--hover d-table-cell--focus d-table-cell--disabled d-table-cell--enabled d-table-column--active d-table-column--hover d-table-column--focus d-table-column--disabled d-table-column--enabled d-table-column-group--active d-table-column-group--hover d-table-column-group--focus d-table-column-group--disabled d-table-column-group--enabled d-table-footer-group--active d-table-footer-group--hover d-table-footer-group--focus d-table-footer-group--disabled d-table-footer-group--enabled d-table-header-group--active d-table-header-group--hover d-table-header-group--focus d-table-header-group--disabled d-table-header-group--enabled d-table-row-group--active d-table-row-group--hover d-table-row-group--focus d-table-row-group--disabled d-table-row-group--enabled d-table-row--active d-table-row--hover d-table-row--focus d-table-row--disabled d-table-row--enabled d-flow-root--active d-flow-root--hover d-flow-root--focus d-flow-root--disabled d-flow-root--enabled d-grid--active d-grid--hover d-grid--focus d-grid--disabled d-grid--enabled d-inline-grid--active d-inline-grid--hover d-inline-grid--focus d-inline-grid--disabled d-inline-grid--enabled d-none--active d-none--hover d-none--focus d-none--disabled d-none--enabled`

### `$flex-direction-atoms`
`flex-row flex-row-reverse flex-col flex-col-reverse`

### `$flex-direction-atoms--breakpoints`
`flex-row--sm flex-row--md flex-row--lg flex-row--xl flex-row-reverse--sm flex-row-reverse--md flex-row-reverse--lg flex-row-reverse--xl flex-col--sm flex-col--md flex-col--lg flex-col--xl flex-col-reverse--sm flex-col-reverse--md flex-col-reverse--lg flex-col-reverse--xl`

### `$flex-direction-atoms--states`
`flex-row--active flex-row--hover flex-row--focus flex-row--disabled flex-row--enabled flex-row-reverse--active flex-row-reverse--hover flex-row-reverse--focus flex-row-reverse--disabled flex-row-reverse--enabled flex-col--active flex-col--hover flex-col--focus flex-col--disabled flex-col--enabled flex-col-reverse--active flex-col-reverse--hover flex-col-reverse--focus flex-col-reverse--disabled flex-col-reverse--enabled`

### `$flex-grow-atoms`
`flex-grow-0 flex-grow`

### `$flex-grow-atoms--breakpoints`
`flex-grow-0--sm flex-grow-0--md flex-grow-0--lg flex-grow-0--xl flex-grow--sm flex-grow--md flex-grow--lg flex-grow--xl`

### `$flex-grow-atoms--states`
`flex-grow-0--active flex-grow-0--hover flex-grow-0--focus flex-grow-0--disabled flex-grow-0--enabled flex-grow--active flex-grow--hover flex-grow--focus flex-grow--disabled flex-grow--enabled`

### `$flex-shrink-atoms`
`flex-shrink-0 flex-shrink`

### `$flex-shrink-atoms--breakpoints`
`flex-shrink-0--sm flex-shrink-0--md flex-shrink-0--lg flex-shrink-0--xl flex-shrink--sm flex-shrink--md flex-shrink--lg flex-shrink--xl`

### `$flex-shrink-atoms--states`
`flex-shrink-0--active flex-shrink-0--hover flex-shrink-0--focus flex-shrink-0--disabled flex-shrink-0--enabled flex-shrink--active flex-shrink--hover flex-shrink--focus flex-shrink--disabled flex-shrink--enabled`

### `$flex-wrap-atoms`
`flex-wrap flex-wrap-reverse flex-nowrap`

### `$flex-wrap-atoms--breakpoints`
`flex-wrap--sm flex-wrap--md flex-wrap--lg flex-wrap--xl flex-wrap-reverse--sm flex-wrap-reverse--md flex-wrap-reverse--lg flex-wrap-reverse--xl flex-nowrap--sm flex-nowrap--md flex-nowrap--lg flex-nowrap--xl`

### `$flex-wrap-atoms--states`
`flex-wrap--active flex-wrap--hover flex-wrap--focus flex-wrap--disabled flex-wrap--enabled flex-wrap-reverse--active flex-wrap-reverse--hover flex-wrap-reverse--focus flex-wrap-reverse--disabled flex-wrap-reverse--enabled flex-nowrap--active flex-nowrap--hover flex-nowrap--focus flex-nowrap--disabled flex-nowrap--enabled`

### `$float-atoms`
`float-left float-right float-none`

### `$float-atoms--breakpoints`
`float-left--sm float-left--md float-left--lg float-left--xl float-right--sm float-right--md float-right--lg float-right--xl float-none--sm float-none--md float-none--lg float-none--xl`

### `$float-atoms--states`
`float-left--active float-left--hover float-left--focus float-left--disabled float-left--enabled float-right--active float-right--hover float-right--focus float-right--disabled float-right--enabled float-none--active float-none--hover float-none--focus float-none--disabled float-none--enabled`

### `$font-family-atoms`
`font-sans font-serif font-mono`

### `$font-family-atoms--breakpoints`
`font-sans--sm font-sans--md font-sans--lg font-sans--xl font-serif--sm font-serif--md font-serif--lg font-serif--xl font-mono--sm font-mono--md font-mono--lg font-mono--xl`

### `$font-family-atoms--states`
`font-sans--active font-sans--hover font-sans--focus font-sans--disabled font-sans--enabled font-serif--active font-serif--hover font-serif--focus font-serif--disabled font-serif--enabled font-mono--active font-mono--hover font-mono--focus font-mono--disabled font-mono--enabled`

### `$font-weight-atoms`
`font-100 font-200 font-300 font-400 font-500 font-600 font-700 font-800 font-900 font-thin font-light font-semilight font-normal font-medium font-semibold font-bold font-extrabold font-thick`

### `$font-weight-atoms--breakpoints`
`font-100--sm font-100--md font-100--lg font-100--xl font-200--sm font-200--md font-200--lg font-200--xl font-300--sm font-300--md font-300--lg font-300--xl font-400--sm font-400--md font-400--lg font-400--xl font-500--sm font-500--md font-500--lg font-500--xl font-600--sm font-600--md font-600--lg font-600--xl font-700--sm font-700--md font-700--lg font-700--xl font-800--sm font-800--md font-800--lg font-800--xl font-900--sm font-900--md font-900--lg font-900--xl font-thin--sm font-thin--md font-thin--lg font-thin--xl font-light--sm font-light--md font-light--lg font-light--xl font-semilight--sm font-semilight--md font-semilight--lg font-semilight--xl font-normal--sm font-normal--md font-normal--lg font-normal--xl font-medium--sm font-medium--md font-medium--lg font-medium--xl font-semibold--sm font-semibold--md font-semibold--lg font-semibold--xl font-bold--sm font-bold--md font-bold--lg font-bold--xl font-extrabold--sm font-extrabold--md font-extrabold--lg font-extrabold--xl font-thick--sm font-thick--md font-thick--lg font-thick--xl`

### `$font-weight-atoms--states`
`font-100--active font-100--hover font-100--focus font-100--disabled font-100--enabled font-200--active font-200--hover font-200--focus font-200--disabled font-200--enabled font-300--active font-300--hover font-300--focus font-300--disabled font-300--enabled font-400--active font-400--hover font-400--focus font-400--disabled font-400--enabled font-500--active font-500--hover font-500--focus font-500--disabled font-500--enabled font-600--active font-600--hover font-600--focus font-600--disabled font-600--enabled font-700--active font-700--hover font-700--focus font-700--disabled font-700--enabled font-800--active font-800--hover font-800--focus font-800--disabled font-800--enabled font-900--active font-900--hover font-900--focus font-900--disabled font-900--enabled font-thin--active font-thin--hover font-thin--focus font-thin--disabled font-thin--enabled font-light--active font-light--hover font-light--focus font-light--disabled font-light--enabled font-semilight--active font-semilight--hover font-semilight--focus font-semilight--disabled font-semilight--enabled font-normal--active font-normal--hover font-normal--focus font-normal--disabled font-normal--enabled font-medium--active font-medium--hover font-medium--focus font-medium--disabled font-medium--enabled font-semibold--active font-semibold--hover font-semibold--focus font-semibold--disabled font-semibold--enabled font-bold--active font-bold--hover font-bold--focus font-bold--disabled font-bold--enabled font-extrabold--active font-extrabold--hover font-extrabold--focus font-extrabold--disabled font-extrabold--enabled font-thick--active font-thick--hover font-thick--focus font-thick--disabled font-thick--enabled`

### `$height-atoms`
`h-0 h-vh0 h-vh1 h-vh2 h-vh3 h-vh4 h-vh5 h-vh6 h-vh7 h-vh8 h-vh9 h-vh10 h-vh11 h-vh12 h-p0 h-p1 h-p2 h-p3 h-p4 h-p5 h-p6 h-p7 h-p8 h-p9 h-p10 h-p11 h-p12`

### `$height-atoms--breakpoints`
`h-0--sm h-0--md h-0--lg h-0--xl h-vh0--sm h-vh0--md h-vh0--lg h-vh0--xl h-vh1--sm h-vh1--md h-vh1--lg h-vh1--xl h-vh2--sm h-vh2--md h-vh2--lg h-vh2--xl h-vh3--sm h-vh3--md h-vh3--lg h-vh3--xl h-vh4--sm h-vh4--md h-vh4--lg h-vh4--xl h-vh5--sm h-vh5--md h-vh5--lg h-vh5--xl h-vh6--sm h-vh6--md h-vh6--lg h-vh6--xl h-vh7--sm h-vh7--md h-vh7--lg h-vh7--xl h-vh8--sm h-vh8--md h-vh8--lg h-vh8--xl h-vh9--sm h-vh9--md h-vh9--lg h-vh9--xl h-vh10--sm h-vh10--md h-vh10--lg h-vh10--xl h-vh11--sm h-vh11--md h-vh11--lg h-vh11--xl h-vh12--sm h-vh12--md h-vh12--lg h-vh12--xl h-p0--sm h-p0--md h-p0--lg h-p0--xl h-p1--sm h-p1--md h-p1--lg h-p1--xl h-p2--sm h-p2--md h-p2--lg h-p2--xl h-p3--sm h-p3--md h-p3--lg h-p3--xl h-p4--sm h-p4--md h-p4--lg h-p4--xl h-p5--sm h-p5--md h-p5--lg h-p5--xl h-p6--sm h-p6--md h-p6--lg h-p6--xl h-p7--sm h-p7--md h-p7--lg h-p7--xl h-p8--sm h-p8--md h-p8--lg h-p8--xl h-p9--sm h-p9--md h-p9--lg h-p9--xl h-p10--sm h-p10--md h-p10--lg h-p10--xl h-p11--sm h-p11--md h-p11--lg h-p11--xl h-p12--sm h-p12--md h-p12--lg h-p12--xl`

### `$height-atoms--states`
`h-0--active h-0--hover h-0--focus h-0--disabled h-0--enabled h-vh0--active h-vh0--hover h-vh0--focus h-vh0--disabled h-vh0--enabled h-vh1--active h-vh1--hover h-vh1--focus h-vh1--disabled h-vh1--enabled h-vh2--active h-vh2--hover h-vh2--focus h-vh2--disabled h-vh2--enabled h-vh3--active h-vh3--hover h-vh3--focus h-vh3--disabled h-vh3--enabled h-vh4--active h-vh4--hover h-vh4--focus h-vh4--disabled h-vh4--enabled h-vh5--active h-vh5--hover h-vh5--focus h-vh5--disabled h-vh5--enabled h-vh6--active h-vh6--hover h-vh6--focus h-vh6--disabled h-vh6--enabled h-vh7--active h-vh7--hover h-vh7--focus h-vh7--disabled h-vh7--enabled h-vh8--active h-vh8--hover h-vh8--focus h-vh8--disabled h-vh8--enabled h-vh9--active h-vh9--hover h-vh9--focus h-vh9--disabled h-vh9--enabled h-vh10--active h-vh10--hover h-vh10--focus h-vh10--disabled h-vh10--enabled h-vh11--active h-vh11--hover h-vh11--focus h-vh11--disabled h-vh11--enabled h-vh12--active h-vh12--hover h-vh12--focus h-vh12--disabled h-vh12--enabled h-p0--active h-p0--hover h-p0--focus h-p0--disabled h-p0--enabled h-p1--active h-p1--hover h-p1--focus h-p1--disabled h-p1--enabled h-p2--active h-p2--hover h-p2--focus h-p2--disabled h-p2--enabled h-p3--active h-p3--hover h-p3--focus h-p3--disabled h-p3--enabled h-p4--active h-p4--hover h-p4--focus h-p4--disabled h-p4--enabled h-p5--active h-p5--hover h-p5--focus h-p5--disabled h-p5--enabled h-p6--active h-p6--hover h-p6--focus h-p6--disabled h-p6--enabled h-p7--active h-p7--hover h-p7--focus h-p7--disabled h-p7--enabled h-p8--active h-p8--hover h-p8--focus h-p8--disabled h-p8--enabled h-p9--active h-p9--hover h-p9--focus h-p9--disabled h-p9--enabled h-p10--active h-p10--hover h-p10--focus h-p10--disabled h-p10--enabled h-p11--active h-p11--hover h-p11--focus h-p11--disabled h-p11--enabled h-p12--active h-p12--hover h-p12--focus h-p12--disabled h-p12--enabled`

### `$min-height-atoms`
`hmin-0 hmin-vh0 hmin-vh1 hmin-vh2 hmin-vh3 hmin-vh4 hmin-vh5 hmin-vh6 hmin-vh7 hmin-vh8 hmin-vh9 hmin-vh10 hmin-vh11 hmin-vh12 hmin-p0 hmin-p1 hmin-p2 hmin-p3 hmin-p4 hmin-p5 hmin-p6 hmin-p7 hmin-p8 hmin-p9 hmin-p10 hmin-p11 hmin-p12`

### `$min-height-atoms--breakpoints`
`hmin-0--sm hmin-0--md hmin-0--lg hmin-0--xl hmin-vh0--sm hmin-vh0--md hmin-vh0--lg hmin-vh0--xl hmin-vh1--sm hmin-vh1--md hmin-vh1--lg hmin-vh1--xl hmin-vh2--sm hmin-vh2--md hmin-vh2--lg hmin-vh2--xl hmin-vh3--sm hmin-vh3--md hmin-vh3--lg hmin-vh3--xl hmin-vh4--sm hmin-vh4--md hmin-vh4--lg hmin-vh4--xl hmin-vh5--sm hmin-vh5--md hmin-vh5--lg hmin-vh5--xl hmin-vh6--sm hmin-vh6--md hmin-vh6--lg hmin-vh6--xl hmin-vh7--sm hmin-vh7--md hmin-vh7--lg hmin-vh7--xl hmin-vh8--sm hmin-vh8--md hmin-vh8--lg hmin-vh8--xl hmin-vh9--sm hmin-vh9--md hmin-vh9--lg hmin-vh9--xl hmin-vh10--sm hmin-vh10--md hmin-vh10--lg hmin-vh10--xl hmin-vh11--sm hmin-vh11--md hmin-vh11--lg hmin-vh11--xl hmin-vh12--sm hmin-vh12--md hmin-vh12--lg hmin-vh12--xl hmin-p0--sm hmin-p0--md hmin-p0--lg hmin-p0--xl hmin-p1--sm hmin-p1--md hmin-p1--lg hmin-p1--xl hmin-p2--sm hmin-p2--md hmin-p2--lg hmin-p2--xl hmin-p3--sm hmin-p3--md hmin-p3--lg hmin-p3--xl hmin-p4--sm hmin-p4--md hmin-p4--lg hmin-p4--xl hmin-p5--sm hmin-p5--md hmin-p5--lg hmin-p5--xl hmin-p6--sm hmin-p6--md hmin-p6--lg hmin-p6--xl hmin-p7--sm hmin-p7--md hmin-p7--lg hmin-p7--xl hmin-p8--sm hmin-p8--md hmin-p8--lg hmin-p8--xl hmin-p9--sm hmin-p9--md hmin-p9--lg hmin-p9--xl hmin-p10--sm hmin-p10--md hmin-p10--lg hmin-p10--xl hmin-p11--sm hmin-p11--md hmin-p11--lg hmin-p11--xl hmin-p12--sm hmin-p12--md hmin-p12--lg hmin-p12--xl`

### `$min-height-atoms--states`
`hmin-0--active hmin-0--hover hmin-0--focus hmin-0--disabled hmin-0--enabled hmin-vh0--active hmin-vh0--hover hmin-vh0--focus hmin-vh0--disabled hmin-vh0--enabled hmin-vh1--active hmin-vh1--hover hmin-vh1--focus hmin-vh1--disabled hmin-vh1--enabled hmin-vh2--active hmin-vh2--hover hmin-vh2--focus hmin-vh2--disabled hmin-vh2--enabled hmin-vh3--active hmin-vh3--hover hmin-vh3--focus hmin-vh3--disabled hmin-vh3--enabled hmin-vh4--active hmin-vh4--hover hmin-vh4--focus hmin-vh4--disabled hmin-vh4--enabled hmin-vh5--active hmin-vh5--hover hmin-vh5--focus hmin-vh5--disabled hmin-vh5--enabled hmin-vh6--active hmin-vh6--hover hmin-vh6--focus hmin-vh6--disabled hmin-vh6--enabled hmin-vh7--active hmin-vh7--hover hmin-vh7--focus hmin-vh7--disabled hmin-vh7--enabled hmin-vh8--active hmin-vh8--hover hmin-vh8--focus hmin-vh8--disabled hmin-vh8--enabled hmin-vh9--active hmin-vh9--hover hmin-vh9--focus hmin-vh9--disabled hmin-vh9--enabled hmin-vh10--active hmin-vh10--hover hmin-vh10--focus hmin-vh10--disabled hmin-vh10--enabled hmin-vh11--active hmin-vh11--hover hmin-vh11--focus hmin-vh11--disabled hmin-vh11--enabled hmin-vh12--active hmin-vh12--hover hmin-vh12--focus hmin-vh12--disabled hmin-vh12--enabled hmin-p0--active hmin-p0--hover hmin-p0--focus hmin-p0--disabled hmin-p0--enabled hmin-p1--active hmin-p1--hover hmin-p1--focus hmin-p1--disabled hmin-p1--enabled hmin-p2--active hmin-p2--hover hmin-p2--focus hmin-p2--disabled hmin-p2--enabled hmin-p3--active hmin-p3--hover hmin-p3--focus hmin-p3--disabled hmin-p3--enabled hmin-p4--active hmin-p4--hover hmin-p4--focus hmin-p4--disabled hmin-p4--enabled hmin-p5--active hmin-p5--hover hmin-p5--focus hmin-p5--disabled hmin-p5--enabled hmin-p6--active hmin-p6--hover hmin-p6--focus hmin-p6--disabled hmin-p6--enabled hmin-p7--active hmin-p7--hover hmin-p7--focus hmin-p7--disabled hmin-p7--enabled hmin-p8--active hmin-p8--hover hmin-p8--focus hmin-p8--disabled hmin-p8--enabled hmin-p9--active hmin-p9--hover hmin-p9--focus hmin-p9--disabled hmin-p9--enabled hmin-p10--active hmin-p10--hover hmin-p10--focus hmin-p10--disabled hmin-p10--enabled hmin-p11--active hmin-p11--hover hmin-p11--focus hmin-p11--disabled hmin-p11--enabled hmin-p12--active hmin-p12--hover hmin-p12--focus hmin-p12--disabled hmin-p12--enabled`

### `$max-height-atoms`
`hmax-0 hmax-vh0 hmax-vh1 hmax-vh2 hmax-vh3 hmax-vh4 hmax-vh5 hmax-vh6 hmax-vh7 hmax-vh8 hmax-vh9 hmax-vh10 hmax-vh11 hmax-vh12 hmax-p0 hmax-p1 hmax-p2 hmax-p3 hmax-p4 hmax-p5 hmax-p6 hmax-p7 hmax-p8 hmax-p9 hmax-p10 hmax-p11 hmax-p12`

### `$max-height-atoms--breakpoints`
`hmax-0--sm hmax-0--md hmax-0--lg hmax-0--xl hmax-vh0--sm hmax-vh0--md hmax-vh0--lg hmax-vh0--xl hmax-vh1--sm hmax-vh1--md hmax-vh1--lg hmax-vh1--xl hmax-vh2--sm hmax-vh2--md hmax-vh2--lg hmax-vh2--xl hmax-vh3--sm hmax-vh3--md hmax-vh3--lg hmax-vh3--xl hmax-vh4--sm hmax-vh4--md hmax-vh4--lg hmax-vh4--xl hmax-vh5--sm hmax-vh5--md hmax-vh5--lg hmax-vh5--xl hmax-vh6--sm hmax-vh6--md hmax-vh6--lg hmax-vh6--xl hmax-vh7--sm hmax-vh7--md hmax-vh7--lg hmax-vh7--xl hmax-vh8--sm hmax-vh8--md hmax-vh8--lg hmax-vh8--xl hmax-vh9--sm hmax-vh9--md hmax-vh9--lg hmax-vh9--xl hmax-vh10--sm hmax-vh10--md hmax-vh10--lg hmax-vh10--xl hmax-vh11--sm hmax-vh11--md hmax-vh11--lg hmax-vh11--xl hmax-vh12--sm hmax-vh12--md hmax-vh12--lg hmax-vh12--xl hmax-p0--sm hmax-p0--md hmax-p0--lg hmax-p0--xl hmax-p1--sm hmax-p1--md hmax-p1--lg hmax-p1--xl hmax-p2--sm hmax-p2--md hmax-p2--lg hmax-p2--xl hmax-p3--sm hmax-p3--md hmax-p3--lg hmax-p3--xl hmax-p4--sm hmax-p4--md hmax-p4--lg hmax-p4--xl hmax-p5--sm hmax-p5--md hmax-p5--lg hmax-p5--xl hmax-p6--sm hmax-p6--md hmax-p6--lg hmax-p6--xl hmax-p7--sm hmax-p7--md hmax-p7--lg hmax-p7--xl hmax-p8--sm hmax-p8--md hmax-p8--lg hmax-p8--xl hmax-p9--sm hmax-p9--md hmax-p9--lg hmax-p9--xl hmax-p10--sm hmax-p10--md hmax-p10--lg hmax-p10--xl hmax-p11--sm hmax-p11--md hmax-p11--lg hmax-p11--xl hmax-p12--sm hmax-p12--md hmax-p12--lg hmax-p12--xl`

### `$max-height-atoms--states`
`hmax-0--active hmax-0--hover hmax-0--focus hmax-0--disabled hmax-0--enabled hmax-vh0--active hmax-vh0--hover hmax-vh0--focus hmax-vh0--disabled hmax-vh0--enabled hmax-vh1--active hmax-vh1--hover hmax-vh1--focus hmax-vh1--disabled hmax-vh1--enabled hmax-vh2--active hmax-vh2--hover hmax-vh2--focus hmax-vh2--disabled hmax-vh2--enabled hmax-vh3--active hmax-vh3--hover hmax-vh3--focus hmax-vh3--disabled hmax-vh3--enabled hmax-vh4--active hmax-vh4--hover hmax-vh4--focus hmax-vh4--disabled hmax-vh4--enabled hmax-vh5--active hmax-vh5--hover hmax-vh5--focus hmax-vh5--disabled hmax-vh5--enabled hmax-vh6--active hmax-vh6--hover hmax-vh6--focus hmax-vh6--disabled hmax-vh6--enabled hmax-vh7--active hmax-vh7--hover hmax-vh7--focus hmax-vh7--disabled hmax-vh7--enabled hmax-vh8--active hmax-vh8--hover hmax-vh8--focus hmax-vh8--disabled hmax-vh8--enabled hmax-vh9--active hmax-vh9--hover hmax-vh9--focus hmax-vh9--disabled hmax-vh9--enabled hmax-vh10--active hmax-vh10--hover hmax-vh10--focus hmax-vh10--disabled hmax-vh10--enabled hmax-vh11--active hmax-vh11--hover hmax-vh11--focus hmax-vh11--disabled hmax-vh11--enabled hmax-vh12--active hmax-vh12--hover hmax-vh12--focus hmax-vh12--disabled hmax-vh12--enabled hmax-p0--active hmax-p0--hover hmax-p0--focus hmax-p0--disabled hmax-p0--enabled hmax-p1--active hmax-p1--hover hmax-p1--focus hmax-p1--disabled hmax-p1--enabled hmax-p2--active hmax-p2--hover hmax-p2--focus hmax-p2--disabled hmax-p2--enabled hmax-p3--active hmax-p3--hover hmax-p3--focus hmax-p3--disabled hmax-p3--enabled hmax-p4--active hmax-p4--hover hmax-p4--focus hmax-p4--disabled hmax-p4--enabled hmax-p5--active hmax-p5--hover hmax-p5--focus hmax-p5--disabled hmax-p5--enabled hmax-p6--active hmax-p6--hover hmax-p6--focus hmax-p6--disabled hmax-p6--enabled hmax-p7--active hmax-p7--hover hmax-p7--focus hmax-p7--disabled hmax-p7--enabled hmax-p8--active hmax-p8--hover hmax-p8--focus hmax-p8--disabled hmax-p8--enabled hmax-p9--active hmax-p9--hover hmax-p9--focus hmax-p9--disabled hmax-p9--enabled hmax-p10--active hmax-p10--hover hmax-p10--focus hmax-p10--disabled hmax-p10--enabled hmax-p11--active hmax-p11--hover hmax-p11--focus hmax-p11--disabled hmax-p11--enabled hmax-p12--active hmax-p12--hover hmax-p12--focus hmax-p12--disabled hmax-p12--enabled`

### `$justify-content-atoms`
`justify-start justify-end justify-center justify-between justify-around justify-evenly`

### `$justify-content-atoms--breakpoints`
`justify-start--sm justify-start--md justify-start--lg justify-start--xl justify-end--sm justify-end--md justify-end--lg justify-end--xl justify-center--sm justify-center--md justify-center--lg justify-center--xl justify-between--sm justify-between--md justify-between--lg justify-between--xl justify-around--sm justify-around--md justify-around--lg justify-around--xl justify-evenly--sm justify-evenly--md justify-evenly--lg justify-evenly--xl`

### `$justify-content-atoms--states`
`justify-start--active justify-start--hover justify-start--focus justify-start--disabled justify-start--enabled justify-end--active justify-end--hover justify-end--focus justify-end--disabled justify-end--enabled justify-center--active justify-center--hover justify-center--focus justify-center--disabled justify-center--enabled justify-between--active justify-between--hover justify-between--focus justify-between--disabled justify-between--enabled justify-around--active justify-around--hover justify-around--focus justify-around--disabled justify-around--enabled justify-evenly--active justify-evenly--hover justify-evenly--focus justify-evenly--disabled justify-evenly--enabled`

### `$justify-items-atoms`
`justify-items-auto justify-items-start justify-items-end justify-items-center justify-items-stretch`

### `$justify-items-atoms--breakpoints`
`justify-items-auto--sm justify-items-auto--md justify-items-auto--lg justify-items-auto--xl justify-items-start--sm justify-items-start--md justify-items-start--lg justify-items-start--xl justify-items-end--sm justify-items-end--md justify-items-end--lg justify-items-end--xl justify-items-center--sm justify-items-center--md justify-items-center--lg justify-items-center--xl justify-items-stretch--sm justify-items-stretch--md justify-items-stretch--lg justify-items-stretch--xl`

### `$justify-items-atoms`
`justify-items-auto justify-items-start justify-items-end justify-items-center justify-items-stretch`

### `$justify-self-atoms`
`justify-self-auto justify-self-start justify-self-end justify-self-center justify-self-stretch`

### `$justify-self-atoms--breakpoints`
`justify-self-auto--sm justify-self-auto--md justify-self-auto--lg justify-self-auto--xl justify-self-start--sm justify-self-start--md justify-self-start--lg justify-self-start--xl justify-self-end--sm justify-self-end--md justify-self-end--lg justify-self-end--xl justify-self-center--sm justify-self-center--md justify-self-center--lg justify-self-center--xl justify-self-stretch--sm justify-self-stretch--md justify-self-stretch--lg justify-self-stretch--xl`

### `$justify-self-atoms--states`
`justify-self-auto--active justify-self-auto--hover justify-self-auto--focus justify-self-auto--disabled justify-self-auto--enabled justify-self-start--active justify-self-start--hover justify-self-start--focus justify-self-start--disabled justify-self-start--enabled justify-self-end--active justify-self-end--hover justify-self-end--focus justify-self-end--disabled justify-self-end--enabled justify-self-center--active justify-self-center--hover justify-self-center--focus justify-self-center--disabled justify-self-center--enabled justify-self-stretch--active justify-self-stretch--hover justify-self-stretch--focus justify-self-stretch--disabled justify-self-stretch--enabled`

### `$margin-atoms`
`m-0 m-1 m-2 m-3 m-4 m-5 m-6`

### `$margin-atoms--breakpoints`
`m-0--sm m-0--md m-0--lg m-0--xl m-1--sm m-1--md m-1--lg m-1--xl m-2--sm m-2--md m-2--lg m-2--xl m-3--sm m-3--md m-3--lg m-3--xl m-4--sm m-4--md m-4--lg m-4--xl m-5--sm m-5--md m-5--lg m-5--xl m-6--sm m-6--md m-6--lg m-6--xl`

### `$margin-atoms--states`
`m-0--active m-0--hover m-0--focus m-0--disabled m-0--enabled m-1--active m-1--hover m-1--focus m-1--disabled m-1--enabled m-2--active m-2--hover m-2--focus m-2--disabled m-2--enabled m-3--active m-3--hover m-3--focus m-3--disabled m-3--enabled m-4--active m-4--hover m-4--focus m-4--disabled m-4--enabled m-5--active m-5--hover m-5--focus m-5--disabled m-5--enabled m-6--active m-6--hover m-6--focus m-6--disabled m-6--enabled`

### `$margin-x-atoms`
`mx-0 mx-1 mx-2 mx-3 mx-4 mx-5 mx-6`

### `$margin-x-atoms--breakpoints`
`mx-0--sm mx-0--md mx-0--lg mx-0--xl mx-1--sm mx-1--md mx-1--lg mx-1--xl mx-2--sm mx-2--md mx-2--lg mx-2--xl mx-3--sm mx-3--md mx-3--lg mx-3--xl mx-4--sm mx-4--md mx-4--lg mx-4--xl mx-5--sm mx-5--md mx-5--lg mx-5--xl mx-6--sm mx-6--md mx-6--lg mx-6--xl`

### `$margin-x-atoms--states`
`mx-0--active mx-0--hover mx-0--focus mx-0--disabled mx-0--enabled mx-1--active mx-1--hover mx-1--focus mx-1--disabled mx-1--enabled mx-2--active mx-2--hover mx-2--focus mx-2--disabled mx-2--enabled mx-3--active mx-3--hover mx-3--focus mx-3--disabled mx-3--enabled mx-4--active mx-4--hover mx-4--focus mx-4--disabled mx-4--enabled mx-5--active mx-5--hover mx-5--focus mx-5--disabled mx-5--enabled mx-6--active mx-6--hover mx-6--focus mx-6--disabled mx-6--enabled`

### `$margin-y-atoms`
`my-0 my-1 my-2 my-3 my-4 my-5 my-6`

### `$margin-y-atoms--breakpoints`
`my-0--sm my-0--md my-0--lg my-0--xl my-1--sm my-1--md my-1--lg my-1--xl my-2--sm my-2--md my-2--lg my-2--xl my-3--sm my-3--md my-3--lg my-3--xl my-4--sm my-4--md my-4--lg my-4--xl my-5--sm my-5--md my-5--lg my-5--xl my-6--sm my-6--md my-6--lg my-6--xl`

### `$margin-y-atoms--states`
`my-0--active my-0--hover my-0--focus my-0--disabled my-0--enabled my-1--active my-1--hover my-1--focus my-1--disabled my-1--enabled my-2--active my-2--hover my-2--focus my-2--disabled my-2--enabled my-3--active my-3--hover my-3--focus my-3--disabled my-3--enabled my-4--active my-4--hover my-4--focus my-4--disabled my-4--enabled my-5--active my-5--hover my-5--focus my-5--disabled my-5--enabled my-6--active my-6--hover my-6--focus my-6--disabled my-6--enabled`

### `$margin-t-atoms`
`mt-0 mt-1 mt-2 mt-3 mt-4 mt-5 mt-6`

### `$margin-t-atoms--breakpoints`
`mt-0--sm mt-0--md mt-0--lg mt-0--xl mt-1--sm mt-1--md mt-1--lg mt-1--xl mt-2--sm mt-2--md mt-2--lg mt-2--xl mt-3--sm mt-3--md mt-3--lg mt-3--xl mt-4--sm mt-4--md mt-4--lg mt-4--xl mt-5--sm mt-5--md mt-5--lg mt-5--xl mt-6--sm mt-6--md mt-6--lg mt-6--xl`

### `$margin-t-atoms--states`
`mt-0--active mt-0--hover mt-0--focus mt-0--disabled mt-0--enabled mt-1--active mt-1--hover mt-1--focus mt-1--disabled mt-1--enabled mt-2--active mt-2--hover mt-2--focus mt-2--disabled mt-2--enabled mt-3--active mt-3--hover mt-3--focus mt-3--disabled mt-3--enabled mt-4--active mt-4--hover mt-4--focus mt-4--disabled mt-4--enabled mt-5--active mt-5--hover mt-5--focus mt-5--disabled mt-5--enabled mt-6--active mt-6--hover mt-6--focus mt-6--disabled mt-6--enabled`

### `$margin-r-atoms`
`mr-0 mr-1 mr-2 mr-3 mr-4 mr-5 mr-6`

### `$margin-r-atoms--breakpoints`
`mr-0--sm mr-0--md mr-0--lg mr-0--xl mr-1--sm mr-1--md mr-1--lg mr-1--xl mr-2--sm mr-2--md mr-2--lg mr-2--xl mr-3--sm mr-3--md mr-3--lg mr-3--xl mr-4--sm mr-4--md mr-4--lg mr-4--xl mr-5--sm mr-5--md mr-5--lg mr-5--xl mr-6--sm mr-6--md mr-6--lg mr-6--xl`

### `$margin-r-atoms--states`
`mr-0--active mr-0--hover mr-0--focus mr-0--disabled mr-0--enabled mr-1--active mr-1--hover mr-1--focus mr-1--disabled mr-1--enabled mr-2--active mr-2--hover mr-2--focus mr-2--disabled mr-2--enabled mr-3--active mr-3--hover mr-3--focus mr-3--disabled mr-3--enabled mr-4--active mr-4--hover mr-4--focus mr-4--disabled mr-4--enabled mr-5--active mr-5--hover mr-5--focus mr-5--disabled mr-5--enabled mr-6--active mr-6--hover mr-6--focus mr-6--disabled mr-6--enabled`

### `$margin-b-atoms`
`mb-0 mb-1 mb-2 mb-3 mb-4 mb-5 mb-6`

### `$margin-b-atoms--breakpoints`
`mb-0--sm mb-0--md mb-0--lg mb-0--xl mb-1--sm mb-1--md mb-1--lg mb-1--xl mb-2--sm mb-2--md mb-2--lg mb-2--xl mb-3--sm mb-3--md mb-3--lg mb-3--xl mb-4--sm mb-4--md mb-4--lg mb-4--xl mb-5--sm mb-5--md mb-5--lg mb-5--xl mb-6--sm mb-6--md mb-6--lg mb-6--xl`

### `$margin-b-atoms--states`
`mb-0--active mb-0--hover mb-0--focus mb-0--disabled mb-0--enabled mb-1--active mb-1--hover mb-1--focus mb-1--disabled mb-1--enabled mb-2--active mb-2--hover mb-2--focus mb-2--disabled mb-2--enabled mb-3--active mb-3--hover mb-3--focus mb-3--disabled mb-3--enabled mb-4--active mb-4--hover mb-4--focus mb-4--disabled mb-4--enabled mb-5--active mb-5--hover mb-5--focus mb-5--disabled mb-5--enabled mb-6--active mb-6--hover mb-6--focus mb-6--disabled mb-6--enabled`

### `$margin-l-atoms`
`ml-0 ml-1 ml-2 ml-3 ml-4 ml-5 ml-6`

### `$margin-l-atoms--breakpoints`
`ml-0--sm ml-0--md ml-0--lg ml-0--xl ml-1--sm ml-1--md ml-1--lg ml-1--xl ml-2--sm ml-2--md ml-2--lg ml-2--xl ml-3--sm ml-3--md ml-3--lg ml-3--xl ml-4--sm ml-4--md ml-4--lg ml-4--xl ml-5--sm ml-5--md ml-5--lg ml-5--xl ml-6--sm ml-6--md ml-6--lg ml-6--xl`

### `$margin-l-atoms--states`
`ml-0--active ml-0--hover ml-0--focus ml-0--disabled ml-0--enabled ml-1--active ml-1--hover ml-1--focus ml-1--disabled ml-1--enabled ml-2--active ml-2--hover ml-2--focus ml-2--disabled ml-2--enabled ml-3--active ml-3--hover ml-3--focus ml-3--disabled ml-3--enabled ml-4--active ml-4--hover ml-4--focus ml-4--disabled ml-4--enabled ml-5--active ml-5--hover ml-5--focus ml-5--disabled ml-5--enabled ml-6--active ml-6--hover ml-6--focus ml-6--disabled ml-6--enabled`

### `$object-fit-atoms`
`obj-contain obj-cover obj-fill obj-none obj-scale-down`

### `$object-fit-atoms--breakpoints`
`obj-contain--sm obj-contain--md obj-contain--lg obj-contain--xl obj-cover--sm obj-cover--md obj-cover--lg obj-cover--xl obj-fill--sm obj-fill--md obj-fill--lg obj-fill--xl obj-none--sm obj-none--md obj-none--lg obj-none--xl obj-scale-down--sm obj-scale-down--md obj-scale-down--lg obj-scale-down--xl`

### `$object-fit-atoms--states`
`obj-contain--active obj-contain--hover obj-contain--focus obj-contain--disabled obj-contain--enabled obj-cover--active obj-cover--hover obj-cover--focus obj-cover--disabled obj-cover--enabled obj-fill--active obj-fill--hover obj-fill--focus obj-fill--disabled obj-fill--enabled obj-none--active obj-none--hover obj-none--focus obj-none--disabled obj-none--enabled obj-scale-down--active obj-scale-down--hover obj-scale-down--focus obj-scale-down--disabled obj-scale-down--enabled`

### `$object-position-atoms`
`obj-top-left obj-top obj-top-right obj-left obj-center obj-right obj-bottom-left obj-bottom obj-bottom-right`

### `$object-position-atoms--breakpoints`
`obj-top-left--sm obj-top-left--md obj-top-left--lg obj-top-left--xl obj-top--sm obj-top--md obj-top--lg obj-top--xl obj-top-right--sm obj-top-right--md obj-top-right--lg obj-top-right--xl obj-left--sm obj-left--md obj-left--lg obj-left--xl obj-center--sm obj-center--md obj-center--lg obj-center--xl obj-right--sm obj-right--md obj-right--lg obj-right--xl obj-bottom-left--sm obj-bottom-left--md obj-bottom-left--lg obj-bottom-left--xl obj-bottom--sm obj-bottom--md obj-bottom--lg obj-bottom--xl obj-bottom-right--sm obj-bottom-right--md obj-bottom-right--lg obj-bottom-right--xl`

### `$object-position-atoms--states`
`obj-top-left--active obj-top-left--hover obj-top-left--focus obj-top-left--disabled obj-top-left--enabled obj-top--active obj-top--hover obj-top--focus obj-top--disabled obj-top--enabled obj-top-right--active obj-top-right--hover obj-top-right--focus obj-top-right--disabled obj-top-right--enabled obj-left--active obj-left--hover obj-left--focus obj-left--disabled obj-left--enabled obj-center--active obj-center--hover obj-center--focus obj-center--disabled obj-center--enabled obj-right--active obj-right--hover obj-right--focus obj-right--disabled obj-right--enabled obj-bottom-left--active obj-bottom-left--hover obj-bottom-left--focus obj-bottom-left--disabled obj-bottom-left--enabled obj-bottom--active obj-bottom--hover obj-bottom--focus obj-bottom--disabled obj-bottom--enabled obj-bottom-right--active obj-bottom-right--hover obj-bottom-right--focus obj-bottom-right--disabled obj-bottom-right--enabled`

### `$overflow-atoms`
`overflow-auto overflow-hidden overflow-visible overflow-scroll`

### `$overflow-atoms--breakpoints`
`overflow-auto--sm overflow-auto--md overflow-auto--lg overflow-auto--xl overflow-hidden--sm overflow-hidden--md overflow-hidden--lg overflow-hidden--xl overflow-visible--sm overflow-visible--md overflow-visible--lg overflow-visible--xl overflow-scroll--sm overflow-scroll--md overflow-scroll--lg overflow-scroll--xl`

### `$overflow-atoms--states`
`overflow-auto--active overflow-auto--hover overflow-auto--focus overflow-auto--disabled overflow-auto--enabled overflow-hidden--active overflow-hidden--hover overflow-hidden--focus overflow-hidden--disabled overflow-hidden--enabled overflow-visible--active overflow-visible--hover overflow-visible--focus overflow-visible--disabled overflow-visible--enabled overflow-scroll--active overflow-scroll--hover overflow-scroll--focus overflow-scroll--disabled overflow-scroll--enabled`

### `$overflow-x-atoms`
`overflow-x-auto overflow-x-hidden overflow-x-visible overflow-x-scroll`

### `$overflow-x-atoms--breakpoints`
`overflow-x-auto--sm overflow-x-auto--md overflow-x-auto--lg overflow-x-auto--xl overflow-x-hidden--sm overflow-x-hidden--md overflow-x-hidden--lg overflow-x-hidden--xl overflow-x-visible--sm overflow-x-visible--md overflow-x-visible--lg overflow-x-visible--xl overflow-x-scroll--sm overflow-x-scroll--md overflow-x-scroll--lg overflow-x-scroll--xl`

### `$overflow-x-atoms--states`
`overflow-x-auto--active overflow-x-auto--hover overflow-x-auto--focus overflow-x-auto--disabled overflow-x-auto--enabled overflow-x-hidden--active overflow-x-hidden--hover overflow-x-hidden--focus overflow-x-hidden--disabled overflow-x-hidden--enabled overflow-x-visible--active overflow-x-visible--hover overflow-x-visible--focus overflow-x-visible--disabled overflow-x-visible--enabled overflow-x-scroll--active overflow-x-scroll--hover overflow-x-scroll--focus overflow-x-scroll--disabled overflow-x-scroll--enabled`

### `$overflow-y-atoms`
`overflow-y-auto overflow-y-hidden overflow-y-visible overflow-y-scroll`

### `$overflow-y-atoms--breakpoints`
`overflow-y-auto--sm overflow-y-auto--md overflow-y-auto--lg overflow-y-auto--xl overflow-y-hidden--sm overflow-y-hidden--md overflow-y-hidden--lg overflow-y-hidden--xl overflow-y-visible--sm overflow-y-visible--md overflow-y-visible--lg overflow-y-visible--xl overflow-y-scroll--sm overflow-y-scroll--md overflow-y-scroll--lg overflow-y-scroll--xl`

### `$overflow-y-atoms--states`
`overflow-y-auto--active overflow-y-auto--hover overflow-y-auto--focus overflow-y-auto--disabled overflow-y-auto--enabled overflow-y-hidden--active overflow-y-hidden--hover overflow-y-hidden--focus overflow-y-hidden--disabled overflow-y-hidden--enabled overflow-y-visible--active overflow-y-visible--hover overflow-y-visible--focus overflow-y-visible--disabled overflow-y-visible--enabled overflow-y-scroll--active overflow-y-scroll--hover overflow-y-scroll--focus overflow-y-scroll--disabled overflow-y-scroll--enabled`

### `$padding-atoms`
`p-0 p-1 p-2 p-3 p-4 p-5 p-6`

### `$padding-atoms--breakpoints`
`p-0--sm p-0--md p-0--lg p-0--xl p-1--sm p-1--md p-1--lg p-1--xl p-2--sm p-2--md p-2--lg p-2--xl p-3--sm p-3--md p-3--lg p-3--xl p-4--sm p-4--md p-4--lg p-4--xl p-5--sm p-5--md p-5--lg p-5--xl p-6--sm p-6--md p-6--lg p-6--xl`

### `$padding-atoms--states`
`p-0--active p-0--hover p-0--focus p-0--disabled p-0--enabled p-1--active p-1--hover p-1--focus p-1--disabled p-1--enabled p-2--active p-2--hover p-2--focus p-2--disabled p-2--enabled p-3--active p-3--hover p-3--focus p-3--disabled p-3--enabled p-4--active p-4--hover p-4--focus p-4--disabled p-4--enabled p-5--active p-5--hover p-5--focus p-5--disabled p-5--enabled p-6--active p-6--hover p-6--focus p-6--disabled p-6--enabled`

### `$padding-x-atoms`
`px-0 px-1 px-2 px-3 px-4 px-5 px-6`

### `$padding-x-atoms--breakpoints`
`px-0--sm px-0--md px-0--lg px-0--xl px-1--sm px-1--md px-1--lg px-1--xl px-2--sm px-2--md px-2--lg px-2--xl px-3--sm px-3--md px-3--lg px-3--xl px-4--sm px-4--md px-4--lg px-4--xl px-5--sm px-5--md px-5--lg px-5--xl px-6--sm px-6--md px-6--lg px-6--xl`

### `$padding-x-atoms--states`
`px-0--active px-0--hover px-0--focus px-0--disabled px-0--enabled px-1--active px-1--hover px-1--focus px-1--disabled px-1--enabled px-2--active px-2--hover px-2--focus px-2--disabled px-2--enabled px-3--active px-3--hover px-3--focus px-3--disabled px-3--enabled px-4--active px-4--hover px-4--focus px-4--disabled px-4--enabled px-5--active px-5--hover px-5--focus px-5--disabled px-5--enabled px-6--active px-6--hover px-6--focus px-6--disabled px-6--enabled`

### `$padding-y-atoms`
`py-0 py-1 py-2 py-3 py-4 py-5 py-6`

### `$padding-y-atoms--breakpoints`
`py-0--sm py-0--md py-0--lg py-0--xl py-1--sm py-1--md py-1--lg py-1--xl py-2--sm py-2--md py-2--lg py-2--xl py-3--sm py-3--md py-3--lg py-3--xl py-4--sm py-4--md py-4--lg py-4--xl py-5--sm py-5--md py-5--lg py-5--xl py-6--sm py-6--md py-6--lg py-6--xl`

### `$padding-y-atoms--states`
`py-0--active py-0--hover py-0--focus py-0--disabled py-0--enabled py-1--active py-1--hover py-1--focus py-1--disabled py-1--enabled py-2--active py-2--hover py-2--focus py-2--disabled py-2--enabled py-3--active py-3--hover py-3--focus py-3--disabled py-3--enabled py-4--active py-4--hover py-4--focus py-4--disabled py-4--enabled py-5--active py-5--hover py-5--focus py-5--disabled py-5--enabled py-6--active py-6--hover py-6--focus py-6--disabled py-6--enabled`

### `$padding-t-atoms`
`pt-0 pt-1 pt-2 pt-3 pt-4 pt-5 pt-6`

### `$padding-t-atoms--breakpoints`
`pt-0--sm pt-0--md pt-0--lg pt-0--xl pt-1--sm pt-1--md pt-1--lg pt-1--xl pt-2--sm pt-2--md pt-2--lg pt-2--xl pt-3--sm pt-3--md pt-3--lg pt-3--xl pt-4--sm pt-4--md pt-4--lg pt-4--xl pt-5--sm pt-5--md pt-5--lg pt-5--xl pt-6--sm pt-6--md pt-6--lg pt-6--xl`

### `$padding-t-atoms--states`
`pt-0--active pt-0--hover pt-0--focus pt-0--disabled pt-0--enabled pt-1--active pt-1--hover pt-1--focus pt-1--disabled pt-1--enabled pt-2--active pt-2--hover pt-2--focus pt-2--disabled pt-2--enabled pt-3--active pt-3--hover pt-3--focus pt-3--disabled pt-3--enabled pt-4--active pt-4--hover pt-4--focus pt-4--disabled pt-4--enabled pt-5--active pt-5--hover pt-5--focus pt-5--disabled pt-5--enabled pt-6--active pt-6--hover pt-6--focus pt-6--disabled pt-6--enabled`

### `$padding-r-atoms`
`pr-0 pr-1 pr-2 pr-3 pr-4 pr-5 pr-6`

### `$padding-r-atoms--breakpoints`
`pr-0--sm pr-0--md pr-0--lg pr-0--xl pr-1--sm pr-1--md pr-1--lg pr-1--xl pr-2--sm pr-2--md pr-2--lg pr-2--xl pr-3--sm pr-3--md pr-3--lg pr-3--xl pr-4--sm pr-4--md pr-4--lg pr-4--xl pr-5--sm pr-5--md pr-5--lg pr-5--xl pr-6--sm pr-6--md pr-6--lg pr-6--xl`

### `$padding-r-atoms--states`
`pr-0--active pr-0--hover pr-0--focus pr-0--disabled pr-0--enabled pr-1--active pr-1--hover pr-1--focus pr-1--disabled pr-1--enabled pr-2--active pr-2--hover pr-2--focus pr-2--disabled pr-2--enabled pr-3--active pr-3--hover pr-3--focus pr-3--disabled pr-3--enabled pr-4--active pr-4--hover pr-4--focus pr-4--disabled pr-4--enabled pr-5--active pr-5--hover pr-5--focus pr-5--disabled pr-5--enabled pr-6--active pr-6--hover pr-6--focus pr-6--disabled pr-6--enabled`

### `$padding-b-atoms`
`pb-0 pb-1 pb-2 pb-3 pb-4 pb-5 pb-6`

### `$padding-b-atoms--breakpoints`
`pb-0--sm pb-0--md pb-0--lg pb-0--xl pb-1--sm pb-1--md pb-1--lg pb-1--xl pb-2--sm pb-2--md pb-2--lg pb-2--xl pb-3--sm pb-3--md pb-3--lg pb-3--xl pb-4--sm pb-4--md pb-4--lg pb-4--xl pb-5--sm pb-5--md pb-5--lg pb-5--xl pb-6--sm pb-6--md pb-6--lg pb-6--xl`

### `$padding-b-atoms--states`
`pb-0--active pb-0--hover pb-0--focus pb-0--disabled pb-0--enabled pb-1--active pb-1--hover pb-1--focus pb-1--disabled pb-1--enabled pb-2--active pb-2--hover pb-2--focus pb-2--disabled pb-2--enabled pb-3--active pb-3--hover pb-3--focus pb-3--disabled pb-3--enabled pb-4--active pb-4--hover pb-4--focus pb-4--disabled pb-4--enabled pb-5--active pb-5--hover pb-5--focus pb-5--disabled pb-5--enabled pb-6--active pb-6--hover pb-6--focus pb-6--disabled pb-6--enabled`

### `$padding-l-atoms`
`pl-0 pl-1 pl-2 pl-3 pl-4 pl-5 pl-6`

### `$padding-l-atoms--breakpoints`
`pl-0--sm pl-0--md pl-0--lg pl-0--xl pl-1--sm pl-1--md pl-1--lg pl-1--xl pl-2--sm pl-2--md pl-2--lg pl-2--xl pl-3--sm pl-3--md pl-3--lg pl-3--xl pl-4--sm pl-4--md pl-4--lg pl-4--xl pl-5--sm pl-5--md pl-5--lg pl-5--xl pl-6--sm pl-6--md pl-6--lg pl-6--xl`

### `$padding-l-atoms--states`
`pl-0--active pl-0--hover pl-0--focus pl-0--disabled pl-0--enabled pl-1--active pl-1--hover pl-1--focus pl-1--disabled pl-1--enabled pl-2--active pl-2--hover pl-2--focus pl-2--disabled pl-2--enabled pl-3--active pl-3--hover pl-3--focus pl-3--disabled pl-3--enabled pl-4--active pl-4--hover pl-4--focus pl-4--disabled pl-4--enabled pl-5--active pl-5--hover pl-5--focus pl-5--disabled pl-5--enabled pl-6--active pl-6--hover pl-6--focus pl-6--disabled pl-6--enabled`

### `$position-atoms`
`pos-static pos-fixed pos-absolute pos-relative pos-sticky`

### `$position-atoms--breakpoints`
`pos-static--sm pos-static--md pos-static--lg pos-static--xl pos-fixed--sm pos-fixed--md pos-fixed--lg pos-fixed--xl pos-absolute--sm pos-absolute--md pos-absolute--lg pos-absolute--xl pos-relative--sm pos-relative--md pos-relative--lg pos-relative--xl pos-sticky--sm pos-sticky--md pos-sticky--lg pos-sticky--xl`

### `$position-atoms--states`
`pos-static--active pos-static--hover pos-static--focus pos-static--disabled pos-static--enabled pos-fixed--active pos-fixed--hover pos-fixed--focus pos-fixed--disabled pos-fixed--enabled pos-absolute--active pos-absolute--hover pos-absolute--focus pos-absolute--disabled pos-absolute--enabled pos-relative--active pos-relative--hover pos-relative--focus pos-relative--disabled pos-relative--enabled pos-sticky--active pos-sticky--hover pos-sticky--focus pos-sticky--disabled pos-sticky--enabled`

### `$text-align-atoms`
`text-left text-center text-right text-justify`

### `$text-align-atoms--breakpoints`
`text-left--sm text-left--md text-left--lg text-left--xl text-center--sm text-center--md text-center--lg text-center--xl text-right--sm text-right--md text-right--lg text-right--xl text-justify--sm text-justify--md text-justify--lg text-justify--xl`

### `$text-align-atoms--states`
`text-left--active text-left--hover text-left--focus text-left--disabled text-left--enabled text-center--active text-center--hover text-center--focus text-center--disabled text-center--enabled text-right--active text-right--hover text-right--focus text-right--disabled text-right--enabled text-justify--active text-justify--hover text-justify--focus text-justify--disabled text-justify--enabled`

### `$text-color-atoms`
`text-primary-50 text-primary-100 text-primary-200 text-primary-300 text-primary-400 text-primary-500 text-primary-600 text-primary-700 text-primary-800 text-primary-900 text-secondary-50 text-secondary-100 text-secondary-200 text-secondary-300 text-secondary-400 text-secondary-500 text-secondary-600 text-secondary-700 text-secondary-800 text-secondary-900 text-link-50 text-link-100 text-link-200 text-link-300 text-link-400 text-link-500 text-link-600 text-link-700 text-link-800 text-link-900 text-muted-50 text-muted-100 text-muted-200 text-muted-300 text-muted-400 text-muted-500 text-muted-600 text-muted-700 text-muted-800 text-muted-900 text-success-50 text-success-100 text-success-200 text-success-300 text-success-400 text-success-500 text-success-600 text-success-700 text-success-800 text-success-900 text-warning-50 text-warning-100 text-warning-200 text-warning-300 text-warning-400 text-warning-500 text-warning-600 text-warning-700 text-warning-800 text-warning-900 text-error-50 text-error-100 text-error-200 text-error-300 text-error-400 text-error-500 text-error-600 text-error-700 text-error-800 text-error-900 text-gray-50 text-gray-100 text-gray-200 text-gray-300 text-gray-400 text-gray-500 text-gray-600 text-gray-700 text-gray-800 text-gray-900`

### `$text-color-atoms--breakpoints`
`text-primary-50--sm text-primary-50--md text-primary-50--lg text-primary-50--xl text-primary-100--sm text-primary-100--md text-primary-100--lg text-primary-100--xl text-primary-200--sm text-primary-200--md text-primary-200--lg text-primary-200--xl text-primary-300--sm text-primary-300--md text-primary-300--lg text-primary-300--xl text-primary-400--sm text-primary-400--md text-primary-400--lg text-primary-400--xl text-primary-500--sm text-primary-500--md text-primary-500--lg text-primary-500--xl text-primary-600--sm text-primary-600--md text-primary-600--lg text-primary-600--xl text-primary-700--sm text-primary-700--md text-primary-700--lg text-primary-700--xl text-primary-800--sm text-primary-800--md text-primary-800--lg text-primary-800--xl text-primary-900--sm text-primary-900--md text-primary-900--lg text-primary-900--xl text-secondary-50--sm text-secondary-50--md text-secondary-50--lg text-secondary-50--xl text-secondary-100--sm text-secondary-100--md text-secondary-100--lg text-secondary-100--xl text-secondary-200--sm text-secondary-200--md text-secondary-200--lg text-secondary-200--xl text-secondary-300--sm text-secondary-300--md text-secondary-300--lg text-secondary-300--xl text-secondary-400--sm text-secondary-400--md text-secondary-400--lg text-secondary-400--xl text-secondary-500--sm text-secondary-500--md text-secondary-500--lg text-secondary-500--xl text-secondary-600--sm text-secondary-600--md text-secondary-600--lg text-secondary-600--xl text-secondary-700--sm text-secondary-700--md text-secondary-700--lg text-secondary-700--xl text-secondary-800--sm text-secondary-800--md text-secondary-800--lg text-secondary-800--xl text-secondary-900--sm text-secondary-900--md text-secondary-900--lg text-secondary-900--xl text-link-50--sm text-link-50--md text-link-50--lg text-link-50--xl text-link-100--sm text-link-100--md text-link-100--lg text-link-100--xl text-link-200--sm text-link-200--md text-link-200--lg text-link-200--xl text-link-300--sm text-link-300--md text-link-300--lg text-link-300--xl text-link-400--sm text-link-400--md text-link-400--lg text-link-400--xl text-link-500--sm text-link-500--md text-link-500--lg text-link-500--xl text-link-600--sm text-link-600--md text-link-600--lg text-link-600--xl text-link-700--sm text-link-700--md text-link-700--lg text-link-700--xl text-link-800--sm text-link-800--md text-link-800--lg text-link-800--xl text-link-900--sm text-link-900--md text-link-900--lg text-link-900--xl text-muted-50--sm text-muted-50--md text-muted-50--lg text-muted-50--xl text-muted-100--sm text-muted-100--md text-muted-100--lg text-muted-100--xl text-muted-200--sm text-muted-200--md text-muted-200--lg text-muted-200--xl text-muted-300--sm text-muted-300--md text-muted-300--lg text-muted-300--xl text-muted-400--sm text-muted-400--md text-muted-400--lg text-muted-400--xl text-muted-500--sm text-muted-500--md text-muted-500--lg text-muted-500--xl text-muted-600--sm text-muted-600--md text-muted-600--lg text-muted-600--xl text-muted-700--sm text-muted-700--md text-muted-700--lg text-muted-700--xl text-muted-800--sm text-muted-800--md text-muted-800--lg text-muted-800--xl text-muted-900--sm text-muted-900--md text-muted-900--lg text-muted-900--xl text-success-50--sm text-success-50--md text-success-50--lg text-success-50--xl text-success-100--sm text-success-100--md text-success-100--lg text-success-100--xl text-success-200--sm text-success-200--md text-success-200--lg text-success-200--xl text-success-300--sm text-success-300--md text-success-300--lg text-success-300--xl text-success-400--sm text-success-400--md text-success-400--lg text-success-400--xl text-success-500--sm text-success-500--md text-success-500--lg text-success-500--xl text-success-600--sm text-success-600--md text-success-600--lg text-success-600--xl text-success-700--sm text-success-700--md text-success-700--lg text-success-700--xl text-success-800--sm text-success-800--md text-success-800--lg text-success-800--xl text-success-900--sm text-success-900--md text-success-900--lg text-success-900--xl text-warning-50--sm text-warning-50--md text-warning-50--lg text-warning-50--xl text-warning-100--sm text-warning-100--md text-warning-100--lg text-warning-100--xl text-warning-200--sm text-warning-200--md text-warning-200--lg text-warning-200--xl text-warning-300--sm text-warning-300--md text-warning-300--lg text-warning-300--xl text-warning-400--sm text-warning-400--md text-warning-400--lg text-warning-400--xl text-warning-500--sm text-warning-500--md text-warning-500--lg text-warning-500--xl text-warning-600--sm text-warning-600--md text-warning-600--lg text-warning-600--xl text-warning-700--sm text-warning-700--md text-warning-700--lg text-warning-700--xl text-warning-800--sm text-warning-800--md text-warning-800--lg text-warning-800--xl text-warning-900--sm text-warning-900--md text-warning-900--lg text-warning-900--xl text-error-50--sm text-error-50--md text-error-50--lg text-error-50--xl text-error-100--sm text-error-100--md text-error-100--lg text-error-100--xl text-error-200--sm text-error-200--md text-error-200--lg text-error-200--xl text-error-300--sm text-error-300--md text-error-300--lg text-error-300--xl text-error-400--sm text-error-400--md text-error-400--lg text-error-400--xl text-error-500--sm text-error-500--md text-error-500--lg text-error-500--xl text-error-600--sm text-error-600--md text-error-600--lg text-error-600--xl text-error-700--sm text-error-700--md text-error-700--lg text-error-700--xl text-error-800--sm text-error-800--md text-error-800--lg text-error-800--xl text-error-900--sm text-error-900--md text-error-900--lg text-error-900--xl text-gray-50--sm text-gray-50--md text-gray-50--lg text-gray-50--xl text-gray-100--sm text-gray-100--md text-gray-100--lg text-gray-100--xl text-gray-200--sm text-gray-200--md text-gray-200--lg text-gray-200--xl text-gray-300--sm text-gray-300--md text-gray-300--lg text-gray-300--xl text-gray-400--sm text-gray-400--md text-gray-400--lg text-gray-400--xl text-gray-500--sm text-gray-500--md text-gray-500--lg text-gray-500--xl text-gray-600--sm text-gray-600--md text-gray-600--lg text-gray-600--xl text-gray-700--sm text-gray-700--md text-gray-700--lg text-gray-700--xl text-gray-800--sm text-gray-800--md text-gray-800--lg text-gray-800--xl text-gray-900--sm text-gray-900--md text-gray-900--lg text-gray-900--xl`

### `$text-color-atoms--states`
`text-primary-50--active text-primary-50--hover text-primary-50--focus text-primary-50--disabled text-primary-50--enabled text-primary-100--active text-primary-100--hover text-primary-100--focus text-primary-100--disabled text-primary-100--enabled text-primary-200--active text-primary-200--hover text-primary-200--focus text-primary-200--disabled text-primary-200--enabled text-primary-300--active text-primary-300--hover text-primary-300--focus text-primary-300--disabled text-primary-300--enabled text-primary-400--active text-primary-400--hover text-primary-400--focus text-primary-400--disabled text-primary-400--enabled text-primary-500--active text-primary-500--hover text-primary-500--focus text-primary-500--disabled text-primary-500--enabled text-primary-600--active text-primary-600--hover text-primary-600--focus text-primary-600--disabled text-primary-600--enabled text-primary-700--active text-primary-700--hover text-primary-700--focus text-primary-700--disabled text-primary-700--enabled text-primary-800--active text-primary-800--hover text-primary-800--focus text-primary-800--disabled text-primary-800--enabled text-primary-900--active text-primary-900--hover text-primary-900--focus text-primary-900--disabled text-primary-900--enabled text-secondary-50--active text-secondary-50--hover text-secondary-50--focus text-secondary-50--disabled text-secondary-50--enabled text-secondary-100--active text-secondary-100--hover text-secondary-100--focus text-secondary-100--disabled text-secondary-100--enabled text-secondary-200--active text-secondary-200--hover text-secondary-200--focus text-secondary-200--disabled text-secondary-200--enabled text-secondary-300--active text-secondary-300--hover text-secondary-300--focus text-secondary-300--disabled text-secondary-300--enabled text-secondary-400--active text-secondary-400--hover text-secondary-400--focus text-secondary-400--disabled text-secondary-400--enabled text-secondary-500--active text-secondary-500--hover text-secondary-500--focus text-secondary-500--disabled text-secondary-500--enabled text-secondary-600--active text-secondary-600--hover text-secondary-600--focus text-secondary-600--disabled text-secondary-600--enabled text-secondary-700--active text-secondary-700--hover text-secondary-700--focus text-secondary-700--disabled text-secondary-700--enabled text-secondary-800--active text-secondary-800--hover text-secondary-800--focus text-secondary-800--disabled text-secondary-800--enabled text-secondary-900--active text-secondary-900--hover text-secondary-900--focus text-secondary-900--disabled text-secondary-900--enabled text-link-50--active text-link-50--hover text-link-50--focus text-link-50--disabled text-link-50--enabled text-link-100--active text-link-100--hover text-link-100--focus text-link-100--disabled text-link-100--enabled text-link-200--active text-link-200--hover text-link-200--focus text-link-200--disabled text-link-200--enabled text-link-300--active text-link-300--hover text-link-300--focus text-link-300--disabled text-link-300--enabled text-link-400--active text-link-400--hover text-link-400--focus text-link-400--disabled text-link-400--enabled text-link-500--active text-link-500--hover text-link-500--focus text-link-500--disabled text-link-500--enabled text-link-600--active text-link-600--hover text-link-600--focus text-link-600--disabled text-link-600--enabled text-link-700--active text-link-700--hover text-link-700--focus text-link-700--disabled text-link-700--enabled text-link-800--active text-link-800--hover text-link-800--focus text-link-800--disabled text-link-800--enabled text-link-900--active text-link-900--hover text-link-900--focus text-link-900--disabled text-link-900--enabled text-muted-50--active text-muted-50--hover text-muted-50--focus text-muted-50--disabled text-muted-50--enabled text-muted-100--active text-muted-100--hover text-muted-100--focus text-muted-100--disabled text-muted-100--enabled text-muted-200--active text-muted-200--hover text-muted-200--focus text-muted-200--disabled text-muted-200--enabled text-muted-300--active text-muted-300--hover text-muted-300--focus text-muted-300--disabled text-muted-300--enabled text-muted-400--active text-muted-400--hover text-muted-400--focus text-muted-400--disabled text-muted-400--enabled text-muted-500--active text-muted-500--hover text-muted-500--focus text-muted-500--disabled text-muted-500--enabled text-muted-600--active text-muted-600--hover text-muted-600--focus text-muted-600--disabled text-muted-600--enabled text-muted-700--active text-muted-700--hover text-muted-700--focus text-muted-700--disabled text-muted-700--enabled text-muted-800--active text-muted-800--hover text-muted-800--focus text-muted-800--disabled text-muted-800--enabled text-muted-900--active text-muted-900--hover text-muted-900--focus text-muted-900--disabled text-muted-900--enabled text-success-50--active text-success-50--hover text-success-50--focus text-success-50--disabled text-success-50--enabled text-success-100--active text-success-100--hover text-success-100--focus text-success-100--disabled text-success-100--enabled text-success-200--active text-success-200--hover text-success-200--focus text-success-200--disabled text-success-200--enabled text-success-300--active text-success-300--hover text-success-300--focus text-success-300--disabled text-success-300--enabled text-success-400--active text-success-400--hover text-success-400--focus text-success-400--disabled text-success-400--enabled text-success-500--active text-success-500--hover text-success-500--focus text-success-500--disabled text-success-500--enabled text-success-600--active text-success-600--hover text-success-600--focus text-success-600--disabled text-success-600--enabled text-success-700--active text-success-700--hover text-success-700--focus text-success-700--disabled text-success-700--enabled text-success-800--active text-success-800--hover text-success-800--focus text-success-800--disabled text-success-800--enabled text-success-900--active text-success-900--hover text-success-900--focus text-success-900--disabled text-success-900--enabled text-warning-50--active text-warning-50--hover text-warning-50--focus text-warning-50--disabled text-warning-50--enabled text-warning-100--active text-warning-100--hover text-warning-100--focus text-warning-100--disabled text-warning-100--enabled text-warning-200--active text-warning-200--hover text-warning-200--focus text-warning-200--disabled text-warning-200--enabled text-warning-300--active text-warning-300--hover text-warning-300--focus text-warning-300--disabled text-warning-300--enabled text-warning-400--active text-warning-400--hover text-warning-400--focus text-warning-400--disabled text-warning-400--enabled text-warning-500--active text-warning-500--hover text-warning-500--focus text-warning-500--disabled text-warning-500--enabled text-warning-600--active text-warning-600--hover text-warning-600--focus text-warning-600--disabled text-warning-600--enabled text-warning-700--active text-warning-700--hover text-warning-700--focus text-warning-700--disabled text-warning-700--enabled text-warning-800--active text-warning-800--hover text-warning-800--focus text-warning-800--disabled text-warning-800--enabled text-warning-900--active text-warning-900--hover text-warning-900--focus text-warning-900--disabled text-warning-900--enabled text-error-50--active text-error-50--hover text-error-50--focus text-error-50--disabled text-error-50--enabled text-error-100--active text-error-100--hover text-error-100--focus text-error-100--disabled text-error-100--enabled text-error-200--active text-error-200--hover text-error-200--focus text-error-200--disabled text-error-200--enabled text-error-300--active text-error-300--hover text-error-300--focus text-error-300--disabled text-error-300--enabled text-error-400--active text-error-400--hover text-error-400--focus text-error-400--disabled text-error-400--enabled text-error-500--active text-error-500--hover text-error-500--focus text-error-500--disabled text-error-500--enabled text-error-600--active text-error-600--hover text-error-600--focus text-error-600--disabled text-error-600--enabled text-error-700--active text-error-700--hover text-error-700--focus text-error-700--disabled text-error-700--enabled text-error-800--active text-error-800--hover text-error-800--focus text-error-800--disabled text-error-800--enabled text-error-900--active text-error-900--hover text-error-900--focus text-error-900--disabled text-error-900--enabled text-gray-50--active text-gray-50--hover text-gray-50--focus text-gray-50--disabled text-gray-50--enabled text-gray-100--active text-gray-100--hover text-gray-100--focus text-gray-100--disabled text-gray-100--enabled text-gray-200--active text-gray-200--hover text-gray-200--focus text-gray-200--disabled text-gray-200--enabled text-gray-300--active text-gray-300--hover text-gray-300--focus text-gray-300--disabled text-gray-300--enabled text-gray-400--active text-gray-400--hover text-gray-400--focus text-gray-400--disabled text-gray-400--enabled text-gray-500--active text-gray-500--hover text-gray-500--focus text-gray-500--disabled text-gray-500--enabled text-gray-600--active text-gray-600--hover text-gray-600--focus text-gray-600--disabled text-gray-600--enabled text-gray-700--active text-gray-700--hover text-gray-700--focus text-gray-700--disabled text-gray-700--enabled text-gray-800--active text-gray-800--hover text-gray-800--focus text-gray-800--disabled text-gray-800--enabled text-gray-900--active text-gray-900--hover text-gray-900--focus text-gray-900--disabled text-gray-900--enabled`

### `$text-size-atoms`
`text-xs text-sm text-md text-lg text-xl text-2xl text-3xl text-4xl text-5xl text-6xl`

### `$text-size-atoms--breakpoints`
`text-xs--sm text-xs--md text-xs--lg text-xs--xl text-sm--sm text-sm--md text-sm--lg text-sm--xl text-md--sm text-md--md text-md--lg text-md--xl text-lg--sm text-lg--md text-lg--lg text-lg--xl text-xl--sm text-xl--md text-xl--lg text-xl--xl text-2xl--sm text-2xl--md text-2xl--lg text-2xl--xl text-3xl--sm text-3xl--md text-3xl--lg text-3xl--xl text-4xl--sm text-4xl--md text-4xl--lg text-4xl--xl text-5xl--sm text-5xl--md text-5xl--lg text-5xl--xl text-6xl--sm text-6xl--md text-6xl--lg text-6xl--xl`

### `$text-size-atoms--states`
`text-xs--active text-xs--hover text-xs--focus text-xs--disabled text-xs--enabled text-sm--active text-sm--hover text-sm--focus text-sm--disabled text-sm--enabled text-md--active text-md--hover text-md--focus text-md--disabled text-md--enabled text-lg--active text-lg--hover text-lg--focus text-lg--disabled text-lg--enabled text-xl--active text-xl--hover text-xl--focus text-xl--disabled text-xl--enabled text-2xl--active text-2xl--hover text-2xl--focus text-2xl--disabled text-2xl--enabled text-3xl--active text-3xl--hover text-3xl--focus text-3xl--disabled text-3xl--enabled text-4xl--active text-4xl--hover text-4xl--focus text-4xl--disabled text-4xl--enabled text-5xl--active text-5xl--hover text-5xl--focus text-5xl--disabled text-5xl--enabled text-6xl--active text-6xl--hover text-6xl--focus text-6xl--disabled text-6xl--enabled`

### `$visibility-atoms`
`v-visible v-hidden`

### `$visibility-atoms--breakpoints`
`v-visible--sm v-visible--md v-visible--lg v-visible--xl v-hidden--sm v-hidden--md v-hidden--lg v-hidden--xl`

### `$visibility-atoms--states`
`v-visible--active v-visible--hover v-visible--focus v-visible--disabled v-visible--enabled v-hidden--active v-hidden--hover v-hidden--focus v-hidden--disabled v-hidden--enabled`

### `$width-atoms`
`w-0 w-vw0 w-vw1 w-vw2 w-vw3 w-vw4 w-vw5 w-vw6 w-vw7 w-vw8 w-vw9 w-vw10 w-vw11 w-vw12 w-p0 w-p1 w-p2 w-p3 w-p4 w-p5 w-p6 w-p7 w-p8 w-p9 w-p10 w-p11 w-p12`

### `$width-atoms--breakpoints`
`w-0--sm w-0--md w-0--lg w-0--xl w-vw0--sm w-vw0--md w-vw0--lg w-vw0--xl w-vw1--sm w-vw1--md w-vw1--lg w-vw1--xl w-vw2--sm w-vw2--md w-vw2--lg w-vw2--xl w-vw3--sm w-vw3--md w-vw3--lg w-vw3--xl w-vw4--sm w-vw4--md w-vw4--lg w-vw4--xl w-vw5--sm w-vw5--md w-vw5--lg w-vw5--xl w-vw6--sm w-vw6--md w-vw6--lg w-vw6--xl w-vw7--sm w-vw7--md w-vw7--lg w-vw7--xl w-vw8--sm w-vw8--md w-vw8--lg w-vw8--xl w-vw9--sm w-vw9--md w-vw9--lg w-vw9--xl w-vw10--sm w-vw10--md w-vw10--lg w-vw10--xl w-vw11--sm w-vw11--md w-vw11--lg w-vw11--xl w-vw12--sm w-vw12--md w-vw12--lg w-vw12--xl w-p0--sm w-p0--md w-p0--lg w-p0--xl w-p1--sm w-p1--md w-p1--lg w-p1--xl w-p2--sm w-p2--md w-p2--lg w-p2--xl w-p3--sm w-p3--md w-p3--lg w-p3--xl w-p4--sm w-p4--md w-p4--lg w-p4--xl w-p5--sm w-p5--md w-p5--lg w-p5--xl w-p6--sm w-p6--md w-p6--lg w-p6--xl w-p7--sm w-p7--md w-p7--lg w-p7--xl w-p8--sm w-p8--md w-p8--lg w-p8--xl w-p9--sm w-p9--md w-p9--lg w-p9--xl w-p10--sm w-p10--md w-p10--lg w-p10--xl w-p11--sm w-p11--md w-p11--lg w-p11--xl w-p12--sm w-p12--md w-p12--lg w-p12--xl`

### `$width-atoms--states`
`w-0--active w-0--hover w-0--focus w-0--disabled w-0--enabled w-vw0--active w-vw0--hover w-vw0--focus w-vw0--disabled w-vw0--enabled w-vw1--active w-vw1--hover w-vw1--focus w-vw1--disabled w-vw1--enabled w-vw2--active w-vw2--hover w-vw2--focus w-vw2--disabled w-vw2--enabled w-vw3--active w-vw3--hover w-vw3--focus w-vw3--disabled w-vw3--enabled w-vw4--active w-vw4--hover w-vw4--focus w-vw4--disabled w-vw4--enabled w-vw5--active w-vw5--hover w-vw5--focus w-vw5--disabled w-vw5--enabled w-vw6--active w-vw6--hover w-vw6--focus w-vw6--disabled w-vw6--enabled w-vw7--active w-vw7--hover w-vw7--focus w-vw7--disabled w-vw7--enabled w-vw8--active w-vw8--hover w-vw8--focus w-vw8--disabled w-vw8--enabled w-vw9--active w-vw9--hover w-vw9--focus w-vw9--disabled w-vw9--enabled w-vw10--active w-vw10--hover w-vw10--focus w-vw10--disabled w-vw10--enabled w-vw11--active w-vw11--hover w-vw11--focus w-vw11--disabled w-vw11--enabled w-vw12--active w-vw12--hover w-vw12--focus w-vw12--disabled w-vw12--enabled w-p0--active w-p0--hover w-p0--focus w-p0--disabled w-p0--enabled w-p1--active w-p1--hover w-p1--focus w-p1--disabled w-p1--enabled w-p2--active w-p2--hover w-p2--focus w-p2--disabled w-p2--enabled w-p3--active w-p3--hover w-p3--focus w-p3--disabled w-p3--enabled w-p4--active w-p4--hover w-p4--focus w-p4--disabled w-p4--enabled w-p5--active w-p5--hover w-p5--focus w-p5--disabled w-p5--enabled w-p6--active w-p6--hover w-p6--focus w-p6--disabled w-p6--enabled w-p7--active w-p7--hover w-p7--focus w-p7--disabled w-p7--enabled w-p8--active w-p8--hover w-p8--focus w-p8--disabled w-p8--enabled w-p9--active w-p9--hover w-p9--focus w-p9--disabled w-p9--enabled w-p10--active w-p10--hover w-p10--focus w-p10--disabled w-p10--enabled w-p11--active w-p11--hover w-p11--focus w-p11--disabled w-p11--enabled w-p12--active w-p12--hover w-p12--focus w-p12--disabled w-p12--enabled`

### `$min-width-atoms`
`wmin-0 wmin-vw0 wmin-vw1 wmin-vw2 wmin-vw3 wmin-vw4 wmin-vw5 wmin-vw6 wmin-vw7 wmin-vw8 wmin-vw9 wmin-vw10 wmin-vw11 wmin-vw12 wmin-p0 wmin-p1 wmin-p2 wmin-p3 wmin-p4 wmin-p5 wmin-p6 wmin-p7 wmin-p8 wmin-p9 wmin-p10 wmin-p11 wmin-p12`

### `$min-width-atoms--breakpoints`
`wmin-0--sm wmin-0--md wmin-0--lg wmin-0--xl wmin-vw0--sm wmin-vw0--md wmin-vw0--lg wmin-vw0--xl wmin-vw1--sm wmin-vw1--md wmin-vw1--lg wmin-vw1--xl wmin-vw2--sm wmin-vw2--md wmin-vw2--lg wmin-vw2--xl wmin-vw3--sm wmin-vw3--md wmin-vw3--lg wmin-vw3--xl wmin-vw4--sm wmin-vw4--md wmin-vw4--lg wmin-vw4--xl wmin-vw5--sm wmin-vw5--md wmin-vw5--lg wmin-vw5--xl wmin-vw6--sm wmin-vw6--md wmin-vw6--lg wmin-vw6--xl wmin-vw7--sm wmin-vw7--md wmin-vw7--lg wmin-vw7--xl wmin-vw8--sm wmin-vw8--md wmin-vw8--lg wmin-vw8--xl wmin-vw9--sm wmin-vw9--md wmin-vw9--lg wmin-vw9--xl wmin-vw10--sm wmin-vw10--md wmin-vw10--lg wmin-vw10--xl wmin-vw11--sm wmin-vw11--md wmin-vw11--lg wmin-vw11--xl wmin-vw12--sm wmin-vw12--md wmin-vw12--lg wmin-vw12--xl wmin-p0--sm wmin-p0--md wmin-p0--lg wmin-p0--xl wmin-p1--sm wmin-p1--md wmin-p1--lg wmin-p1--xl wmin-p2--sm wmin-p2--md wmin-p2--lg wmin-p2--xl wmin-p3--sm wmin-p3--md wmin-p3--lg wmin-p3--xl wmin-p4--sm wmin-p4--md wmin-p4--lg wmin-p4--xl wmin-p5--sm wmin-p5--md wmin-p5--lg wmin-p5--xl wmin-p6--sm wmin-p6--md wmin-p6--lg wmin-p6--xl wmin-p7--sm wmin-p7--md wmin-p7--lg wmin-p7--xl wmin-p8--sm wmin-p8--md wmin-p8--lg wmin-p8--xl wmin-p9--sm wmin-p9--md wmin-p9--lg wmin-p9--xl wmin-p10--sm wmin-p10--md wmin-p10--lg wmin-p10--xl wmin-p11--sm wmin-p11--md wmin-p11--lg wmin-p11--xl wmin-p12--sm wmin-p12--md wmin-p12--lg wmin-p12--xl`

### `$min-width-atoms--states`
`wmin-0--active wmin-0--hover wmin-0--focus wmin-0--disabled wmin-0--enabled wmin-vw0--active wmin-vw0--hover wmin-vw0--focus wmin-vw0--disabled wmin-vw0--enabled wmin-vw1--active wmin-vw1--hover wmin-vw1--focus wmin-vw1--disabled wmin-vw1--enabled wmin-vw2--active wmin-vw2--hover wmin-vw2--focus wmin-vw2--disabled wmin-vw2--enabled wmin-vw3--active wmin-vw3--hover wmin-vw3--focus wmin-vw3--disabled wmin-vw3--enabled wmin-vw4--active wmin-vw4--hover wmin-vw4--focus wmin-vw4--disabled wmin-vw4--enabled wmin-vw5--active wmin-vw5--hover wmin-vw5--focus wmin-vw5--disabled wmin-vw5--enabled wmin-vw6--active wmin-vw6--hover wmin-vw6--focus wmin-vw6--disabled wmin-vw6--enabled wmin-vw7--active wmin-vw7--hover wmin-vw7--focus wmin-vw7--disabled wmin-vw7--enabled wmin-vw8--active wmin-vw8--hover wmin-vw8--focus wmin-vw8--disabled wmin-vw8--enabled wmin-vw9--active wmin-vw9--hover wmin-vw9--focus wmin-vw9--disabled wmin-vw9--enabled wmin-vw10--active wmin-vw10--hover wmin-vw10--focus wmin-vw10--disabled wmin-vw10--enabled wmin-vw11--active wmin-vw11--hover wmin-vw11--focus wmin-vw11--disabled wmin-vw11--enabled wmin-vw12--active wmin-vw12--hover wmin-vw12--focus wmin-vw12--disabled wmin-vw12--enabled wmin-p0--active wmin-p0--hover wmin-p0--focus wmin-p0--disabled wmin-p0--enabled wmin-p1--active wmin-p1--hover wmin-p1--focus wmin-p1--disabled wmin-p1--enabled wmin-p2--active wmin-p2--hover wmin-p2--focus wmin-p2--disabled wmin-p2--enabled wmin-p3--active wmin-p3--hover wmin-p3--focus wmin-p3--disabled wmin-p3--enabled wmin-p4--active wmin-p4--hover wmin-p4--focus wmin-p4--disabled wmin-p4--enabled wmin-p5--active wmin-p5--hover wmin-p5--focus wmin-p5--disabled wmin-p5--enabled wmin-p6--active wmin-p6--hover wmin-p6--focus wmin-p6--disabled wmin-p6--enabled wmin-p7--active wmin-p7--hover wmin-p7--focus wmin-p7--disabled wmin-p7--enabled wmin-p8--active wmin-p8--hover wmin-p8--focus wmin-p8--disabled wmin-p8--enabled wmin-p9--active wmin-p9--hover wmin-p9--focus wmin-p9--disabled wmin-p9--enabled wmin-p10--active wmin-p10--hover wmin-p10--focus wmin-p10--disabled wmin-p10--enabled wmin-p11--active wmin-p11--hover wmin-p11--focus wmin-p11--disabled wmin-p11--enabled wmin-p12--active wmin-p12--hover wmin-p12--focus wmin-p12--disabled wmin-p12--enabled`

### `$max-width-atoms`
`wmax-0 wmax-vw0 wmax-vw1 wmax-vw2 wmax-vw3 wmax-vw4 wmax-vw5 wmax-vw6 wmax-vw7 wmax-vw8 wmax-vw9 wmax-vw10 wmax-vw11 wmax-vw12 wmax-p0 wmax-p1 wmax-p2 wmax-p3 wmax-p4 wmax-p5 wmax-p6 wmax-p7 wmax-p8 wmax-p9 wmax-p10 wmax-p11 wmax-p12`

### `$max-width-atoms--breakpoints`
`wmax-0--sm wmax-0--md wmax-0--lg wmax-0--xl wmax-vw0--sm wmax-vw0--md wmax-vw0--lg wmax-vw0--xl wmax-vw1--sm wmax-vw1--md wmax-vw1--lg wmax-vw1--xl wmax-vw2--sm wmax-vw2--md wmax-vw2--lg wmax-vw2--xl wmax-vw3--sm wmax-vw3--md wmax-vw3--lg wmax-vw3--xl wmax-vw4--sm wmax-vw4--md wmax-vw4--lg wmax-vw4--xl wmax-vw5--sm wmax-vw5--md wmax-vw5--lg wmax-vw5--xl wmax-vw6--sm wmax-vw6--md wmax-vw6--lg wmax-vw6--xl wmax-vw7--sm wmax-vw7--md wmax-vw7--lg wmax-vw7--xl wmax-vw8--sm wmax-vw8--md wmax-vw8--lg wmax-vw8--xl wmax-vw9--sm wmax-vw9--md wmax-vw9--lg wmax-vw9--xl wmax-vw10--sm wmax-vw10--md wmax-vw10--lg wmax-vw10--xl wmax-vw11--sm wmax-vw11--md wmax-vw11--lg wmax-vw11--xl wmax-vw12--sm wmax-vw12--md wmax-vw12--lg wmax-vw12--xl wmax-p0--sm wmax-p0--md wmax-p0--lg wmax-p0--xl wmax-p1--sm wmax-p1--md wmax-p1--lg wmax-p1--xl wmax-p2--sm wmax-p2--md wmax-p2--lg wmax-p2--xl wmax-p3--sm wmax-p3--md wmax-p3--lg wmax-p3--xl wmax-p4--sm wmax-p4--md wmax-p4--lg wmax-p4--xl wmax-p5--sm wmax-p5--md wmax-p5--lg wmax-p5--xl wmax-p6--sm wmax-p6--md wmax-p6--lg wmax-p6--xl wmax-p7--sm wmax-p7--md wmax-p7--lg wmax-p7--xl wmax-p8--sm wmax-p8--md wmax-p8--lg wmax-p8--xl wmax-p9--sm wmax-p9--md wmax-p9--lg wmax-p9--xl wmax-p10--sm wmax-p10--md wmax-p10--lg wmax-p10--xl wmax-p11--sm wmax-p11--md wmax-p11--lg wmax-p11--xl wmax-p12--sm wmax-p12--md wmax-p12--lg wmax-p12--xl`

### `$max-width-atoms--states`
`wmax-0--active wmax-0--hover wmax-0--focus wmax-0--disabled wmax-0--enabled wmax-vw0--active wmax-vw0--hover wmax-vw0--focus wmax-vw0--disabled wmax-vw0--enabled wmax-vw1--active wmax-vw1--hover wmax-vw1--focus wmax-vw1--disabled wmax-vw1--enabled wmax-vw2--active wmax-vw2--hover wmax-vw2--focus wmax-vw2--disabled wmax-vw2--enabled wmax-vw3--active wmax-vw3--hover wmax-vw3--focus wmax-vw3--disabled wmax-vw3--enabled wmax-vw4--active wmax-vw4--hover wmax-vw4--focus wmax-vw4--disabled wmax-vw4--enabled wmax-vw5--active wmax-vw5--hover wmax-vw5--focus wmax-vw5--disabled wmax-vw5--enabled wmax-vw6--active wmax-vw6--hover wmax-vw6--focus wmax-vw6--disabled wmax-vw6--enabled wmax-vw7--active wmax-vw7--hover wmax-vw7--focus wmax-vw7--disabled wmax-vw7--enabled wmax-vw8--active wmax-vw8--hover wmax-vw8--focus wmax-vw8--disabled wmax-vw8--enabled wmax-vw9--active wmax-vw9--hover wmax-vw9--focus wmax-vw9--disabled wmax-vw9--enabled wmax-vw10--active wmax-vw10--hover wmax-vw10--focus wmax-vw10--disabled wmax-vw10--enabled wmax-vw11--active wmax-vw11--hover wmax-vw11--focus wmax-vw11--disabled wmax-vw11--enabled wmax-vw12--active wmax-vw12--hover wmax-vw12--focus wmax-vw12--disabled wmax-vw12--enabled wmax-p0--active wmax-p0--hover wmax-p0--focus wmax-p0--disabled wmax-p0--enabled wmax-p1--active wmax-p1--hover wmax-p1--focus wmax-p1--disabled wmax-p1--enabled wmax-p2--active wmax-p2--hover wmax-p2--focus wmax-p2--disabled wmax-p2--enabled wmax-p3--active wmax-p3--hover wmax-p3--focus wmax-p3--disabled wmax-p3--enabled wmax-p4--active wmax-p4--hover wmax-p4--focus wmax-p4--disabled wmax-p4--enabled wmax-p5--active wmax-p5--hover wmax-p5--focus wmax-p5--disabled wmax-p5--enabled wmax-p6--active wmax-p6--hover wmax-p6--focus wmax-p6--disabled wmax-p6--enabled wmax-p7--active wmax-p7--hover wmax-p7--focus wmax-p7--disabled wmax-p7--enabled wmax-p8--active wmax-p8--hover wmax-p8--focus wmax-p8--disabled wmax-p8--enabled wmax-p9--active wmax-p9--hover wmax-p9--focus wmax-p9--disabled wmax-p9--enabled wmax-p10--active wmax-p10--hover wmax-p10--focus wmax-p10--disabled wmax-p10--enabled wmax-p11--active wmax-p11--hover wmax-p11--focus wmax-p11--disabled wmax-p11--enabled wmax-p12--active wmax-p12--hover wmax-p12--focus wmax-p12--disabled wmax-p12--enabled`
