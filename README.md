# Autofill With Resume

`autofill-with-resume` is a powerful Node.js package designed to extract structured data from resumes in PDF format and convert it into a JSON format. Leveraging the Gemini API, it enhances accuracy in extracting detailed information, making it a useful tool for developers building resume parsing systems.

## Installation

To install `autofill-with-resume`, ensure you have Node.js installed, and run the following command:

```bash
npm install autofill-with-resume
```

## API Key Setup

To use this package, you need a Gemini API key. You can obtain this key by signing up or logging into [Google AI Studio](https://aistudio.google.com/app/apikey). Once you have the key, it can be integrated into your application as shown in the usage examples.

## Basic Usage

### Simple Use Case

Here is a basic example demonstrating how to use `autofill-with-resume` to extract data from a PDF resume:

```javascript
const AutofillWithResume = require('autofill-with-resume');

const processor = new AutofillWithResume('YOUR_GEMINI_API_KEY', './Resume_name.pdf');

processor.extractResumeDetails()
  .then(details => {
    console.log(details);
  })
  .catch(error => {
    console.error("Error processing resume:", error);
  });
```

Replace `'YOUR_GEMINI_API_KEY'` with your actual API key and ensure the PDF file path is correct.

### Server Use Case

If you're building a server application using Node.js and Express, you can implement the following pattern to handle resume uploads and extractions dynamically:

```javascript
const express = require('express');
const multer = require('multer');
const AutofillWithResume = require('autofill-with-resume');
const fs = require('fs');
const path = require('path');

const app = express();
const upload = multer({ dest: 'uploads/' });

const API_KEY = 'YOUR_GEMINI_API_KEY';

app.post('/upload', upload.single('resume'), (req, res) => {
  const filePath = req.file.path;

  const processor = new AutofillWithResume(API_KEY, filePath);

  processor.extractResumeDetails()
    .then(details => {
      res.json(details);
      fs.unlinkSync(filePath); // Clean up the uploaded file
    })
    .catch(error => {
      console.error("Error processing resume:", error);
      res.status(500).json({ error: "Error processing resume" });
      fs.unlinkSync(filePath); // Clean up the uploaded file
    });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

Make sure to replace `'YOUR_GEMINI_API_KEY'` with your actual key. The server code handles file uploads, processes the resume, and returns the JSON response while also cleaning up temporary files.

## JSON Response Structure

The JSON response from the extraction contains structured details including:

- **Personal Details**: Name, website, email, phone, and social media links.
- **Education**: A list of educational qualifications, institutes, and related details.
- **Skills**: An array of skill sets extracted from the resume.
- **Experience**: Employment history including job titles, companies, durations, and descriptions of roles.
- **Projects**: Information on projects, the technologies used, descriptions, and links.
- **Certifications**: Certifications with their names, platforms, years obtained, and links.
- **Achievements**: Detailing titles, competitions, prizes, organizations, locations, and years.

This structured format allows for easy integration into systems that require organized and detailed resume information.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

For further information, contributions, or issues, please visit the [GitHub repository](https://github.com/mahsook3/autofill-with-resume) and check the documentation there.