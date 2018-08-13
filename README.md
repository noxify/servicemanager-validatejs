# servicemanager-validatejs

This is the port of https://github.com/ansman/validate.js.

The difference between the original version and this one is the removed async functionality.
Also we added Underscore.js to replace some native js functionality like `map`, `forEach` and `filter`, because the Service Manager has an old JS Engine.

# Documentation

You can find the full documentation here: https://validatejs.org/#validate-js

# Usage

1. Create a new ScriptLibrary named `validatejs`
2. Copy the content (Source or Minified Version) into the created ScriptLibrary
3. Create a new ScriptLibrary named `validatejs_test` with the following content
```
var validate = system.library.validatejs.require();

//presence example
var resPresense1 = validate({input: ""}, {input: {presence: {allowEmpty: false}}});
print(resPresense1.toSource());
// => {"input:" ["Input can't be blank"]}

var resPresense2 = validate({}, {username: {presence: {message: "is required"}}});
print(resPresense2.toSource());
// => {"username": ["Username is required"]}

// format example
var constraintsFormat = {
  username: {
    format: {
      pattern: "[a-z0-9]+",
      flags: "i",
      message: "can only contain a-z and 0-9"
    }
  }
};

var resFormatInvalid = validate({username: "Nicklas!"}, constraintsFormat);
print(resFormatInvalid.toSource());
// => {"username": ["Username can only contain a-z and 0-9"]}

var resFormatValid = validate({username: "Nicklas"}, constraintsFormat);
print(resFormatValid.toSource());
// => undefined

// custom validations
validate.validators.custom = function(value, options, key, attributes) {
  print(value);
  print(options);
  print(key);
  print(attributes);
  return "is totally wrong";
};

var resCustom = validate({foo: "some value"}, {foo: {custom: "some options"}});

print(resCustom.toSource());


```
