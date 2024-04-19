<div style="text-align: center;">
 <a href="https://newfold.com/" target="_blank">
  <img src="https://newfold.com/content/experience-fragments/newfold/site-header/master/_jcr_content/root/header/logo.coreimg.svg/1621395071423/newfold-digital.svg" alt="Newfold Logo" title="Newfold Digital" height="42" />
 </a>
</div>

# WordPress Fields Module

This module manages field type definitions and the rendering process of fields within an application. It dynamically renders fields according to specified parameters, including the field type, size, validation rules, and display properties. The module ensures that each field is rendered appropriately based on its configuration.

## Module Responsibilities
- Provide a framework for defining and rendering fields.
- Handle JS interactions both in and out of React. As such, we should use Alpine.js.

## Critical Paths
- A field can be rendered and will call a JS callback function.

## API

### PHP Functions
- getField($fieldType, $args) - returns a field instance from the registry on which methods can be called
- renderField($fieldType, $args) - renders a field instance from the registry

## Field PHP Class
- A base class called `NewfoldLabs\WP\Fields\Field` that can be extended.
- The field type name will be extracted from the field class automatically. A field class might be named something like `EmailField`, `SelectField`, `MultiSelectField`, etc. The field type in the registry would become `email` `select`, `multiSelect`.
- The methods on the base class are:
  - __construct($args)
    - Value is the current value (required)
    - On change JS callback (required)
    - Label
    - Description
    - Type is the field type (optional, for inputs)
    - Name is the field name (optional)
    - Other args might be: placeholder, disabled, min, max, size, pattern, checked, required, options, etc.
  - render
    - Grabs the appropriate Alpine.js template from the `views` folder
    - Injects the args into the template
    - Add Cypress test IDs as appropriate - can be automatically set or defined by passing in an argument to the class
    - Outputs the result

## Notes
- This module is a Composer package and should not use the Newfold module loader.
- A `NewfoldLabs\WP\Fields\Registry` class should have the following methods: has, get, set, remove, keys, reset, all
  - This would be a collection of field types. A field class should be able to be fetched by name and then instantiated.
