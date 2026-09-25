# Changelog

## 1.0.0 (2026-09-25)


### Features

* **dependencies:** [#1913](https://github.com/HypeMC/NelmioApiDocBundle/issues/1913) - upgrade symfony 60 dependencies ([2ade72d](https://github.com/HypeMC/NelmioApiDocBundle/commit/2ade72d0aae64d94024745802a9fb85cf498d6c8))
* pass serialization context to name converter ([#2167](https://github.com/HypeMC/NelmioApiDocBundle/issues/2167)) ([c55d9ef](https://github.com/HypeMC/NelmioApiDocBundle/commit/c55d9ef7852fcfe8c1b1263ea33990de6a54de7a))
* sort processors by priority ([#2196](https://github.com/HypeMC/NelmioApiDocBundle/issues/2196)) ([c16f6fe](https://github.com/HypeMC/NelmioApiDocBundle/commit/c16f6fe0f897548ae64097201fec93f407b4d2b3))
* symfony 7 support ([#2164](https://github.com/HypeMC/NelmioApiDocBundle/issues/2164)) ([634a16b](https://github.com/HypeMC/NelmioApiDocBundle/commit/634a16b0482492419c086a9f176e1344a5f93bae))
* update swagger-ui ([#2154](https://github.com/HypeMC/NelmioApiDocBundle/issues/2154)) ([b7a5722](https://github.com/HypeMC/NelmioApiDocBundle/commit/b7a5722c4fbef6052dcc0aebe8b7b4c5b4ff49a0))


### Bug Fixes

* 1628 : annotations construction is context dependant ([#1632](https://github.com/HypeMC/NelmioApiDocBundle/issues/1632)) ([fb84e36](https://github.com/HypeMC/NelmioApiDocBundle/commit/fb84e36fdfb033506b7bde928f8063294e5fe2b5))
* 1885 update psr/log and psr/container ([#1892](https://github.com/HypeMC/NelmioApiDocBundle/issues/1892)) ([35cf37e](https://github.com/HypeMC/NelmioApiDocBundle/commit/35cf37e338ae0bc5797f186b7c41efda32ed5806))
* attribute validation groups not passed ([#2189](https://github.com/HypeMC/NelmioApiDocBundle/issues/2189)) ([2360674](https://github.com/HypeMC/NelmioApiDocBundle/commit/2360674a7bd8bbf5fb834b08e89662b6ad851618))
* describe nullable enums with allOf ([#2178](https://github.com/HypeMC/NelmioApiDocBundle/issues/2178)) ([23d157c](https://github.com/HypeMC/NelmioApiDocBundle/commit/23d157c02c505e4592ca134b91d22ab35584458c))
* different context uses same model ([#2183](https://github.com/HypeMC/NelmioApiDocBundle/issues/2183)) ([31da761](https://github.com/HypeMC/NelmioApiDocBundle/commit/31da761b6c9d275fb3bbee87c4c6888b17aec4ad))
* do not escape slashes ([#2157](https://github.com/HypeMC/NelmioApiDocBundle/issues/2157)) ([83e7fdd](https://github.com/HypeMC/NelmioApiDocBundle/commit/83e7fdde88181331d5c5795f7b74976b972be981))
* invalid nullable enums with OAS 3.1 version ([f98641d](https://github.com/HypeMC/NelmioApiDocBundle/commit/f98641dee93295fbc55c9799699a34254fde0fac))
* move to processor to correct dir ([#2204](https://github.com/HypeMC/NelmioApiDocBundle/issues/2204)) ([008ae69](https://github.com/HypeMC/NelmioApiDocBundle/commit/008ae69267fbf1cc5b25fbf29597790b42a03c45))
* pass configured openapi version to swagger-php ([#2159](https://github.com/HypeMC/NelmioApiDocBundle/issues/2159)) ([b415300](https://github.com/HypeMC/NelmioApiDocBundle/commit/b4153009220948da67af9b25e4fc04abf3765942))
* use iterable check instead of array ([#2239](https://github.com/HypeMC/NelmioApiDocBundle/issues/2239)) ([a15b592](https://github.com/HypeMC/NelmioApiDocBundle/commit/a15b5923602c669007ea53a1a87991e9e147daab))
* use oneOf instead of allOf ([#2156](https://github.com/HypeMC/NelmioApiDocBundle/issues/2156)) ([d8a9b66](https://github.com/HypeMC/NelmioApiDocBundle/commit/d8a9b662612595bb5ae14d07fab39203fe5696a2))

CHANGELOG
=========

4.24.0
-----
* Added support for some integer ranges (https://phpstan.org/writing-php-code/phpdoc-types#integer-ranges).  
  Annotations attached to integer properties like:
  ```php
    /**
     * @var int<6, 11>
     * @var int<min, 11>
     * @var int<6, max>
     * @var positive-int
     * @var negative-int
     */
  ```
  will be interpreted as appropriate `minimum` and `maximum` properties in the generated OpenAPI specification.

### Minor breaking change
Dropped support for PHP 7.2 and PHP 7.3. PHP 7.4 is the minimum required version now.

4.23.0
-----
* Cache configuration option `nelmio_api_doc.cache.item_id` now automatically gets the area appended.
  ```yml
  nelmio_api_doc:
      cache:
          pool: app.cache
          item_id: nelmio_api_doc.docs
      areas:
          default: 
              ...
          area1:   
              ...
  ```
  Result in cache keys: `nelmio_api_doc.docs.default` & `nelmio_api_doc.docs.area1` to be used respectively.
* Added cache configuration option per area.
  ```yml
  nelmio_api_doc:
      areas:
          default: # Manual cache configuration
              cache:
                  pool: app.cache
                  item_id: nelmio_api_doc.docs.default
              ...
          area1:   
              cache:
                  pool: app.cache
                  item_id: nelmio_api_doc.docs.area1
              ...
  ```
  Non-configured options will be inherited from `nelmio_api_doc.cache`.
* Fixed vendor extensions (`x-*`) from configuration not being outputted in the generated specification.
  ```yml
  nelmio_api_doc:
      documentation:
          info:
              title: 'My API'
              description: 'My API description'
              x-foo: 'bar'
  ```
  Now results in JSON specification:
  ```json
  {
    ...
    "info": {
      "title": "API",
      "version": "1.0",
      "x-foo": "bar"
    },
    ...
  }
  ```
* Updated nullable enum handling to align with the behaviour of other object types. It now uses wraps nullable enums with `oneOf` instead of `allOf`.

4.22.0
-----
* Updated bundle directory structure to recommended file structure as described in https://symfony.com/doc/7.0/bundles/best_practices.html.

  It might be necessary to reinstall the assets:
  ```bash
    bin/console assets:install
  ```

### Breaking change
If your codebase mentions a file or directory by path then an update to this path is necessary. For example to following configuration:
```yaml
doc-api:
    resource: "@NelmioApiDocBundle/Resources/config/routing/swaggerui.xml"
    prefix: /api/doc
```
Becomes:
```yaml
doc-api:
    resource: "@NelmioApiDocBundle/config/routing/swaggerui.xml"
    prefix: /api/doc
```

4.21.0
-----
* Added bundle configuration options `nelmio_api_doc.cache.pool` and `nelmio_api_doc.cache.item_id`.
  ```yml
  nelmio_api_doc:
      cache:
          pool: app.cache
          item_id: nelmio_api_doc.docs
  ```
  
4.20.0
-----
* Added Redocly as an alternative to Swagger UI. https://github.com/Redocly/redoc.
* Added support for describing dictionary types in OpenAPI 3.0.

4.0.0
-----
* Added support of OpenAPI 3.0. The internals were completely reworked and this version introduces BC breaks.

3.7.0
-----

* Added `@SerializedName` annotation support and name converters when using Symfony >= 4.2.
* Removed pattern added from the Expression Violation message.
* Added FOSRestBundle 3.x support
* Added `@SWG` annotations support at methods level in models

3.3.0
-----

* Usage of Google Fonts was removed. System fonts `serif` / `sans` will be used instead.
  This can lead to a different look on different operating systems.
  You can [re-add Google Fonts again manually by overriding the template](https://symfony.com/doc/current/bundles/NelmioApiDocBundle/faq.html#re-add-google-fonts).

* The Twig template for the Swagger UI now contains blocks to make it easier to overwrite certain parts.
  See the [official documentation](https://symfony.com/doc/current/bundles/NelmioApiDocBundle/customization.html) how to do this.

3.2.0 (2018-03-24)
------------------

* Add a documentation form extension. Use the ``documentation`` option to define how a form field is documented.
* Allow references to config definitions in controllers.
* Using `@Model` implicitely in `@SWG\Schema`, `@SWG\Items` and `@SWG\Property` is deprecated. Use `ref=@Model()` instead.

  Before:
  ```php
  /**
   * This was considered as an array of models.
   *
   * @SWG\Property(@Model(type=FooClass::class))
   */
  ```

  After:
  ```php
  /**
   * For an individual object:
   * @SWG\Property(ref=@Model(type=FooClass::class))
   *
   * For an array:
   * @SWG\Property(type="array", @SWG\Items(ref=@Model(type=FooClass::class)))
   */
  ```

Config
* `nelmio_api_doc.areas` added support to filter by host patterns.

  ```yml
  nelmio_api_doc:
      areas: [ host_patterns: [ ^api\. ] ]
  ```

* Added dependency for "symfony/options-resolver:^3.4.4|^4.0"

3.1.0 (2018-01-28)
------------------

* Added Symfony Validator constraints support

Symfony Forms
* Support for boolean checkbox
* Support for integer

JMS Serializer
* Support JMS `int` (alias for `integer`)
* Also process phpdoc annotations

SwaggerPHP
* Handle `enum` and `default` properties from SwaggerPHP annotation
* Support `@Security` annotations

Config
* `nelmio_api_doc.routes` has been replaced by `nelmio_api_doc.areas`. Please update your config accordingly.

  Before:
  ```yml
  nelmio_api_doc:
      routes: [ path_patterns: [ /api ] ]
  ```

  After:
  ```yml
  nelmio_api_doc:
      areas: [ path_patterns: [ /api ] ]
  ```

3.0.0 (2017-12-10)
------------------

Large refactoring introducing `zircote/swagger-php` for swagger annotations.

See UPGRADE-3.0.md for upgrading instructions.
