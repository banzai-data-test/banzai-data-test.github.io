<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSV Upload</title>
    <!-- Add Bootstrap CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <!-- Add custom CSS for animations -->
    <style>
        body.light-mode .page-heading {
             color: black;
        }

        body.dark-mode .page-heading {
            color: white;
        }
        body.light-mode {
            background-color: white;
            color: black;
        }
        
        body.dark-mode {
            background-color: #212529;
            color: white;
        }
        
        .file-input {
            opacity: 0;
            position: absolute;
        }
        
        .file-label {
            border: 2px solid #007bff;
            border-radius: 4px;
            color: #007bff;
            cursor: pointer;
            display: inline-block;
            padding: 8px 12px;
            transition: background-color 0.2s;
        }
        
        .file-label:hover {
            background-color: #007bff;
            color: white;
        }
        
        .submit-btn {
            background-color: transparent;
            border: 2px solid #007bff;
            border-radius: 4px;
            color: #007bff;
            cursor: pointer;
            display: inline-block;
            margin-top: 16px;
            padding: 8px 12px;
            transition: background-color 0.2s;
        }
        
        .submit-btn:hover {
            background-color: #007bff;
            color: white;
        }
    </style>
    <!-- Add custom JavaScript for toggling light and dark mode -->
    <script>
        function toggleDarkMode() {
            document.body.classList.toggle('dark-mode');
        }
    </script>
</head>
<body class="light-mode">
    <div class="container">
        <div class="d-flex justify-content-end mt-2">
            <button class="btn btn-sm btn-outline-secondary" onclick="toggleDarkMode()">Toggle Dark Mode</button>
        </div>
        <h1 class="mt-3 mb-4 text-center page-heading">Upload a CSV file</h1>
        <div class="row justify-content-center">
            <div class="col-md-6">
                <form action="https://your-flask-app.herokuapp.com/upload_csv" method="POST" enctype="multipart/form-data">
                    <div class="text-center">
                        <input type="file" class="file-input" name="file" id="file" accept=".csv">
                        <label for="file" class="file-label">Choose a CSV file</label>
                    </div>
                    <div class="text-center">
                        <button type="submit" class="submit-btn">Upload CSV</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
    
    <!-- Add Bootstrap JavaScript and its dependencies -->
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.3/dist/umd/popper.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
</body>
</html>
