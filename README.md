# Number Classifier API

This is a simple API that classifies numbers based on various mathematical properties and provides a fun fact about them.

## API Endpoints

### GET http://127.0.0.1:5000/api/classify-number?number=371
This endpoint classifies a number and returns various mathematical properties.

#### Query Parameters
- `number`: The number you want to classify. It must be an integer.

#### Example Response

```json
{
    "number": 371,
    "is_prime": false,
    "is_perfect": false,
    "properties": ["armstrong", "odd"],
    "digit_sum": 11,
    "fun_fact": "371 is an Armstrong number because 3^3 + 7^3 + 1^3 = 371"
}
