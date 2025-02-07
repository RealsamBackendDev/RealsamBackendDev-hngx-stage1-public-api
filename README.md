# Number Classifier API

This is a simple API that classifies numbers based on various mathematical properties and provides a fun fact about them.

## API Endpoints

### GET [http://127.0.0.1:5000/api/classify-number?number=371](https://vercel.com/realsambackenddevs-projects/realsam-backend-dev-hngx-stage1-public-api)
This endpoint classifies a number and returns various mathematical properties.
## CODE
from flask import Flask, jsonify, request
from flask_cors import CORS  # Import CORS
import requests

# Initialize the Flask app
app = Flask(__name__)

# Apply CORS to the app
CORS(app)  # Enable CORS

# Helper function to check if a number is prime
def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            return False
    return True

# Helper function to check if a number is perfect
def is_perfect(n):
    divisors_sum = sum(i for i in range(1, n) if n % i == 0)
    return divisors_sum == n

# Helper function to check if a number is Armstrong
def is_armstrong(n):
    digits = list(map(int, str(n)))
    power = len(digits)
    return n == sum(digit ** power for digit in digits)

# Helper function to get the sum of digits
def digit_sum(n):
    return sum(int(digit) for digit in str(n))

# Helper function to classify number properties
def classify_number(n):
    properties = []
    if is_armstrong(n):
        properties.append("armstrong")
    if n % 2 == 0:
        properties.append("even")
    else:
        properties.append("odd")
    
    return properties

# Function to get a fun fact using the Numbers API
def get_fun_fact(n):
    response = requests.get(f"http://numbersapi.com/{n}?json&math")
    if response.status_code == 200:
        return response.json().get("text", "")
    else:
        return "No fun fact available."

@app.route('/api/classify-number', methods=['GET'])
def classify():
    try:
        # Get number from the query parameter
        number = request.args.get('number')
        
        # Validate if the number is an integer
        if not number.isdigit():
            return jsonify({"number": number, "error": True}), 400

        number = int(number)

        # Classify the number
        properties = classify_number(number)
        digit_sum_value = digit_sum(number)
        fun_fact = get_fun_fact(number)

        response = {
            "number": number,
            "is_prime": is_prime(number),
            "is_perfect": is_perfect(number),
            "properties": properties,
            "digit_sum": digit_sum_value,
            "fun_fact": fun_fact
        }

        return jsonify(response)

    except Exception as e:
        return jsonify({"error": str(e)}), 500

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5000)


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
