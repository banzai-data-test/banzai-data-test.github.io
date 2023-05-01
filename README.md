# CSV / XLSX Upload GitHub Page

This is a simple GitHub Pages site that provides a user-friendly web interface for uploading CSV / XLSX files. It is styled using Bootstrap, a popular CSS framework, and communicates with a separate Flask-based backend for processing the uploaded files.

## Features

- Upload CSV / XLSX files through a user-friendly web interface
- Progress bar to display the upload progress in real-time
- Dark mode toggle to switch between light and dark themes
- Responsive design that works on various devices and screen sizes

## Usage

1. Visit the GitHub Pages site at https://yourusername.github.io/csv-upload-webapp/.
2. Click the "Choose a CSV / XLSX file" button to open a file picker dialog.
3. Select a CSV file from your local machine.
4. Click the "Upload CSV / XLSX" button to start the file upload. The progress bar will indicate the upload progress.
5. Once the upload is complete, you will see an alert indicating that the file was uploaded successfully.

**Note**: The Flask backend application is hosted separately and is not part of this repository. Make sure to update the `action` attribute of the `form` element in the `index.html` file to point to your Flask application's `/upload_csv` endpoint.

## Customization

You can easily customize the appearance of this GitHub Pages site by modifying the CSS styles in the `index.html` file. Additionally, you can modify the JavaScript code for handling the upload process or adding new features. Simply edit the `index.html` file and push your changes to the `gh-pages` branch of your repository.
