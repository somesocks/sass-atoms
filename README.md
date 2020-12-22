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

You can also use this to add all default atoms to a class, but this will result in a large output:

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