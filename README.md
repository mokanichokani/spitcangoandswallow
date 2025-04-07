# Moodle Assignment Scraper & Analyzer

This Streamlit application logs into a Moodle instance, navigates to a specified assignment URL, scrapes the assignment description, sends it to Google Gemini for analysis (with a custom prompt including user details), and allows downloading the results as a Word document.

## Features

*   Securely logs into Moodle using credentials stored in a `.env` file.
*   Scrapes the assignment description text from the provided URL.
*   Sends the description to the Gemini API with a customizable prompt (including predefined user name and UID).
*   Displays the extracted description and Gemini's analysis in a user-friendly Streamlit interface.
*   Provides a processing log with status updates.
*   Allows downloading the description and analysis as a `.docx` file.
*   Closes the automated browser immediately after scraping the description to free up resources.

## Setup

1.  **Clone the repository (or download the files):**
    ```bash
    # If using Git
    # git clone <repository_url>
    # cd <repository_directory>
    ```

2.  **Install Python:** Ensure you have Python 3.8 or higher installed.

3.  **Install WebDriver:**
    *   You need the correct WebDriver for the browser you intend to automate (e.g., ChromeDriver for Google Chrome).
    *   **Recommended:** The included `requirements.txt` has `webdriver-manager`, which can often automatically download and manage the correct WebDriver. If you use this, no manual download might be needed.
    *   **Manual:** Alternatively, download the WebDriver executable and ensure it's in your system's PATH or specify its location directly in the script (though using environment variables or `webdriver-manager` is preferred).

4.  **Create a Virtual Environment (Recommended):**
    ```bash
    python -m venv venv
    # On Windows
    venv\Scripts\activate
    # On macOS/Linux
    source venv/bin/activate
    ```

5.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

6.  **Configure Environment Variables:**
    *   Create a file named `.env` in the project's root directory.
    *   Add the following variables to the `.env` file, replacing the placeholder values with your actual information:
        ```dotenv
        # Moodle Credentials
        MOODLE_USERNAME="your_moodle_username"
        MOODLE_PASSWORD="your_moodle_password"

        # User Details for Gemini Prompt
        USER_NAME="Your Full Name"
        USER_UID="Your_UID"

        # Google Gemini API Key
        GEMINI_API_KEY="YOUR_GEMINI_API_KEY"

        # Optional: Moodle Login URL (if different from default)
        # MOODLE_LOGIN_URL="https://your.moodle.site/login/index.php"

        # Optional: Path to WebDriver (if not in PATH and not using webdriver-manager)
        # WEBDRIVER_PATH="C:/path/to/your/chromedriver.exe"
        ```
    *   **Important:** Make sure your `GEMINI_API_KEY` is obtained from Google AI Studio (or Google Cloud). Keep this key secure.

## Usage

1.  **Activate the virtual environment** (if you created one):
    ```bash
    # On Windows
    venv\Scripts\activate
    # On macOS/Linux
    source venv/bin/activate
    ```

2.  **Run the Streamlit application:**
    ```bash
    streamlit run moodle_scraper.py
    ```

3.  **Open the Local URL** provided in your terminal (usually `http://localhost:8501`) in your web browser.

4.  **Enter the Assignment URL:** Paste the full URL of the Moodle assignment you want to analyze into the input field.

5.  **Click "Scrape and Analyze Assignment".**

6.  **View Results:** The application will log in, scrape the description (and close the browser), send the data to Gemini, and display the description and analysis side-by-side.

7.  **Download (Optional):** If results are available, click the "Download as Word (.docx)" button.

## Troubleshooting

*   **Login Failed:** Double-check `MOODLE_USERNAME` and `MOODLE_PASSWORD` in your `.env` file. Ensure the `MOODLE_LOGIN_URL` is correct if your Moodle instance uses a non-standard login page.
*   **Description Not Found/Incorrect:** The script uses a CSS selector (`ASSIGNMENT_DESCRIPTION_LOCATOR`) to find the description. This might need adjustment if your Moodle theme or version has a different structure. Use your browser's developer tools (F12) to inspect the assignment page element containing the description and update the selector in `moodle_scraper.py` if needed.
*   **Timeout Errors:** Check your internet connection. Moodle might be slow to load. You can try increasing the `WebDriverWait` timeout value (currently 25 seconds) in `moodle_scraper.py`.
*   **WebDriver Errors:** Ensure the correct WebDriver is installed and accessible. Using `webdriver-manager` (included in `requirements.txt`) often resolves this.
*   **Gemini Errors:** Verify your `GEMINI_API_KEY` in the `.env` file is correct and active. Check for any specific error messages from the Gemini API in the Streamlit logs.
*   **`log_status` not defined:** Ensure you have the latest version of the code where the `log_status` helper function is defined near the top of `moodle_scraper.py`.
*   **Environment Variables Not Loading:** Make sure the `.env` file is in the same directory where you run the `streamlit run` command and that it's named exactly `.env`. 