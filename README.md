# Qurey-Interface
from flask import Flask, request, jsonify
import sqlite3

app = Flask(_name_)
DB_NAME = 'logs.db'

def execute_query(query, params=None):
    with sqlite3.connect(DB_NAME) as conn:
        cursor = conn.cursor()
        if params:
            cursor.execute(query, params)
        else:
            cursor.execute(query)
        result = cursor.fetchall()
    return result

@app.route('/search', methods=['GET'])
def search_logs():
    query = request.args.get('query')
    if query:
        result = execute_query("SELECT * FROM logs WHERE " + query)
        return jsonify(result)
    else:
        return jsonify({'error': 'Query parameter is missing'}), 400

if _name_ == '_main_':
    app.run(port=3001)
