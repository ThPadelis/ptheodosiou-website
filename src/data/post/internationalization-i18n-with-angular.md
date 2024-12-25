---
publishDate: 2019-11-07T09:30:00Z
# author: Pantelis Theodosiou
title: Internationalization ( i18n ) with Angular
excerpt: Learn how to implement internationalization (i18n) in Angular applications, making your app usable in different languages and locales. This guide covers the process of designing, preparing, and translating your app for global audiences.
image: https://images.unsplash.com/photo-1516996087931-5ae405802f9f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80
# category: Tutorials
tags:
  - angular
  - internationalization
  - web-development
# metadata:
#   canonical: https://astrowind.vercel.app/get-started-website-with-astro-tailwind-css
---

>  *Internationalization* is the process of designing and preparing your app to be usable in different languages. *Localization* is the process of translating your internationalized app into specific languages for particular locales.

Therefore, just as you’d ensure that your design is aesthetically-pleasing and accessible, you should also ensure that your text is localized. This is the process of ensuring that your applications support multiple languages. In Angular, this is done in multiple ways.

For example, you can use third-party libraries, such us [ngx-translate](https://github.com/ngx-translate/core) or you can just use the built-in i18n functionality.

## Why is Internationalization so important?

These days, with so many websites to choose from, making applications available and user-friendly to a worldwide audience is important. It's the move to make a better user experience.

In this article, we will be focused on using the built-in i18n tool. So let's get started.

## New Angular Project

```bash
# Install Angular CLI globally
npm install --global @angular/cli

# Create Angular project
ng new ng-internationalization

# Would you like to add Angular routing? N
# Which stylesheet format would you like to use? SCSS
# Go to project's directory
cd ng-internationalization

# Open this folder with your favourite editor
code . 

# Serve the application
ng serve -o
```

## Translations

Angular's default locale is set to `en-US`. To support more languages, we need to update the default configuration and add additional locales. You can find a list of various locale codes [here](https://github.com/ladjs/i18n-locales).

## Project Templates

To be able to set the translations up, we need to add some basic content on our application. Head over to `app.component.html` and add some content.

```html
<main>
  <section>
    <h1>Why is Internationalization so important?</h1>
    <p>
      These days, with so many websites to choose from, making applications
      available and user-friendly to a worldwide audience is important. It's the
      move to make a better user experience.
      <small
        >Find the full article
        <a
          href="https://dev.to/thpadelis/internationalization-i18n-with-angular-4ao7"
          target="_blank"
          >here</a
        ></small
      >
    </p>
  </section>
</main>
```

We can add some styles to make this site a bit more user-friendly, inside of `app.component.scss`

```scss
@import url("https://fonts.googleapis.com/css?family=Roboto&display=swap&subset=greek");

main {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: #6441a5;
  background: -webkit-linear-gradient(to right, #6441a5, #2a0845);
  background: linear-gradient(to right, #6441a5, #2a0845);

  section {
    font-family: "Roboto", sans-serif;
    text-align: center;
    color: #cec6d4;
    border: solid thin #cec6d4;
    padding: 40px;
    width: 50%;
    -webkit-box-shadow: 15px 15px 5px 0px rgba(0, 0, 0, 0.75);
    -moz-box-shadow: 15px 15px 5px 0px rgba(0, 0, 0, 0.75);
    box-shadow: 15px 15px 5px 0px rgba(0, 0, 0, 0.75);

    a {
      color: inherit;
    }

    small {
      display: block;
      margin-top: 10px;
    }
  }
}
```

And finally, inside of `styles.scss` let's set the body styles

```scss
body {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}
```

This is what we should have by this time

![Screenshot_1](https://imgur.com/KS92bWt.png)

## Text marking

To be able to translate the context in the application, we first need to mark the text with a custom attribute named `i18n`. After the text marking, we will be able to translate the application into our desire languages. In our case `el-GR` and `fr-FR`. I will be using Google Translate to translate the text into French.

Let's add the `i18n` attribute to all of the text that we want to translate

```html
<main>
  <section>
    <h1 i18n>Why is Internationalization so important?</h1>
    <p i18n>
      These days, with so many websites to choose from, making applications
      available and user-friendly to a worldwide audience is important. It's the
      move to make a better user experience.
    </p>
    <small i18n
      >Find the full article
      <a
        href="https://github.com/ThPadelis/ng-internationalization"
        target="_blank"
        >here</a
      ></small
    >
  </section>
</main>
```

>  `i18n` is a custom attribute, recognized by Angular tools and compilers. After translation, the compiler removes it. It is not an Angular directive. 

Now, we need a `script` that uses Angular CLI to extract this into a `messages.xlf` file. This file contains all of our marked items. Head over to `packages.json` and add this `script`

```json
"scripts": {
    "i18n:extract": "ng xi18n --output-path src/locales"
}
```

After adding this script, go ahead and run `npm run i18n:extract`. Then, open up `scr/locales/messages.xlf` and you will see something like this

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<xliff version="1.2" xmlns="urn:oasis:names:tc:xliff:document:1.2">
  <file source-language="en" datatype="plaintext" original="ng2.template">
    <body>
      <trans-unit id="4bcef995bebcf205074f9fd756b822a488b452cc" datatype="html">
        <source>Why is Internationalization so important?</source>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">3</context>
        </context-group>
      </trans-unit>
      <trans-unit id="e8db4c58a5fc95a2a8d80183e4b527f4480fa06e" datatype="html">
        <source>
      These days, with so many websites to choose from, making applications
      available and user-friendly to a worldwide audience is important. It&apos;s the
      move to make a better user experience.
    </source>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">4</context>
        </context-group>
      </trans-unit>
      <trans-unit id="0f16c7aaa76d2f52dbabcd329ebf11a39a26918d" datatype="html">
        <source>Find the full article
      <x id="START_LINK" ctype="x-a" equiv-text="&lt;a&gt;"/>here<x id="CLOSE_LINK" ctype="x-a" equiv-text="&lt;/a&gt;"/></source>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">10</context>
        </context-group>
      </trans-unit>
    </body>
  </file>
</xliff>
```

For each `html` element marked with the `i18n` directive, a `trans-unit` will be created.

```xml
<trans-unit id="4bcef995bebcf205074f9fd756b822a488b452cc" datatype="html">
    <source>Why is Internationalization so important?</source>
    <context-group purpose="location">
        <context context-type="sourcefile">src/app/app.component.html</context>
        <context context-type="linenumber">3</context>
    </context-group>
</trans-unit>
```

We can provide more information about the translations using this structure by adding a `description`. Inside of `app.component.hmtl`, update the `i18n` items with a description.

```html
<main>
  <section>
    <h1 i18n="Title for the article">
      Why is Internationalization so important?
    </h1>
    <p i18n="Description for the article">
      These days, with so many websites to choose from, making applications
      available and user-friendly to a worldwide audience is important. It's the
      move to make a better user experience.
    </p>
    <small i18n="Read the whole article"
      >Find the full article
      <a
        href="https://dev.to/thpadelis/internationalization-i18n-with-angular-4ao7"
        target="_blank"
        >here</a
      ></small
    >
  </section>
</main>
```

Additionally, we can help the translator with a `description` and a `meaning` by using this format

```html
<!-- i18n="<meaning>|<description>" -->
<main>
  <section>
    <h1 i18n="Article Heading|Title for the article">
      Why is Internationalization so important?
    </h1>
    <p i18n="Article Description|Description for the article">
      These days, with so many websites to choose from, making applications
      available and user-friendly to a worldwide audience is important. It's the
      move to make a better user experience.
    </p>
    <small i18n="Full Article|Read the whole article"
      >Find the full article
      <a
        href="https://dev.to/thpadelis/internationalization-i18n-with-angular-4ao7"
        target="_blank"
        >here</a
      ></small
    >
  </section>
</main>
```

We can also se a custom id for persistence and maintenance

```html
<!-- i18n="<meanin>|<description>@@customId" -->
<h1 i18n="Article Heading|Title for the article@@articleHeading">
    Why is Internationalization so important?
</h1>
```

We can finally generate our translations

```bash
npm run i18n:extract
```

Now that we have added `meaning`, `description` and `id` our `messages.xml` should look like this

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<xliff version="1.2" xmlns="urn:oasis:names:tc:xliff:document:1.2">
  <file source-language="en" datatype="plaintext" original="ng2.template">
    <body>
      <trans-unit id="articleHeading" datatype="html">
        <source>
      Why is Internationalization so important?
    </source>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">3</context>
        </context-group>
        <note priority="1" from="description">Title for the article</note>
        <note priority="1" from="meaning">Article Heading</note>
      </trans-unit>
      <trans-unit id="articleDescription" datatype="html">
        <source>
      These days, with so many websites to choose from, making applications
      available and user-friendly to a worldwide audience is important. It&apos;s the
      move to make a better user experience.
    </source>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">8</context>
        </context-group>
        <note priority="1" from="description">Description for the article</note>
        <note priority="1" from="meaning">Article Description</note>
      </trans-unit>
      <trans-unit id="fullArticle" datatype="html">
        <source>Find the full article
      <x id="START_LINK" ctype="x-a" equiv-text="&lt;a&gt;"/>here<x id="CLOSE_LINK" ctype="x-a" equiv-text="&lt;/a&gt;"/></source>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">14</context>
        </context-group>
        <note priority="1" from="description">Read the whole article</note>
        <note priority="1" from="meaning">Full Article</note>
      </trans-unit>
    </body>
  </file>
</xliff>
```

## Translations

At this point, we have a `messages.xlf` file that contains all of the items that we want to translate. Let's create a copy `messages.xlf` for each language we want to translate the application.

```sh
cp src\locales\messages.xlf src\locales\messages.fr.xlf
cp src\locales\messages.xlf src\locales\messages.el.xlf
```

### Greek

Let's start with `messages.el.xlf`. We will translate the messages by using the `target` and `source`. The `target` attribute is the the translation in that language.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<xliff version="1.2" xmlns="urn:oasis:names:tc:xliff:document:1.2">
  <file source-language="en" datatype="plaintext" original="ng2.template">
    <body>
      <trans-unit id="articleHeading" datatype="html">
        <source>Why is Internationalization so important?</source>
        <target>Γιατί είναι τόσο σημαντική η διεθνοποίηση;</target>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">3</context>
        </context-group>
        <note priority="1" from="description">Title for the article</note>
        <note priority="1" from="meaning">Article Heading</note>
      </trans-unit>

      <trans-unit id="articleDescription" datatype="html">
        <source>These days, with so many websites to choose from, making applications available and user-friendly to a worldwide audience is important. It&apos;s the move to make a better user experience.</source>
        <target>Αυτές τις μέρες, ανάμεσα σε τόσους ιστότοπους για να διαλέξεις, κάνοντας εφαρμογές διαθέσιμες και φιλικές προς τον χρήστη είναι σημαντικό. Είναι μία κίνηση για καλύτερη εμπειρία χρήστη.</target>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">8</context>
        </context-group>
        <note priority="1" from="description">Description for the article</note>
        <note priority="1" from="meaning">Article Description</note>
      </trans-unit>

      <trans-unit id="fullArticle" datatype="html">
        <source>Find the full article <x id="START_LINK" ctype="x-a" equiv-text="&lt;a&gt;"/>here<x id="CLOSE_LINK" ctype="x-a" equiv-text="&lt;/a&gt;"/></source>
        <target>Βρείτε ολόκληρο το άρθρο <x id="START_LINK" ctype="x-a" equiv-text="&lt;a&gt;"/>εδώ<x id="CLOSE_LINK" ctype="x-a" equiv-text="&lt;/a&gt;"/></target>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">14</context>
        </context-group>
        <note priority="1" from="description">Read the whole article</note>
        <note priority="1" from="meaning">Full Article</note>
      </trans-unit>
    </body>
  </file>
</xliff>
```

### French

Now, let's do the same for `messages.fr.xlf` in French

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<xliff version="1.2" xmlns="urn:oasis:names:tc:xliff:document:1.2">
  <file source-language="en" datatype="plaintext" original="ng2.template">
    <body>
      <trans-unit id="articleHeading" datatype="html">
        <source>Why is Internationalization so important?</source>
        <target>Pourquoi l'internationalisation est-elle si importante?</target>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">3</context>
        </context-group>
        <note priority="1" from="description">Title for the article</note>
        <note priority="1" from="meaning">Article Heading</note>
      </trans-unit>

      <trans-unit id="articleDescription" datatype="html">
        <source>These days, with so many websites to choose from, making applications available and user-friendly to a worldwide audience is important. It&apos;s the move to make a better user experience.</source>
        <target>Ces jours-ci, avec autant de sites Web à choisir, il est important de rendre les applications disponibles et conviviales pour un public mondial. C'est le geste d'améliorer l'expérience utilisateur.</target>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">8</context>
        </context-group>
        <note priority="1" from="description">Description for the article</note>
        <note priority="1" from="meaning">Article Description</note>
      </trans-unit>

      <trans-unit id="fullArticle" datatype="html">
        <source>Find the full article <x id="START_LINK" ctype="x-a" equiv-text="&lt;a&gt;"/>here<x id="CLOSE_LINK" ctype="x-a" equiv-text="&lt;/a&gt;"/></source>
        <target>Retrouvez l'article complet <x id="START_LINK" ctype="x-a" equiv-text="&lt;a&gt;"/>ici<x id="CLOSE_LINK" ctype="x-a" equiv-text="&lt;/a&gt;"/></target>
        <context-group purpose="location">
          <context context-type="sourcefile">src/app/app.component.html</context>
          <context context-type="linenumber">14</context>
        </context-group>
        <note priority="1" from="description">Read the whole article</note>
        <note priority="1" from="meaning">Full Article</note>
      </trans-unit>
    </body>
  </file>
</xliff>
```

> *NOTE*: These are not correct translations for French.  Be kind to me.

## Locale build configuration

Perfect! Now, we have three versions of our application. 

A good practice is to have a `script` that builds the application for each locale we want to support.

Head over to `angular.json` and add the below configurations.

```json
{
  "projects": {
    "ng-internationalization": {
      "architect": {
        "build": {
          "configurations": {
            "fr-FR": {
              "aot": true,
              "outputPath": "dist/fr-FR",
              "i18nFile": "src/locales/messages.fr.xlf",
              "i18nFormat": "xlf",
              "i18nLocale": "fr",
              "i18nMissingTranslation": "error"
            },
            "el-GR": {
              "aot": true,
              "outputPath": "dist/el-GR",
              "i18nFile": "src/locales/messages.el.xlf",
              "i18nFormat": "xlf",
              "i18nLocale": "el-GR",
              "i18nMissingTranslation": "error"
            }
          }
        },
        "serve": {
          "configurations": {
            "production": {
              "browserTarget": "ng-internationalization:build:production"
            },
            "fr-FR": {
              "browserTarget": "ng-internationalization:build:fr-FR"
            },
            "el-GR": {
              "browserTarget": "ng-internationalization:build:el-GR"
            }
          }
        }
      }
    }
  }
}
```

> *NOTE*: I've omitted most of the file to keep it brief.

Let's create some more scripts inside the `package.json` file to be able to `build` and `serve` our new locales.

```json
"scripts": {
    "start": "ng serve",
    "start:fr": "ng serve --configuration=fr-FR",
    "start:gr": "ng serve --configuration=el-GR",
    "build": "ng build",
    "build:fr": "ng build --configuration=fr-FR",
    "build:gr": "ng build --configuration=el-GR",
}
```

> *NOTE*: Once again, I kept the file brief.

It's time to see what we have created.

```bash
npm run start
npm run start:fr -- --port 4201
npm run start:gr -- --port 4202
```

And there you go! | Et Voilà! | Μπουμ!

![i18n](https://imgur.com/47iFTqv.jpg)

## Conclusion

Internationalization and localization are an important part of an application. We can use angular-cli to support it or install and external library to save some time.

## References / Resources

1. [i18n](https://angular.io/guide/i18n)
2. [ngx-translate](https://github.com/ngx-translate/core)