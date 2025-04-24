

## Setting up the Python Flask Environment

1. **Update the system and install Python**:
   ```bash
   sudo dnf update -y
   sudo dnf install python39 python39-devel python39-pip -y
   ```

2. **Install required packages**:
   ```bash
   sudo dnf install gcc -y
   ```

3. **Create a virtual environment**:
   ```bash
   mkdir /var/www/flask_app
   cd /var/www/flask_app
   python3.9 -m venv venv
   source venv/bin/activate
   ```

4. **Install Flask and related packages**:
   ```bash
   pip install flask flask-sqlalchemy pymysql gunicorn
   ```

5. **Set up Gunicorn as a service**:
   ```bash
   sudo nano /etc/systemd/system/flask_app.service
   ```
   
   Add the following:
   ```
   [Unit]
   Description=Gunicorn instance to serve Flask application
   After=network.target

   [Service]
   User=apache
   Group=apache
   WorkingDirectory=/var/www/flask_app
   Environment="PATH=/var/www/flask_app/venv/bin"
   ExecStart=/var/www/flask_app/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:5000 app:app

   [Install]
   WantedBy=multi-user.target
   ```

6. **Enable and start the service**:
   ```bash
   sudo systemctl enable flask_app
   sudo systemctl start flask_app
   ```

7. **Configure firewall** (if needed):
   ```bash
   sudo firewall-cmd --permanent --add-port=5000/tcp
   sudo firewall-cmd --reload
   ```

## Python Flask Application Code

Let's create the application files:

1. **Create app.py** - Main application file:

```python
from flask import Flask, render_template, request
import pymysql
from pymysql.cursors import DictCursor
import os

app = Flask(__name__)

# Database configuration
DB_CONFIG = {
    'host': 'localhost',
    'user': 'admin',
    'password': '',
    'db': 'sakila',
    'charset': 'utf8mb4',
    'cursorclass': DictCursor
}

# Function to replace the MySQL secondsToHoursMinsSecs function
def seconds_to_hours_mins_secs(seconds, format_type='short'):
    try:
        seconds = float(seconds)
        hours = int(seconds // 3600)
        minutes = int((seconds % 3600) // 60)
        secs = int(seconds % 60)
        
        if format_type == 'short':
            return f"{hours:02d}:{minutes:02d}:{secs:02d}"
        else:  # long format
            parts = []
            if hours > 0:
                parts.append(f"{hours} hour{'s' if hours != 1 else ''}")
            if minutes > 0:
                parts.append(f"{minutes} minute{'s' if minutes != 1 else ''}")
            if secs > 0 or not parts:  # Include seconds if it's non-zero or if hours and minutes are both zero
                parts.append(f"{secs} second{'s' if secs != 1 else ''}")
            return " ".join(parts)
    except ValueError:
        return "Invalid input"

# Get a database connection
def get_db_connection():
    return pymysql.connect(**DB_CONFIG)

@app.route('/', methods=['GET', 'POST'])
def index():
    time_result = ''
    time_error = ''
    films = []
    
    # Time Converter Processing
    if request.method == 'POST' and 'convert_time' in request.form:
        seconds = request.form.get('seconds', '')
        format_type = request.form.get('format', 'short')
        
        if seconds and seconds.replace('.', '', 1).isdigit() and float(seconds) >= 0:
            time_result = seconds_to_hours_mins_secs(float(seconds), format_type)
        else:
            time_error = 'Enter a valid number of seconds'
    
    # Get the film list from database
    try:
        connection = get_db_connection()
        with connection.cursor() as cursor:
            cursor.execute("SELECT * FROM film_list")
            films = cursor.fetchall()
            
            # Convert film length to formatted time
            for film in films:
                length_in_seconds = film['length'] * 60
                film['formatted_length'] = seconds_to_hours_mins_secs(length_in_seconds, 'short')
                
        connection.close()
    except Exception as e:
        print(f"Database error: {e}")
    
    return render_template('index.html', 
                           films=films, 
                           time_result=time_result, 
                           time_error=time_error,
                           seconds=request.form.get('seconds', ''),
                           format=request.form.get('format', 'short'))

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

2. **Create templates directory and index.html template**:

```bash
mkdir -p /var/www/flask_app/templates
```

```html
<!DOCTYPE html>
<html>
<head>
    <title>Sakila Films</title>
    <link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h1>Sakila Film List</h1>
    
    <!-- Time Converter Section -->
    <div class="time-converter">
        <h2>Time Converter</h2>
        <p>Convert seconds to hours, minutes, and seconds format</p>
        
        <form method="POST">
            <label for="seconds">Enter seconds:</label>
            <input type="text" id="seconds" name="seconds" value="{{ seconds }}">
            
            <label for="format">Select format:</label>
            <select id="format" name="format">
                <option value="short" {% if format == 'short' %}selected{% endif %}>Short (00:00:00)</option>
                <option value="long" {% if format == 'long' %}selected{% endif %}>Long (X hours Y minutes Z seconds)</option>
            </select>
            
            <button type="submit" name="convert_time" value="1">Convert</button>
        </form>
        
        {% if time_result %}
        <div class="time-result">
            Result: {{ time_result }}
        </div>
        {% endif %}
        
        {% if time_error %}
        <div class="time-error">
            {{ time_error }}
        </div>
        {% endif %}
    </div>
    
    <!-- Sakila Film List Table -->
    <table>
        <tr>
            <th>Title</th>
            <th>Description</th>
            <th>Category</th>
            <th>Price</th>
            <th>Length</th>
            <th>Length (Formatted)</th>
            <th>Rating</th>
        </tr>
        {% for film in films %}
        <tr>
            <td>{{ film.title }}</td>
            <td>{{ film.description }}</td>
            <td>{{ film.category }}</td>
            <td>{{ film.price }}</td>
            <td>{{ film.length }} min</td>
            <td>{{ film.formatted_length }}</td>
            <td>{{ film.rating }}</td>
        </tr>
        {% endfor %}
    </table>
</body>
</html>
```

3. **Create static directory and CSS file**:

```bash
mkdir -p /var/www/flask_app/static
```

```css
body {
    font-family: Arial, sans-serif;
    margin: 20px;
    padding: 20px;
    background-color: #8cb9f7;
}

h1, h2 {
    color: #333;
}

table {
    width: 100%;
    border-collapse: collapse;
    background-color: white;
}

th, td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
}

th {
    background-color: #333;
    color: white;
}

/* Simple Time Converter Styles */
.time-converter {
    margin-bottom: 20px;
    background-color: white;
    padding: 15px;
    border-radius: 5px;
}

.time-converter input[type="text"],
.time-converter select {
    padding: 4px;
    margin-right: 5px;
}

.time-converter button {
    padding: 4px 10px;
    background-color: #333;
    color: white;
    border: none;
    cursor: pointer;
}

.time-result {
    margin-top: 10px;
    padding: 8px;
    background-color: #e8f4ff;
    border-left: 3px solid #4d90fe;
}

.time-error {
    color: red;
    margin-top: 10px;
}
```

4. **Set proper permissions**:

```bash
sudo chown -R apache:apache /var/www/flask_app
```

5. **Test the application**:

Access your application at http://your-server-ip:5000

This setup provides you with a Flask application that replicates the functionality of your PHP application. The Python code handles database connections, implements the time conversion function, and renders the HTML template with Jinja2 (Flask's templating engine).