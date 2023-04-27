<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSV Upload</title>
</head>
<body>
    <h1>Upload a CSV file</h1>
    <form action="https://your-flask-app.herokuapp.com/upload_csv" method="POST" enctype="multipart/form-data">
        <input type="file" name="file" accept=".csv">
        <button type="submit">Upload CSV</button>
    </form>
</body>
</html>
