<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSV/XLSX Upload</title>
    <!-- Add Bootstrap CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <!-- Add custom CSS -->
    <style>
        body.dark-mode {
            background-color: #343a40;
            color: #fff;
        }
        .file-label {
            display: inline-block;
            padding: 6px 12px;
            cursor: pointer;
            background-color: #007bff;
            color: #fff;
            border-radius: 4px;
        }
        .file-label:hover {
            background-color: #0056b3;
        }
        .row.mt-3 {
            display: flex;
            justify-content: flex-end;
        }
        .toggle-btn {
            position: relative;
            display: inline-block;
            width: 60px;
            height: 34px;
        }
        .toggle-btn input {
            display: none;
        }
        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            -webkit-transition: .4s;
            transition: .4s;
        }
        .slider:before {
            position: absolute;
            content: "";
            height: 26px;
            width: 26px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            -webkit-transition: .4s;
            transition: .4s;
        }
        input:checked + .slider {
            background-color: #2196F3;
        }
        input:checked + .slider:before {
            -webkit-transform: translateX(26px);
            -ms-transform: translateX(26px);
            transform: translateX(26px);
        }
        .slider.round {
            border-radius: 34px;
        }
        .slider.round:before {
            border-radius: 50%;
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
        <div class="row mt-3">
            <div class="col text-right">
                <label class="toggle-btn">
                    <input type="checkbox" onclick="toggleDarkMode()">
                    <span class="slider round"></span>
                </label>
            </div>
        </div>
        <div class="row justify-content-center">
            <div class="col-md-6">
                <form id="upload-form" action="https://banzai-data-app.herokuapp.com/upload_csv" method="POST" enctype="multipart/form-data">
                    <div class="form-group">
                        <input type="file" class="file-input form-control-file" name="file" id="file" accept=".csv,.xlsx" style="display:none;">
                        <label for="file" class="file-label">Choose a CSV or XLSX file</label>
                    </div>
                    <div class="form-group">
                           <button type="submit" class="submit-btn btn btn-primary">Upload CSV</button>
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
    <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.3/dist/umd/popper.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
    <script>    
    document.getElementById('file').addEventListener('change', function () {
        var fileName = this.files[0].name;
        var fileLabel = document.querySelector('.file-label');
        fileLabel.textContent = fileName || 'Choose a CSV or XLSX file';
    });
    

    // Add an event listener for form submission
    document.getElementById('upload-form').addEventListener('submit', function (event) {
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

        // Add event listener for file input change
        xhr.upload.addEventListener('loadstart', function(event) {
            document.getElementById('file').addEventListener('change', function () {
                var fileName = this.files[0].name;
                var fileLabel = document.querySelector('.file-label');
                fileLabel.textContent = fileName || 'Choose a CSV or XLSX file';
            });
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
                var response = JSON.parse(xhr.responseText);
                console.log("Response received:", response); // Add this line to log the response object
                if (response.message === "File processed and saved to Google Sheets") {
                    alert('File uploaded successfully and processed.');
                } else {
                    alert('File uploaded successfully, but an error occurred during processing.');
                }
            } else {
                alert('File upload failed: ' + xhr.statusText);
            }
            document.getElementById('upload-progress').style.display = 'none';
        });
        
        // Handle upload errors
        xhr.addEventListener('error', function (event) {
            var errorMessage = 'File upload failed at ' + getCurrentTimestamp() + '\nError details: ' + xhr.status + ' ' + xhr.statusText + '\n';
            alert('File upload failed with status ' + xhr.status + ': ' + xhr.statusText + '. Check the logs for more details.');
            saveLog(errorMessage);
            document.getElementById('upload-progress').style.display = 'none';
        });
        
        // Add this function to save the log message to a file in a "logs" folder
        function saveLog(logMessage) {
            // Create a Blob object containing the log message
            var logBlob = new Blob([logMessage], { type: 'text/plain' });

            // Create an anchor element to download the log file
            var link = document.createElement('a');
            link.href = URL.createObjectURL(logBlob);
            link.download = 'logs/upload_error_' + getCurrentTimestamp().replace(/[: ]/g, '_') + '.log';

            // Trigger the download and remove the anchor element
            link.click();
            link.remove();
        }

        // Send the form data
        xhr.send(formData);
    });
    </script>
</body>
</html>
