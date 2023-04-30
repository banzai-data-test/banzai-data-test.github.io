<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSV Upload</title>
    <!-- Add Bootstrap CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <!-- Add custom CSS for animations -->
    <style>
        /* ... */
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
        <!-- ... -->
        <div class="row justify-content-center">
            <div class="col-md-6">
                <form id="upload-form" action="https://your-flask-app.herokuapp.com/upload_csv" method="POST" enctype="multipart/form-data">
                    <div class="text-center">
                        <input type="file" class="file-input" name="file" id="file" accept=".csv">
                        <label for="file" class="file-label">Choose a CSV file</label>
                    </div>
                    <div class="text-center">
                        <button type="submit" class="submit-btn">Upload CSV</button>
                    </div>
                    <div id="upload-progress" class="mt-3" style="display:none;">
                        <p class="text-center">Uploading...</p>
                        <div class="progress">
                            <div class="progress-bar" role="progressbar" style="width: 0%;" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100"></div>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
    
    <!-- Add Bootstrap JavaScript and its dependencies -->
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.3/dist/umd/popper.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
    
    <script>
    // Add an event listener for form submission
    document.getElementById('upload-form').addEventListener('submit', function(event) {
        event.preventDefault(); // Prevent the form from submitting normally
        var form = event.target;
        var formData = new FormData(form);
        var xhr = new XMLHttpRequest();
        xhr.open(form.method, form.action, true);

        // Update the progress bar when the file is being uploaded
        xhr.upload.addEventListener('progress', function(event) {
            if (event.lengthComputable) {
                var percentComplete = (event.loaded / event.total) * 100;
                var progressBar = document.querySelector('.progress-bar');
                progressBar.style.width = percentComplete + '%';
                progressBar.setAttribute('aria-valuenow', percentComplete);
            }
        });

        // Show the progress bar and reset it when the upload starts
        xhr.upload.addEventListener('loadstart', function(event) {
            var progressBar = document.querySelector('.progress-bar');
            progressBar.style.width = '0%';
            progressBar.setAttribute('aria-valuenow', 0);
            document.getElementById('upload-progress').style.display = 'block';
        });

        // Handle the upload completion
        xhr.addEventListener('load', function(event) {
            if (xhr.status === 200) {
                alert('File uploaded successfully');
            } else {
                alert('File upload failed: ' + xhr.statusText);
            }
            document.getElementById('upload-progress').style.display = 'none';
        });

        // Handle upload errors
        xhr.addEventListener('error', function(event) {
            alert('File upload failed');
            document.getElementById('upload-progress').style.display = 'none';
        });

        // Send the form data
        xhr.send(formData);
    });
</script>
</body>
</html>
