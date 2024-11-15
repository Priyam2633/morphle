# app.py
from flask import Flask
import os
import subprocess
from datetime import datetime
import pytz

app = Flask(__name__)

@app.route('/htop')
def htop():
   
    full_name = "Priyam kumar"
    

    username = os.getlogin()
    

    ist = pytz.timezone('Asia/Kolkata')
    server_time = datetime.now(ist).strftime('%Y-%m-%d %H:%M:%S')
    
    # Get the top output
    try:
        top_output = subprocess.check_output(['top', '-b', '-n', '1']).decode('utf-8')
    except Exception as e:
        top_output = f"Error retrieving 'top' output: {e}"
    
    # HTML structure for output
    response = f"""
    <html>
        <body>
            <h1>System Information</h1>
            <p><strong>Name:</strong> {full_name}</p>
            <p><strong>Username:</strong> {username}</p>
            <p><strong>Server Time (IST):</strong> {server_time}</p>
            <pre><strong>Top Output:</strong>\n{top_output}</pre>
        </body>
    </html>
    """
    return response

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)  # Use port 8080 for public visibility
