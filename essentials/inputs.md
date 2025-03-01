---
title: Inputs
description: FormKit ships with turbocharged inputs for developing production-ready forms in your project.
---

# Inputs

:PageToc

## Introduction

FormKit Inputs are similar to HTML inputs but turbocharged with much needed features like labels, help text, validation, and error messages (and much more). Similar to how HTML’s `<input>` tag uses various `type` attributes (i.e., `<input type="text">` vs `<input type="checkbox">`), FormKit uses the `type` prop for _all_ inputs. In fact, with FormKit, **there is only 1 component you have to learn**:

::Example
---
  name: "Text input"
  file: "_content/_examples/single-component/single-component.vue"
---
::

FormKit Inputs are not confined to what is available in "native" HTML. Our seperate [FormKit Pro](/pro) package provides access to "synthetic" input types such as `repeater`, `autocomplete`, `mask`, `rating` and more. Of course, you can write your own inputs too by creating [custom inputs](/guides/create-a-custom-input).

::Example
---
  name: "Text input"
  file: [
    '/_content/_examples/single-component/single-component-pro.vue',
  ]
---
::

## The Form Input

While you’re free to use FormKit inputs by themselves, you’ll usually want to [group them into a form](/inputs/form):

```html
<FormKit type="form">
  <!-- ... your form inputs -->
</FormKit>
```

The [form type](/inputs/form) provides a host of features including value collection, initial value setting, form submission, error handling, loading states, and more.

## Setting values

<!-- vue-specific -->

There are 4 ways to set the value of an input:

- Using the `value` prop.
- Using `v-model`.
- Using FormKit's node `node.input()` method.
- Setting the value of a parent `FormKit` component.

### Using `value` prop

You can set the initial value of a single input or a group of inputs using the `value`
prop.

::Example
---
  name: "Value prop"
  file: "_content/_examples/value-prop/value-prop.vue"
---
::

::Callout
---
type: "warning"
---
The <code>value</code> prop should only be used for setting the <em>initial</em> value of an input. It will not react to changes after the component has been created.
::

### Using `v-model`

<!-- vue-specific -->

Using `v-model` allows for two-way reactive data binding with any FormKit input.

::Example
---
  name: "Input v-model"
  file: "_content/_examples/v-model/v-model.vue"
---
::

### Using `node.input()`

At the heart of every FormKit input is an instance of [FormKit’s `node`
object](/essentials/architecture#node), and using the `node.input()` method is the most efficient mechanism to modify any input’s value (read more about [getting an instance of the node object](/essentials/architecture#getting-a-components-node)).

::Example
---
  name: "Input v-model"
  file: "_content/_examples/node-input/node-input.vue"
---
::

::Callout
---
type: "tip"
---
Calls to <code>node.input()</code> are debounced, and thus asynchronous (use the <code>delay</code> prop to change the length of the debounce). You can <code>await node.input(val)</code> to determine when the input has settled.
::

### Using a parent

Parent inputs like `list`, `group`, and `form` are also able to directly set the values of any of their children. In fact, the value of a parent is just the aggregate value of its children. You can use any of the above methods (`value` prop, `v-model`, or `node.input()`) to set the value of the children.

::Example
---
  name: "Parent input"
  file: "_content/_examples/parent-input/parent-input.vue"
---
::

## Setting attributes

In nearly all cases, attributes set on the `<FormKit>` component will be passed through to the actual `<input>` element at the heart of the component, rather than any wrapping DOM elements. For example:

::Example
---
  name: "Text input"
  file: "_content/_examples/attributes/attributes.vue"
  tabs: "html"
---
::

## Validation

We discuss validation in more detail on its [own documentation page](/essentials/validation) — but suffice to say adding validation rules to inputs in FormKit is as easy as adding the `validation` prop:

::Example
---
  name: "Simple validation"
  file: "_content/_examples/simple-validation/simple-validation.vue"
---
::


::Cta
---
href: "/essentials/validation"
label: "Learn more about validation rules"
button: "Read the docs"
type: "ghost"
---
::

## Debouncing

For performance, all FormKit inputs support debouncing as a first-class feature. While the value of an input changes on every keystroke (technically the `input` event), this newly updated value is only set internally — validation rules, groups, lists, forms, and (most) plugins are not yet “aware” a change has been made.

Internally, FormKit debounces the `input` event. When the debounce has "settled", the new value is “committed” and the rest of the application is then notified via the [input node’s `commit` event](/essentials/architecture#events). The default debounce delay is 20 milliseconds and can be adjusted with the `delay` prop or config option.

To illustrate this, let's get the `group`'s `value` from the `#default` slot prop and observe how it is not updated until after our 1000ms `delay`:

::Example
---
  name: "Delay prop"
  file: "_content/_examples/delay-prop/delay-prop.vue"
---
::


::Callout
---
type: "info"
label: "Group & List delay"
---
The delay prop’s default is <code>20</code> milliseconds. However, <code>group</code> and <code>list</code> inputs use <code>0</code> milliseconds by default to prevent the debounce delay from “building up” at each level of depth.
::

## Explicit errors

Validation errors are not the only way to set errors on an input. You can also explicitly set error messages on an input by using the `errors` prop.

::Example
---
  name: "Simple validation"
  file: "_content/_examples/simple-errors/simple-errors.vue"
---
::


::Callout
---
type: "info"
label: "Non blocking"
---
Explicitly set errors are non-blocking, meaning they do not prevent the form from submitting the way validation errors do. You can read more about error handling on the <a href="/inputs/form#error-handling">form documentation</a>.
::

## Props & attributes

FormKit inputs accept both _universal_ props (ones that apply to all FormKit inputs), and _input-specific_ props. The following table is a comprehensive list of props available to all FormKit inputs.

:ReferenceTable

## Events

FormKit inputs emit both _universal_ events (ones that are emitted from all inputs), and _input-specific_ events. The following table is a comprehensive list of events emitted by all FormKit inputs.

::ReferenceTable
---
type: "events"
primary: "event"
---
::

::Callout
---
type: "info"
label: "Vue events"
---
  The above are Vue events emitted by <code>@formkit/vue</code>. <code>@formkit/core</code> also emits <a href="/essentials/architecture#core-events">its own events</a> as part of the lifecycle of <a href="/essentials/architecture#node">core nodes</a>.
::

## Sections

Inputs are composed of chunks of HTML called "sections". Each section has a "key" that can be used to target the section for a variety of purposes, like:

- Modifying the section's [classes](/essentials/styling#custom-classes) via `{section-key}-class="your-class"` props
- Overriding the section's structure with [slots](#slots): `<template #{section-key}>`
- Extending each [sections’s schema](#sections-schema)

Many section keys are universally available while others are specific to a given input type (you can define your own for custom inputs as well). The following table is a comprehensive list of those that are generally available in all inputs:

::ReferenceTable
---
type: "sectionKeys"
primary: "section-key"
---
::


## Restructure markup

At times you may find it necessary to restructure the HTML inside a FormKit input, such as adding, editing, moving, or removing sections. This can be done by exporting the input (using the CLI tool), making the desired changes, and then using the modified input in your project. Read the [guide on exporting inputs](/guides/export-and-restructure-inputs) to learn how.

::Cta
--- 
href: "/guides/export-and-restructure-inputs"
label: "Learn to restructure your inputs" 
button: "Export inputs docs"
---
::

## Slots

Inputs can have their structure overridden with slots. You can precisely target where your slot content goes with [section keys](#sections). Slots are then are passed the [context object](/essentials/configuration) for use in their template.

For example, if we wanted to use a slot to define the label of an input, we could use a `label` slot to do so:

::Example
---
  name: "Label slot"
  file: "_content/_examples/label-slot/label-slot.vue"
---
::


::Callout
---
type: "warning"
label: "Consider section schema"
---
A disadvantage of using slots is you often need to re-create unrelated features to make the change you desire. For example, using slots would require you to re-implement any classes applied to those sections (which can be done by using <code>context.classes.sectionName</code>).<br><br>To help address this shortcoming, FormKit is also able to <a href="#sections-schema">selectively override/extend the underlying schema</a> of each section allowing complex structural modification often with no loss of functionality.
::

## Sections schema

FormKit provides an additional mechanism to change the structure of a FormKit input called “sections schema”. Under the hood, all FormKit inputs are powered by [FormKit’s schema](/essentials/schema) — a JSON compatible data format for creating and storing DOM structure and logic. This allows tremendous structural flexibility because all inputs can have pieces of their schema extended via section keys without wholesale replacement of the template.

### Changing HTML tags

For example, by default FormKit uses an unordered list (`<ul>` and `<li>`) to output validation messages — but perhaps you need to use `<div>` tags. You can change these tags using the `schema` prop without having to re-create any functionality:

::Example
---
  name: "Sections schema"
  file: "_content/_examples/schema-overrides/schema-overrides.vue"
  tabs: "html,render"
---
::

### Unwrapping or removing HTML tags

For accessibility and flexibility, FormKit uses several wrapper elements like the ones in the `wrapper` and `inner` [sections](#sections). However, perhaps on some inputs you need to remove a wrapper element to ensure other elements are adjacent. You can do this by providing a `null` value as the schema element:

::Example
---
  name: "No wrappers"
  file: "_content/_examples/schema-wrappers/schema-wrappers.vue"
  tabs: "html,render"
---
::

### Schema logic

Section schemas can also change the content being output using advanced schema logic. You could, for example, output a special value when your input’s value matches a particular string:

::Example
---
  name: "No wrappers"
  file: "_content/_examples/schema-content/schema-content.vue"
---
::

::Cta
---
label: "Learn more about how schemas work" 
href: "/essentials/schema" 
button: "Gimme more schema"
---
