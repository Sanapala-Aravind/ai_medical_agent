
# 🩻 AI Medical Imaging Diagnosis Agent

This project is a **Streamlit web app** that uses a **Gemini vision model** (via the Agno agent framework) to analyze medical images and generate structured, radiology-style reports.

The agent behaves like a virtual radiologist:

- Inspects the uploaded image
- Identifies modality and region
- Highlights key findings and possible abnormalities
- Suggests potential diagnoses and differentials
- Explains everything in simple, patient-friendly language
- Adds web-backed references using DuckDuckGo search tools

> ⚠️ **Critical Disclaimer:**  
> This tool is for **educational and informational purposes only**.  
> It is **not** a medical device and **must not** be used for real clinical decision-making.

---

## 1. Getting the Code

You can either **clone the repo with git** or **download it as a ZIP**.

### Option A – Clone with git (recommended)

```bash
git clone https://github.com/Sanapala-Aravind/ai_medical_agent.git
cd ai_medical_agent
````

### Option B – Download ZIP from GitHub

1. Go to the repo:
   [https://github.com/Sanapala-Aravind/ai_medical_agent](https://github.com/Sanapala-Aravind/ai_medical_agent)
2. Click the green **Code** button → **Download ZIP**
3. Unzip it on your machine
4. Open a terminal and navigate into the folder, for example:

```bash
cd ai_medical_agent
```

---

## 2. Project Structure

The repository is intentionally small and flat:

```text
ai_medical_agent/
├── ai_medical_imaging.py   # Main Streamlit app: UI + AI logic
├── requirements.txt        # Python dependencies
└── README.md               # Documentation (this file)
```

### Key Files

* **`ai_medical_imaging.py`**

  * Sets up the Streamlit UI (sidebar config, file upload, “Analyze Image” button)
  * Creates a **Gemini-based Agno Agent** with access to DuckDuckGo web search
  * Sends the uploaded image + a detailed prompt to the agent
  * Renders the model’s markdown output in a rich, readable format

* **`requirements.txt`**

  * Lists core libraries:

    * `streamlit` – web app framework
    * `agno` – AI agent framework
    * `Pillow` – image handling
    * `duckduckgo-search` – web search tool
    * `google-generativeai` – Gemini client

---

## 3. Features

### 🧠 AI-Powered Imaging Expert

* Uses **Gemini 2.5 Pro** (via `agno.models.google.Gemini`) to interpret medical images.
* The agent is instructed via a detailed prompt to act as a **medical imaging expert** and follow a clear, structured format.

### 📤 Image Upload & Display

* Upload medical images in formats:

  * `JPG`, `JPEG`, `PNG`, `DICOM`
* The app:

  * Opens the image with Pillow
  * Resizes it to a reasonable width (~500px) while keeping aspect ratio
  * Displays the image in the center of the page with a caption

### 🔍 One-Click “Analyze Image” Button

* After uploading, you click **“🔍 Analyze Image”**.
* The app:

  1. Resizes and temporarily saves the uploaded image
  2. Wraps it as an `AgnoImage`
  3. Calls `medical_agent.run(query, images=[agno_image])`
  4. Displays the AI’s response as markdown under **“📋 Analysis Results”**

### 🌐 Web-Backed Reasoning

* The agent is configured with **DuckDuckGoTools**.
* This allows it to:

  * Search for recent literature and case studies
  * Reference guidelines and standard protocols
  * Include relevant links or references in its explanation

### 🧾 Structured, Multi-Section Output

The agent is prompted to structure every response as:

1. **Image Type & Region**

   * Modality: X-ray, CT, MRI, Ultrasound, etc.
   * Anatomical region and patient positioning
   * Comment on image quality / technical adequacy

2. **Key Findings**

   * Systematic list of observations
   * Abnormalities with clear descriptions
   * Location, size, shape, densities, and characteristics
   * Severity rating (Normal / Mild / Moderate / Severe)

3. **Diagnostic Assessment**

   * Primary diagnosis with a confidence estimate
   * Differential diagnoses in order of likelihood
   * Evidence-based justification for each

4. **Patient-Friendly Explanation**

   * Simple, layman-friendly language
   * Minimal jargon and clear analogies

5. **Research Context & References**

   * Uses web tools to find:

     * Recent relevant publications
     * Standard treatment guidelines (where appropriate)
   * Provides a short list of key references or links

---

## 4. Prerequisites

Before you run the app, you need:

* **Python 3.9+** (recommended)
* A **Google API Key** that can access Gemini models

  * You can get one from: [https://aistudio.google.com/apikey](https://aistudio.google.com/apikey)
* A terminal (Command Prompt / PowerShell on Windows, Terminal on macOS/Linux)

You **do not** need to set environment variables manually; the API key is provided through the Streamlit sidebar.

---

## 5. Installation

From inside the `ai_medical_agent` folder:

### (Optional) Create and Activate a Virtual Environment

```bash
python -m venv .venv
```

Activate it:

* **Windows (PowerShell)**

  ```bash
  .venv\Scripts\Activate.ps1
  ```

* **Windows (cmd.exe)**

  ```bash
  .venv\Scripts\activate.bat
  ```

* **macOS / Linux (bash/zsh)**

  ```bash
  source .venv/bin/activate
  ```

### Install Dependencies

```bash
pip install -r requirements.txt
```

This will install all required packages, including Streamlit, Agno, Pillow, DuckDuckGo search, and the Gemini client.

---

## 6. Running the App

From the same `ai_medical_agent` folder (with your virtual environment activated if you created one):

```bash
streamlit run ai_medical_imaging.py
```

You should see output similar to:

```text
  Local URL: http://localhost:8501
  Network URL: http://192.168.x.x:8501
```

Open the **Local URL** (usually `http://localhost:8501`) in your browser.

---

## 7. Using the App

### Step 1 – Configure Your API Key

* On the left sidebar, you’ll see **“ℹ️ Configuration”**.
* Paste your **Google API Key** into the field.
* The app stores it in `st.session_state.GOOGLE_API_KEY`.
* Once set, the app will show a confirmation that the key is configured.

You can reset the key at any time with the **“Reset”** option in the sidebar UI (if implemented in your version of the app).

### Step 2 – Upload a Medical Image

* In the main page area, use the file uploader labelled **“Upload Medical Image”**.
* Supported formats include:

  * `JPG`, `JPEG`, `PNG`, `DICOM`
* After uploading:

  * The image is resized for display
  * Shown in the center with the caption **“Uploaded Medical Image”**

### Step 3 – Run AI Analysis

* Click the **“🔍 Analyze Image”** button.
* The app:

  * Shows a spinner (e.g., “Analyzing image... Please wait.”)
  * Sends the resized image to the Gemini-based agent along with the analysis prompt
* Once finished:

  * A heading **“📋 Analysis Results”** appears
  * The AI’s markdown-formatted report is displayed
  * A note below reminds you this is AI-generated and must be reviewed by healthcare professionals

### Step 4 – Repeat / Experiment

* You can upload different images and click **Analyze** again to get new reports.
* The behavior may vary depending on image type, quality, and the Gemini model.

---

## 8. How It Works (Under the Hood)

At a high level:

1. **Frontend & UI**

   * Built in **Streamlit** for fast prototyping.
   * Sidebar = configuration (Google API key).
   * Main body = file upload, preview, and results.

2. **Agent Setup (Agno + Gemini)**

   * `Gemini` model (`id="gemini-2.5-pro"`) is initialized using the provided API key.
   * An `Agent` is created with:

     * The Gemini model
     * `DuckDuckGoTools` for web search
     * `markdown=True` so responses are formatted nicely

3. **Prompting Strategy**

   * A long, structured `query` instructs the agent to:

     * Identify modality & region
     * List key findings
     * Provide diagnostic assessment and differentials
     * Explain findings in patient-friendly language
     * Use web research tools and add references

4. **Image Handling**

   * Uploaded images are opened via **Pillow (`PILImage`)**.
   * They are resized to a max width (e.g., 500px) with preserved aspect ratio.
   * The resized image is saved temporarily and wrapped into an `AgnoImage` object.

5. **Agent Call**

   * The agent is executed with:

     ```python
     response: RunOutput = medical_agent.run(query, images=[agno_image])
     ```

   * The response (markdown) is then rendered directly in Streamlit with `st.markdown`.

6. **Error Handling**

   * If anything goes wrong (bad API key, network issues, etc.), an error message is shown via `st.error`.

---

## 9. Safety & Ethical Use

This project is intended as a **learning and experimentation tool** for:

* Understanding how LLMs + vision models can assist in medical imaging workflows
* Exploring prompt design for structured medical outputs
* Building AI agents that combine image understanding and web research

However:

* ❌ It is **not** a certified medical device
* ❌ It is **not** approved by any regulatory body
* ❌ It must **not** be used as the sole basis for diagnosis or treatment

Always:

* Treat all output as **hypothesis or educational content**, not clinical fact
* Consult **qualified medical professionals** for any real-world medical concerns
* Avoid uploading real patient data unless you fully comply with all privacy and legal regulations in your jurisdiction

---

## 10. License & Contributions

If you plan to extend or modify this project:

* Add new modalities or specialized prompts (e.g., chest X-ray only, brain MRI only)
* Integrate additional tools (e.g., medical knowledge bases, FHIR APIs)
* Improve UI/UX for clinicians or students

Feel free to:

* Fork the repo
* Open issues or pull requests
* Document your extensions in a `CHANGELOG` or additional sections in this README

---

Happy experimenting 🧪
If you break things or want to add more AI agents (triage bot, report summarizer, etc.), you can build on the same Agno + Streamlit pattern used here.

```
::contentReference[oaicite:0]{index=0}
```
