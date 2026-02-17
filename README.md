# Task-4
Build a REST API with flask
from flask import Flask, request, jsonify

# Create Flask app
app = Flask(__name__)

# In-memory storage (dictionary)
users = {}

# ------------------------------
# Home Route
# ------------------------------
@app.route('/')
def home():
    return "User Management REST API is Running!"

# ------------------------------
# GET - Get all users
# ------------------------------
@app.route('/users', methods=['GET'])
def get_users():
    return jsonify(users)

# ------------------------------
# GET - Get single user
# ------------------------------
@app.route('/users/<int:user_id>', methods=['GET'])
def get_user(user_id):
    if user_id in users:
        return jsonify(users[user_id])
    else:
        return jsonify({"error": "User not found"}), 404

# ------------------------------
# POST - Add new user
# ------------------------------
@app.route('/users', methods=['POST'])
def add_user():
    data = request.get_json()

    if not data:
        return jsonify({"error": "No data provided"}), 400

    user_id = len(users) + 1
    users[user_id] = data

    return jsonify({
        "message": "User added successfully",
        "user_id": user_id,
        "user": users[user_id]
    }), 201

# ------------------------------
# PUT - Update user
# ------------------------------
@app.route('/users/<int:user_id>', methods=['PUT'])
def update_user(user_id):
    if user_id not in users:
        return jsonify({"error": "User not found"}), 404

    data = request.get_json()
    users[user_id] = data

    return jsonify({
        "message": "User updated successfully",
        "user": users[user_id]
    })

# ------------------------------
# DELETE - Remove user
# ------------------------------
@app.route('/users/<int:user_id>', methods=['DELETE'])
def delete_user(user_id):
    if user_id not in users:
        return jsonify({"error": "User not found"}), 404

    deleted_user = users.pop(user_id)

    return jsonify({
        "message": "User deleted successfully",
        "user": deleted_user
    })

# ------------------------------
# Run the App
# ------------------------------
if __name__ == '__main__':
    app.run(debug=True)
