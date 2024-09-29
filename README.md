# Autofill With Resume

`autofill-with-resume` is a package used to extract details from a resume and convert them into JSON format. It also utilizes the Gemini API to extract details more accurately.

## Installation

To install the package, run:

```bash
npm i autofill-with-resume
```


## Basic Usage

### Simple Use Case

```javascript
const AutofillWithResume = require('autofill-with-resume');

const processor = new AutofillWithResume('your_gemini_api', './Resume_name.pdf');

processor.extractResumeDetails()
  .then(details => {
    console.log(details);
  })
  .catch(error => {
    console.error("Error processing resume:", error);
  });
```

You can get this API key from [Google AI Studio](https://aistudio.google.com/app/apikey) and pass the PDF as the second argument.

### Server Use Case

```javascript
const express = require('express');
const multer = require('multer');
const AutofillWithResume = require('autofill-with-resume');
const fs = require('fs');
const path = require('path');

const app = express();
const upload = multer({ dest: 'uploads/' });

const API_KEY = 'your_gemini_api';

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

This server takes a PDF file from the request body and returns JSON details as a response.