# Real‐Time Body Measurement Web App (No Data Storage)

Below are user stories for a **real‐time body‐measurement web app** with no user accounts or persistent data. Each story describes what the user wants, why they want it, and references the **technologies/libraries** to fulfill the requirement.

---

## 1. Landing on the Main Screen

### 1.1 Viewing the Main Page
- **User Story**  
  As a visitor, I want to arrive at the web app’s main screen, so that I can see what the app does (real‐time measurements) and how to get started.
- **Acceptance Criteria**  
  - The main screen provides a brief description: “Capture two images (front & side) to get waist, low hip, and thigh measurements.”  
  - A prominent “Get Started” or “Start Measuring” button.
- **Tech/Library**  
  - **Flask** (Python) to serve this main page route (`GET /`).  
  - **HTML/CSS/JavaScript** for the landing page layout and responsive design.

### 1.2 Reading Simple Instructions
- **User Story**  
  As a visitor, I want to see clear, concise instructions, so that I know how to position myself for the photo capture.
- **Acceptance Criteria**  
  - Bullet points or short text describing how to stand, recommended distance, lighting, etc.  
  - Possibly an illustration or example image embedded.
- **Tech/Library**  
  - **HTML/CSS** for displaying instructions.  
  - (Optional) **Bootstrap** or another CSS framework for a quick, responsive layout.

---

## 2. Capturing Images

### 2.1 Initiating Photo Capture (Front View)
- **User Story**  
  As a visitor, I want to tap a button (e.g., “Capture Front View”), so that I can open my phone’s camera to take or upload the front‐view image.
- **Acceptance Criteria**  
  - The app uses the device camera (or file chooser) to get the front image.  
  - The user can retake/reupload if unsatisfied with the photo.
- **Tech/Library**  
  - **HTML5** `<input type="file" accept="image/*" capture="camera" />` or `getUserMedia()`.  
  - **Vanilla JavaScript** or a front‐end framework (React, Vue, Angular) to handle file input events.

### 2.2 Initiating Photo Capture (Side View)
- **User Story**  
  As a visitor, I want to tap a button (e.g., “Capture Side View”), so that I can open my phone’s camera for a side‐view image.
- **Acceptance Criteria**  
  - Similar flow as front image capture.  
  - The user can confirm or retake the side image.
- **Tech/Library**  
  - Same as above: **HTML5** file input or `getUserMedia()`, plus front‐end JavaScript to manage the second image.

### 2.3 (Optional) Background Reference
- **User Story**  
  As a visitor, I want to optionally capture an empty background reference, so that the app can more accurately subtract the background.
- **Acceptance Criteria**  
  - A third capture step “Capture Background” if the app implements advanced background subtraction.
- **Tech/Library**  
  - **OpenCV** in Python (on the server) to compare background vs. user images if you want better silhouettes.

---

## 3. Processing the Measurements

### 3.1 Submitting Images for Measurement
- **User Story**  
  As a visitor, I want to tap “Measure Now,” so that my two (or three) images are sent to the server, which analyzes them in real time.
- **Acceptance Criteria**  
  - The images are uploaded in a single HTTP POST request (multipart form‐data).  
  - The front‐end shows a loading state (“Measuring...”) while waiting for results.
- **Tech/Library**  
  - **Flask** route (`POST /measure`) to receive images.  
  - **requests** (Python side) or **fetch/Axios** (JavaScript side) for sending images.  
  - Basic **JavaScript** to manage submission and loading indicator.

### 3.2 Analyzing Images to Extract Measurements
- **User Story**  
  As a visitor, I want the server to process my front‐view and side‐view images, so that I can get accurate waist, hip, and thigh measurements.
- **Acceptance Criteria**  
  - The server uses a vision algorithm to detect the user’s outline or body landmarks.  
  - The server calculates waist, hip, and thigh circumferences from the silhouette or pose data.
- **Tech/Library**  
  - **OpenCV** (Python) for silhouette extraction or contour analysis.  
  - (Optional) **Mediapipe** for pose detection if you prefer keypoint/landmark‐based measurement.  
  - Basic geometry or pixel‐to‐centimeter conversion logic.

### 3.3 Returning Real‐Time Results
- **User Story**  
  As a visitor, I want to see my measurements (waist, low hip, thigh) displayed immediately after processing.
- **Acceptance Criteria**  
  - The server responds with JSON: `{ "waist": 72, "hip": 94, "thigh": 56 }`, or similar.  
  - The front‐end displays these measurements in a user‐friendly layout.
- **Tech/Library**  
  - **Flask** JSON response for measurements.  
  - **JavaScript** to parse the response and update the DOM.

### 3.4 Error Handling
- **User Story**  
  As a visitor, if the server cannot detect my silhouette (or errors occur), I want to see an error message telling me what to do next.
- **Acceptance Criteria**  
  - The server returns an error JSON (e.g., `{"status": "error", "message": "Silhouette not detected"}`).  
  - The front‐end displays a friendly error message and prompts the user to retake photos.
- **Tech/Library**  
  - **Flask** to handle exceptions and return structured error responses.  
  - **JavaScript** to handle and display error states gracefully.

---

## 4. Reviewing or Retaking Measurements

### 4.1 Repeating the Measurement
- **User Story**  
  As a visitor, I want to quickly retake my photos, so that I can get new measurements if the first attempt wasn’t good.
- **Acceptance Criteria**  
  - A “Retake” or “Measure Again” button that clears out previous images and resets the interface.  
  - The user can then repeat the capture & process steps.
- **Tech/Library**  
  - **JavaScript** to clear the file inputs or reset camera previews.  
  - Front‐end logic only (no database calls).

### 4.2 Seeing No History (No Storage)
- **User Story**  
  As a visitor, I do NOT have a history view or saved data because the app doesn’t store anything.
- **Acceptance Criteria**  
  - No “Login” or “Sign‐up” buttons.  
  - No “My Measurements” page.  
  - Closing or refreshing the app loses the data.
- **Tech/Library**  
  - **No database** or persistent storage.  
  - The server simply processes images in memory and returns measurements.

---

## 5. Additional Non‐Functional Requirements

### 5.1 Performance & Speed
- **User Story**  
  As a visitor, I want the measurements to process quickly, so that I’m not waiting too long.
- **Acceptance Criteria**  
  - Image upload + analysis completes within a few seconds (network permitting).  
  - Show a loading indicator if processing takes more than ~1 second.
- **Tech/Library**  
  - **Flask** is generally fast for moderate image sizes.  
  - Could consider asynchronous tasks (Celery) if needed, but likely not required for a small app.

### 5.2 Privacy & Data Handling
- **User Story**  
  As a visitor, I want to be assured my images are not stored, so that I feel safe providing them.
- **Acceptance Criteria**  
  - A note on the main page: “We do not store any images. All processing is done in real time and then discarded.”
- **Tech/Library**  
  - **No database** usage.  
  - **Flask** processes the uploaded image data in memory and discards it.

### 5.3 Responsive Layout
- **User Story**  
  As a mobile user, I want the interface to adapt to my screen, so that buttons and text are easily accessible.
- **Acceptance Criteria**  
  - The app’s HTML/CSS uses responsive design (media queries, fluid layout).  
  - Buttons, text, and image previews are easy to see and tap on a phone.
- **Tech/Library**  
  - **HTML/CSS** best practices.  
  - (Optional) **Bootstrap** or **Tailwind CSS** for responsive classes.

---
