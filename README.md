# Flexible Form Validation

## Overview

Flexible Form Validation is a React-based project that provides a customizable and easy-to-use form validation system. Built with Vite and using Joi for schema validation, this project offers a robust solution for handling form validations in React applications.

## Features

- Flexible field definitions with support for various types (string, email, number, date, etc.)
- Custom validation rules for each field (required, min, max, etc.)
- Custom error messages for each validation rule
- Real-time field validation as the user types
- Full form validation on submission
- Easy integration with React components

## Prerequisites

- Node.js (version 14 or higher)
- npm or yarn

## Installation

1. Clone the repository:
   ```
   git clone https://github.com/your-username/flexible-form-validation.git
   cd flexible-form-validation
   ```

2. Install dependencies:
   ```
   npm install
   ```
   or if you're using yarn:
   ```
   yarn
   ```

## Usage

1. Start the development server:
   ```
   npm run dev
   ```
   or with yarn:
   ```
   yarn dev
   ```

2. Open your browser and navigate to `http://localhost:5173` (or the port shown in your terminal).

## Creating a Form with Validation

Here's a basic example of how to use the validation system in a React component:

```jsx
import React, { useState } from "react";
import createValidationSchema, { validateField, validateForm } from 'flexible-form-validation';

function FormComponent() {
    const [data, setData] = useState({
        name: "",
        email: "",
        age: "",
    });
    const [errors, setErrors] = useState({});

    const validationSchema = createValidationSchema([
        { 
            name: 'name', 
            type: 'string', 
            required: true, 
            min: 2, 
            max: 50,
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
        },
    ]);

    const handleChange = (event) => {
        const { name, value } = event.target;
        setData(prev => ({ ...prev, [name]: value }));
        const fieldErrors = validateField(validationSchema, name, value);
        setErrors(prev => ({ ...prev, ...fieldErrors }));
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        const { isValid, errors } = validateForm(validationSchema, data);
        if (!isValid) {
            setErrors(errors);
            return;
        }
        // Proceed with form submission...
    };

    return (
        <form onSubmit={handleSubmit}>
            <input
                name="name"
                value={data.name}
                onChange={handleChange}
                placeholder="Name"
            />
            {errors.name && <p>{errors.name}</p>}
            
            <input
                name="email"
                value={data.email}
                onChange={handleChange}
                placeholder="Email"
            />
            {errors.email && <p>{errors.email}</p>}
            
            <input
                name="age"
                value={data.age}
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

## Customizing Validation Rules

You can customize validation rules for each field by modifying the schema in the `createValidationSchema` function. For example:

```javascript
const validationSchema = createValidationSchema([
    { 
        name: 'password', 
        type: 'string', 
        required: true, 
        min: 8,
        messages: {
            'string.min': 'Password must be at least 8 characters long'
        }
    },
    // ... other fields
]);
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.