The CCTM ships with the following Validators available:

  * [Email](email_Validator.md) -- ensures the input is a valid email address
  * [Number](number_Validator.md) -- ensures the input is a valid number (many configurable options)
  * [Pattern](pattern_Validator.md) -- ensures the input follows a pattern of letters and numbers OR a regex (for developers)
  * [URL](url_Validator.md) -- ensures the input is a valid URL


# How to Use #

You use a validator when you are defining a custom field.  Each field contains various options, and one of those options is what kind of validation rule to use for that field.

![https://img.skitch.com/20120625-dxd7de4w5491d7wy8re2huspxb.jpg](https://img.skitch.com/20120625-dxd7de4w5491d7wy8re2huspxb.jpg)

  1. Create a new Custom Field : **CCTM --> Custom Fields**
  1. Scroll down to the "Validation" section.
  1. Choose a validation rule and fill out any options (if any).
  1. Add your field to one or more post-types under the "Associations" section.
  1. Save your field

Now when you create posts or pages that use this field, the fields will be validated when users save the page.  They can still _save_ the page, but the page cannot be published until the fields contain valid input.

### Sample Error Messages ###

At the top of the page:

![https://img.skitch.com/20120625-gqh8ts84spybksdspt55a12duw.jpg](https://img.skitch.com/20120625-gqh8ts84spybksdspt55a12duw.jpg)

The offending field:

![https://img.skitch.com/20120625-b8r63u3ab537233atpqfkpqxjt.jpg](https://img.skitch.com/20120625-b8r63u3ab537233atpqfkpqxjt.jpg)

# Developing Custom Validators #

Beginning with version 0.9.5 of the CCTM, developers can create their own [Custom Validation Rules](CustomValidators.md).