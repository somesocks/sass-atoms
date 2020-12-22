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
### `$align-content-atoms--breakpoints`
### `$align-content-atoms--states`

### `$align-items-atoms`
### `$align-items-atoms--breakpoints`
### `$align-items-atoms--states`

### `$align-self-atoms`
### `$align-self-atoms--breakpoints`
### `$align-self-atoms--states`

### `$background-clip-atoms`
### `$background-clip-atoms--breakpoints`
### `$background-clip-atoms--states`

### `$background-color-atoms`
### `$background-color-atoms--breakpoints`
### `$background-color-atoms--states`

### `$background-position-atoms`
### `$background-position-atoms--breakpoints`
### `$background-position-atoms--states`

### `$background-repeat-atoms`
### `$background-repeat-atoms--breakpoints`
### `$background-repeat-atoms--states`

### `$background-size-atoms`
### `$background-size-atoms--breakpoints`
### `$background-size-atoms--states`

### `$border-color-atoms`
### `$border-color-atoms--breakpoints`
### `$border-color-atoms--states`

### `$border-radius-atoms`
### `$border-radius-atoms--breakpoints`
### `$border-radius-atoms--states`

### `$border-style-atoms`
### `$border-style-atoms--breakpoints`
### `$border-style-atoms--states`

### `$border-width-atoms`
### `$border-width-atoms--breakpoints`
### `$border-width-atoms--states`

### `$box-sizing-atoms`
### `$box-sizing-atoms--breakpoints`
### `$box-sizing-atoms--states`

### `$clear-atoms`
### `$clear-atoms--breakpoints`
### `$clear-atoms--states`

### `$display-atoms`
### `$display-atoms--breakpoints`
### `$display-atoms--states`

### `$flex-direction-atoms`
### `$flex-direction-atoms--breakpoints`
### `$flex-direction-atoms--states`

### `$flex-grow-atoms`
### `$flex-grow-atoms--breakpoints`
### `$flex-grow-atoms--states`

### `$flex-shrink-atoms`
### `$flex-shrink-atoms--breakpoints`
### `$flex-shrink-atoms--states`

### `$flex-wrap-atoms`
### `$flex-wrap-atoms--breakpoints`
### `$flex-wrap-atoms--states`

### `$float-atoms`
### `$float-atoms--breakpoints`
### `$float-atoms--states`

### `$font-family-atoms`
### `$font-family-atoms--breakpoints`
### `$font-family-atoms--states`

### `$font-weight-atoms`
### `$font-weight-atoms--breakpoints`
### `$font-weight-atoms--states`

### `$height-atoms`
### `$height-atoms--breakpoints`
### `$height-atoms--states`

### `$min-height-atoms`
### `$min-height-atoms--breakpoints`
### `$min-height-atoms--states`

### `$max-height-atoms`
### `$max-height-atoms--breakpoints`
### `$max-height-atoms--states`

### `$justify-content-atoms`
### `$justify-content-atoms--breakpoints`
### `$justify-content-atoms--states`

### `$justify-items-atoms`
### `$justify-items-atoms--breakpoints`
### `$justify-items-atoms`

### `$justify-self-atoms`
### `$justify-self-atoms--breakpoints`
### `$justify-self-atoms--states`

### `$margin-atoms`
### `$margin-atoms--breakpoints`
### `$margin-atoms--states`

### `$margin-x-atoms`
### `$margin-x-atoms--breakpoints`
### `$margin-x-atoms--states`

### `$margin-y-atoms`
### `$margin-y-atoms--breakpoints`
### `$margin-y-atoms--states`

### `$margin-t-atoms`
### `$margin-t-atoms--breakpoints`
### `$margin-t-atoms--states`

### `$margin-r-atoms`
### `$margin-r-atoms--breakpoints`
### `$margin-r-atoms--states`

### `$margin-b-atoms`
### `$margin-b-atoms--breakpoints`
### `$margin-b-atoms--states`

### `$margin-l-atoms`
### `$margin-l-atoms--breakpoints`
### `$margin-l-atoms--states`

### `$object-fit-atoms`
### `$object-fit-atoms--breakpoints`
### `$object-fit-atoms--states`

### `$object-position-atoms`
### `$object-position-atoms--breakpoints`
### `$object-position-atoms--states`

### `$overflow-atoms`
### `$overflow-atoms--breakpoints`
### `$overflow-atoms--states`

### `$overflow-x-atoms`
### `$overflow-x-atoms--breakpoints`
### `$overflow-x-atoms--states`

### `$overflow-y-atoms`
### `$overflow-y-atoms--breakpoints`
### `$overflow-y-atoms--states`

### `$padding-atoms`
### `$padding-atoms--breakpoints`
### `$padding-atoms--states`

### `$padding-x-atoms`
### `$padding-x-atoms--breakpoints`
### `$padding-x-atoms--states`

### `$padding-y-atoms`
### `$padding-y-atoms--breakpoints`
### `$padding-y-atoms--states`

### `$padding-t-atoms`
### `$padding-t-atoms--breakpoints`
### `$padding-t-atoms--states`

### `$padding-r-atoms`
### `$padding-r-atoms--breakpoints`
### `$padding-r-atoms--states`

### `$padding-b-atoms`
### `$padding-b-atoms--breakpoints`
### `$padding-b-atoms--states`

### `$padding-l-atoms`
### `$padding-l-atoms--breakpoints`
### `$padding-l-atoms--states`

### `$position-atoms`
### `$position-atoms--breakpoints`
### `$position-atoms--states`

### `$text-align-atoms`
### `$text-align-atoms--breakpoints`
### `$text-align-atoms--states`

### `$text-color-atoms`
### `$text-color-atoms--breakpoints`
### `$text-color-atoms--states`

### `$text-size-atoms`
### `$text-size-atoms--breakpoints`
### `$text-size-atoms--states`

### `$visibility-atoms`
### `$visibility-atoms--breakpoints`
### `$visibility-atoms--states`

### `$width-atoms`
### `$width-atoms--breakpoints`
### `$width-atoms--states`

### `$min-width-atoms`
### `$min-width-atoms--breakpoints`
### `$min-width-atoms--states`

### `$max-width-atoms`
### `$max-width-atoms--breakpoints`
### `$max-width-atoms--states`
