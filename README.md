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
    'bg-primary-200',
    'bg-primary-300',
    'bg-primary-400',
    'bg-primary-500',
    'bg-primary-600',
    'bg-primary-700',
    'bg-primary-800',
    'bg-primary-900',
    'bg-secondary-100',
    'bg-secondary-200',
    'bg-secondary-300',
    'bg-secondary-400',
    'bg-secondary-500',
    'bg-secondary-600',
    'bg-secondary-700',
    'bg-secondary-800',
    'bg-secondary-900',
  );

  $text-options: (
    'text-primary-100',
    'text-primary-200',
    'text-primary-300',
    'text-primary-400',
    'text-primary-500',
    'text-primary-600',
    'text-primary-700',
    'text-primary-800',
    'text-primary-900',
    'text-secondary-100',
    'text-secondary-200',
    'text-secondary-300',
    'text-secondary-400',
    'text-secondary-500',
    'text-secondary-600',
    'text-secondary-700',
    'text-secondary-800',
    'text-secondary-900',
  );

  // use the `materialize` mixin to generate real classes from placeholders
  // the second argument is a selector to prefix to the class, to add specificity
  @include materialize($background-options, '.banner');
  @include materialize($text-options, '.banner');
```

The resulting CSS looks like this:
```
.banner.bg-primary-100 {
  background-color: #E0E7FF;
}

.banner.bg-primary-200 {
  background-color: #C7D2FE;
}

.banner.bg-primary-300 {
  background-color: #A5B4FC;
}

.banner.bg-primary-400 {
  background-color: #818CF8;
}

.banner.bg-primary-500 {
  background-color: #6366F1;
}

.banner.bg-primary-600 {
  background-color: #4F46E5;
}

.banner.bg-primary-700 {
  background-color: #4338CA;
}

.banner.bg-primary-800 {
  background-color: #3730A3;
}

.banner.bg-primary-900 {
  background-color: #312E81;
}

.banner.bg-secondary-100 {
  background-color: #DBEAFE;
}

.banner.bg-secondary-200 {
  background-color: #BFDBFE;
}

.banner.bg-secondary-300 {
  background-color: #93C5FD;
}

.banner.bg-secondary-400 {
  background-color: #60A5FA;
}

.banner.bg-secondary-500 {
  background-color: #3B82F6;
}

.banner.bg-secondary-600 {
  background-color: #2563EB;
}

.banner.bg-secondary-700 {
  background-color: #1D4ED8;
}

.banner.bg-secondary-800 {
  background-color: #1E40AF;
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

.banner.text-primary-100 {
  color: #E0E7FF;
}

.banner.text-primary-200 {
  color: #C7D2FE;
}

.banner.text-primary-300 {
  color: #A5B4FC;
}

.banner.text-primary-400 {
  color: #818CF8;
}

.banner.text-primary-500 {
  color: #6366F1;
}

.banner.text-primary-600 {
  color: #4F46E5;
}

.banner.text-primary-700 {
  color: #4338CA;
}

.banner.text-primary-800 {
  color: #3730A3;
}

.banner.text-primary-900 {
  color: #312E81;
}

.banner.text-secondary-100 {
  color: #DBEAFE;
}

.banner.text-secondary-200 {
  color: #BFDBFE;
}

.banner.text-secondary-300 {
  color: #93C5FD;
}

.banner.text-secondary-400 {
  color: #60A5FA;
}

.banner.text-secondary-500 {
  color: #3B82F6;
}

.banner.text-secondary-600 {
  color: #2563EB;
}

.banner.text-secondary-700 {
  color: #1D4ED8;
}

.banner.text-secondary-800 {
  color: #1E40AF;
}

.banner.text-secondary-900 {
  color: #1E3A8A;
}

.banner {
  width: 100%;
}
```
