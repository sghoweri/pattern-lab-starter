[![Build Status](https://travis-ci.org/phase2/pattern-lab-starter.svg?branch=master)](https://travis-ci.org/phase2/pattern-lab-starter)

# QuickStart

```bash
npm install
npm start
```

If you're using Drupal 8, get the [Component Libraries module](https://www.drupal.org/project/components):

```bash
drush dl components
drush en components -y
```

That's it.

## Commands

Compile everything:

```bash
npm run compile
```

Start up watches and local server after compiling:

```bash
npm run start # or `npm start`
```

Run Tests:

```bash
npm run test # or `npm test`
```

Create a new component folder in Pattern Lab with scss, twig, md, & yml/json by running:

```bash
npm run new
```

## Orientation

- source/
  - _annotations/ ([annotations](http://patternlab.io/docs/pattern-adding-annotations.html) for Patterns)
  - _data/ (Global JSON data files available to all Patterns, can add multiple)
  - _patterns/ (Twig, Scss, and JS all in here)
    - 00-base/ (Twig Namespace: `@base`)
      - Contains what all that follows needs: variables, mixins, and grid layouts for examples
    - 01-atoms/ (Twig Namespace: `@atoms`)
    - 02-molecules (Twig Namespace: `@molecules`)
    - 03-organisms (Twig Namespace: `@organisms`)
    - 04-templates (Twig Namespace: `@templates`)
    - 05-pages (Twig Namespace: `@pages`)
  - _meta/ (contains the header and footer Twig templates for PL; add any `<link>` or `<script>` tags here; don't edit in between the `<!-- inject -->` tags though; it'll get overwritten)
- pattern-lab/
  - config/config.yml (Pattern Lab configuration)
  - public/ (Where Pattern Lab compiles too, it's just static HTML)
  - composer.json (run `composer update` next to this to update dependencies)
- scss/ - Sass files that aren't really tied to a component, so not in the above location.
- js/ - all js files here and transpiled by Babel and combined into a single `dest/script.js` file.
- images/icons/src/ - all SVGs here are combined into font icons and have classes and Sass mixins made for each based on file name. See `atoms/images/icons` in Pattern Lab.
- dest/ - Many compiled assets end up in here like CSS, JS, Font Icons, and any doc systems like [SassDoc](http://sassdoc.com).
- templates/ - Drupal twig templates. These often will `include`, `embed`, or `extend` the Twig templates found in Pattern Lab like this: `{% include "@molecules/branding/branding.twig" with { url: path('<front>') } %}`. We keep the components in Pattern Lab "pure" and ignorant of Drupal's data model and use these templates to map the data between the two. Think of these as the Presenter templates in the [Model View Presenter](https://en.wikipedia.org/wiki/Model–view–presenter) approach. Also, Drupal Twig templates that have nothing to do with Pattern Lab go here.
- gulpconfig.yml - Configuration for all the gulp tasks, a ton can be controlled here.

---

# Details

## Pattern Lab

Refer to the [Pattern Lab Documentation](http://patternlab.io/docs) for extensive info on how to use it. This theme starter is a custom Pattern Lab 2 *Edition* that is heavily influenced by the [Drupal Edition of Pattern Lab](https://github.com/pattern-lab/edition-php-drupal-standard) and uses the Twig engine to bring it inline with Drupal 8's use of Twig.

### Folder Structure Differences

Our folder structure makes a slight but convenient alteration to the typical Pattern Lab folder setup. Basically we move `pattern-lab/source/` up one level because we keep Sass in there too and it's the "source" for much of the theme. Here's the difference between the typical and our structure (few folders mentioned for brevity; please see Orientation above for a more thorough list).

#### Typical Folder Structure

- pattern-lab/
  - config/
  - public/
  - source/
    - _patterns/ (contains atoms, molecules, etc folders)
  - composer.json

#### Our Folder Structure

- source/
  - _patterns/ (contains atoms, molecules, etc folders)
- pattern-lab/
  - config/
  - public/
  - composer.json

### Dummy data using `Faker`

[`Faker`](https://github.com/fzaninotto/Faker) generates fake data and the [Faker plugin for Pattern Lab](https://github.com/pattern-lab/plugin-php-faker) is used here. This generates *different* fake content for each compile, and allows [translated content](https://github.com/pattern-lab/plugin-php-faker#locales) as well.

**Faker only works in the global data files** found in `source/_data/` currently until [this bug](https://github.com/pattern-lab/plugin-php-faker/issues/2) is fixed.

Use it like this in `source/_data/data.json`:

```json
{
  "description": "Faker.paragraph",
  "text": "Faker.words(3, true)",
  "byline": "Faker.sentence",
  "intro": "Faker.sentences(2, true)"
}
```

The formatters (things like `.paragraph`, `.words`, etc) can accept options, when you see `Faker.words(3, true)` that means give me 3 random words and I'd like them as a string and not an array of strings. All the [formatters and their options are documented here](https://github.com/fzaninotto/Faker#formatters) - there's tons: numbers, address, names, dates, and more.

## Configuration

It's almost all done in `gulpconfig.yml`. End all paths with a `/` please (i.e. `path/to/dir/`). The local `gulpfile.js` passes the `config` object to [`p2-theme-core`](https://github.com/phase2/p2-theme-core) - which can be viewed at `node_modules/p2-theme-core/` (most stuff in `lib/`).

Many of the features can be turned off, for example if we didn't want all the JS features like linting and concatenation, just toggle `enabled` under `js` in `gulpconfig.yml`. So you'd just open `gulpconfig.yml` and change this:

```diff
js:
-    enabled: true
+    enabled: false
```

Documentation for many of the features are found in `node_modules/p2-theme-core/docs/` – those are [hosted here](http://p2-theme-core.readthedocs.org) too.

### Linting Config

- JS: edit `.eslintrc` - [rule docs](http://eslint.org/docs/rules/)
- Scss: edit `.sass-lint.yml` - [rule docs](https://github.com/sasstools/sass-lint/tree/master/docs/rules)

### Babel JS Transpiling Config

Edit `.babelrc` for configuration of [Babel rules](https://babeljs.io/docs/usage/options/) that transpile JS. Default allows ES6 to be transpiled to ES5. Learn about awesome [ES6 features](http://es6-features.org) here.

## Assets

Get front end libraries injected into Drupal theme info file and Pattern Lab with:

    bower install {project-name} --save

Using `--save` adds to Pattern Lab and Drupal; using `--save-dev` adds to just Pattern Lab. Automatic injection for Drupal currently only works in Drupal 7; manually add it in Drupal 8.

## Gulp

The `npm run` commands above basically trigger gulp without having to install a global dependency. For fine grained control of tasks, install gulp globally with `npm install --global gulp` and then run `gulp help` for a list of all available tasks.

Add anything to `gulpfile.js` that you want! Also, you can copy any file from `node_modules/p2-theme-core/lib/` into your repo and use it as a starting point (may need to install packages from `p2-theme-core` too.)

Many of the features can be turned off, for example if we didn't want all the JS features like linting and concatenation, just toggle `enabled` under `js` in `gulpconfig.yml`. So you'd just open `gulpconfig.yml` and change this:

```diff
js:
-    enabled: true
+    enabled: false
```

## Drupal 8 Integration

From your Drupal Twig templates in `templates/` you can `{% include %}`, `{% extends %}`, and `{% embed %}` your Pattern Lab Twig template files. Each of the top level folders has a Twig Namespace like `@organisms` and then you append the path down to the file like below.

```twig
{% include "@organisms/path/to/file.twig" with {
  title: label,
  largeCTA: true
} %}
```

For a demonstration in a sample codebase of how exactly to integrate templates, see the [`drupal-lab`](https://github.com/phase2/drupal-lab) repo; in particular note how both a [node teaser template](https://github.com/phase2/drupal-lab/blob/master/web/themes/dashing/templates/content/node--article--teaser.html.twig) and a [views field template](https://github.com/phase2/drupal-lab/blob/master/web/themes/dashing/templates/views/views-view-fields--newspage--page.html.twig) in the Drupal `templates/` folder can embed the [card template](https://github.com/phase2/drupal-lab/blob/master/web/themes/dashing/pattern-lab/source/_patterns/02-molecules/cards/card.twig) from Pattern Lab while formatting the data.
