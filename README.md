import React, { useState, useCallback } from "react";
import createValidationSchema, { validateField, validateForm } from 'flexible-form-validation';

function FormComponent() {
    const [data, setData] = useState({
        name: "",
        email: "",
        age: "",
        birthdate: "",
        password: ""
    });
    const [errors, setErrors] = useState({});

    const validationSchema = createValidationSchema([
        { 
            name: 'name', 
            type: 'string', 
            required: true, 
            min: 2, 
            max: 50,
            messages: {
                'string.min': 'Name should be at least 2 characters long',
                'string.max': 'Name should not exceed 50 characters'
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
        },
        { 
            name: 'password', 
            type: 'string', 
            required: true, 
            min: 8,
            messages: {
                'string.min': 'Password should be at least 8 characters long'
            }
        }
    ]);

    const handleChange = (event) => {
        const { name, value } = event.target;
        setData(prev => ({ ...prev, [name]: value }));
        setTimeout(() => {
            const fieldErrors = validateField(validationSchema, name, value);
            setErrors(prev => ({ ...prev, ...fieldErrors }));
        }, 1000);
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
            {/* Render your form fields here */}
            {/* Use the 'data' state for values and 'errors' state for displaying error messages */}
        </form>
    );
}