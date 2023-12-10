# HotwireCombobox

A combobox implementation for Ruby on Rails apps running on Hotwire.

## Installation
Add this line to your application's Gemfile:

```ruby
gem "hotwire_combobox"
```

And then execute:
```bash
$ bundle
```

## Output

This is the stripped-down output of a combobox generated with HotwireCombobox. Understanding it will be helpful in getting the most out of this library.

```html
<fieldset class="hw-combobox">
  <input type="hidden" name="provided_name">

  <input type="text" role="combobox">

  <ul role="listbox">
    <li role="option">Provided Option 1 Content</li>
    <li role="option">Provided Option 2 Content</li>
    <!-- ... -->
  </ul>
</fieldset>
```

The `<ul role="listbox">` element is what gets shown when the combobox is open.

The library uses stimulus to add interactivity to the combobox and sync the input element's value with the hidden input element.

The hidden input's value is what ultimately gets sent to the server when submitting a form containing the combobox.

## Usage

### Options

Options are what you see when you open the combobox.

The `options` argument takes an array of any objects which respond to:

| Attribute                        | Description                                                                                                                                | Required? |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|-------------------|
| `id`                             | Used as the option element's `id` attribute. Only required if `value` is not provided.                                                     | Required*         |
| `value`                          | Used to populate the input element's `value` attribute. Falls back to calling `id` on the object if not provided.                          | Optional          |
| `content` <br> **Supports HTML** | Used as the option element's content. Falls back to calling `display` on the object if not provided.                                       | Optional          |
| `filterable_as`                  | Used to filter down the options when the user types into the input element. Falls back to calling `display` on the object if not provided. | Optional          |
| `autocompletable_as`             | Used to autocomplete the input element when the user types into it. Falls back to calling `display` on the object if not provided.         | Optional          |
| `display`                        | Used as a short-hand for other attributes. See the rest of the list for details.                                                           | Optional          |


> [!NOTE]
> The `id` attribute is required only if `value` is not provided.


The gem provides a `HotwireCombobox::Option` class which you can use to create options:

```ruby
@states = [
  HotwireCombobox::Option.new(value: "AL", display: "Alabama"),
  # ...
]
```

If you feel `HotwireCombobox::Option` is too verbose, you can also use the `hwbox_options` helper. It will destructure the hashes you pass to it and create `HotwireCombobox::Option` instances for you:

```ruby
@states = hwbox_options [
  { value: "AL", display: "Alabama" },
  # ...
]
```

### Styling

The combobox is completely unstyled by default. You can use the following CSS selectors to style the combobox:

* `.hw-combobox` targets the `<fieldset>` element used to wrap the whole component.
* `.hw-combobox [role="combobox"]` targets the input element.
* `.hw-combobox [role="listbox"]` targets the listbox which encloses all option elements.
* `.hw-combobox [role="option"]` targets each option element inside the listbox.

Additionally, you can pass the following [Stimulus class values](https://stimulus.hotwired.dev/reference/css-classes) to `hw_combobox_tag`:

* `data-hw-combobox-selected-class`: The class to apply to the selected option while shown inside an open listbox.
* `data-hw-combobox-invalid-class`: The class to apply to the input element when the current value is invalid.

### Validity

The hidden input can't have a value that's not in the list of options.

If a nonexistent value is typed into the combobox, the value of the hidden input will be set empty.

The only way a value can be marked as invalid is if the field is required and empty after having interacted with the combobox.

The library will mark the element as invalid but this won't be noticeable in the UI unless you specify styles for the invalid state. See the [Styling](#styling) section for details.

> [!CAUTION]
> Bad actors can still submit invalid values to the server. You should always validate the input on the server side.

## Contributing

### Setup
```bash
$ bin/setup
```

### Running the tests
```bash
$ bundle exec rake app:test
```

```bash
$ bundle exec rake app:test:system
```

### Running the dummy app
```bash
$ bin/rails s
```

### Releasing

1. Bump the version in `lib/hotwire_combobox/version.rb` (e.g. `VERSION = "0.1.0"`)
2. Bump the version in `Gemfile.lock` (e.g. `hotwire_combobox (0.1.0)`)
3. Commit the change (e.g. `git commit -am "Bump to 0.1.0"`)
4. Run `bundle exec rake release`

## License
The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
