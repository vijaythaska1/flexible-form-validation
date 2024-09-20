# flexible-form-validation

A flexible and powerful form validation library for React applications.

## Features

- Easy-to-use validation schema creation
- Support for various field types (string, email, number, date, etc.)
- Customizable validation rules and error messages
- Real-time field validation
- Full form validation
- Lightweight and performant

## Installation

```bash
npm install flexible-form-validation
```

or if you're using yarn:

```bash
yarn add flexible-form-validation
```

## Usage

### Importing

```javascript
import createValidationSchema, { validateField, validateForm } from 'flexible-form-validation';
```

### Creating a Validation Schema

```javascript
const validationSchema = createValidationSchema([
  { 
    name: 'username', 
    type: 'string', 
    required: true, 
    min: 3, 
    max: 20,
    messages: {
      'string.min': 'Username must be at least 3 characters long',
      'string.max': 'Username cannot exceed 20 characters'
    }
  },
  { 
    name: 'email', 
    type: 'email', 
    required: true 
  },
  { 
    name: 'age', 
    type: 'number', 
    min: 18, 
    max: 100,
    messages: {
      'number.min': 'You must be at least 18 years old',
      'number.max': 'Age cannot exceed 100'
    }
  },
  { 
    name: 'birthdate', 
    type: 'date', 
    required: true 
  }
]);
```

### Validating a Single Field

```javascript
const handleChange = (event) => {
  const { name, value } = event.target;
  const fieldErrors = validateField(validationSchema, name, value);
  setErrors(prev => ({ ...prev, ...fieldErrors }));
};
```

### Validating the Entire Form

```javascript
const handleSubmit = (e) => {
  e.preventDefault();
  const { isValid, errors } = validateForm(validationSchema, formData);
  if (!isValid) {
    setErrors(errors);
    return;
  }
  // Proceed with form submission...
};
```

## API Reference

### `createValidationSchema(fields)`

Creates a validation schema based on the provided field configurations.

- `fields`: An array of field objects, each containing:
  - `name`: Field name (string)
  - `type`: Field type ('string', 'email', 'number', 'date', etc.)
  - `required`: Whether the field is required (boolean)
  - `min`: Minimum value/length (number)
  - `max`: Maximum value/length (number)
  - `messages`: Custom error messages (object)

Returns a validation schema object.

### `validateField(schema, name, value)`

Validates a single field.

- `schema`: The validation schema created by `createValidationSchema`
- `name`: The name of the field to validate
- `value`: The value to validate

Returns an object with the field name as key and the error message (if any) as value.

### `validateForm(schema, data)`

Validates the entire form.

- `schema`: The validation schema created by `createValidationSchema`
- `data`: An object containing all form data

Returns an object with `isValid` (boolean) and `errors` (object with field names as keys and error messages as values).

## Example

Here's a complete example of how to use the library in a React component:

```jsx
import React, { useState } from 'react';
import createValidationSchema, { validateField, validateForm } from 'flexible-form-validation';

const validationSchema = createValidationSchema([
  { name: 'username', type: 'string', required: true, min: 3, max: 20 },
  { name: 'email', type: 'email', required: true },
  { name: 'age', type: 'number', min: 18, max: 100 }
]);

function FormComponent() {
  const [formData, setFormData] = useState({ username: '', email: '', age: '' });
  const [errors, setErrors] = useState({});

  const handleChange = (event) => {
    const { name, value } = event.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    const fieldErrors = validateField(validationSchema, name, value);
    setErrors(prev => ({ ...prev, ...fieldErrors }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const { isValid, errors } = validateForm(validationSchema, formData);
    if (!isValid) {
      setErrors(errors);
      return;
    }
    // Proceed with form submission...
    console.log('Form is valid!', formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        name="username"
        value={formData.username}
        onChange={handleChange}
        placeholder="Username"
      />
      {errors.username && <p>{errors.username}</p>}
      
      <input
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      {errors.email && <p>{errors.email}</p>}
      
      <input
        name="age"
        value={formData.age}
        onChange={handleChange}
        placeholder="Age"
        type="number"
      />
      {errors.age && <p>{errors.age}</p>}
      
      <button type="submit">Submit</button>
    </form>
  );
}

export default FormComponent;
```

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.