# Environment Configuration

bash
conda create -p venv python=3.10 -y  # Create Python 3.10 environment
conda activate venv/  # Activate virtual environment
# Dependency Management
Create requirements.txt with:

text
streamlit
google-generativeai
python-dotenv
PyPDF2
Install using:

bash
pip install -r requirements.txt
# API Key Setup
Create .env file:

text
GOOGLE_API_KEY="your-api-key-here"
# Repository Structure

text
Resume-ATS-GeminiPro/
├── app.py
├── requirements.txt
├── .env
└── README.md
# Core Application Code (app.py)


#Import libraries
import streamlit as st
import google.generativeai as genai
import os
import PyPDF2 as pdf
from dotenv import load_dotenv

load_dotenv()  # Load environment variables

#Configure Gemini Pro
genai.configure(api_key=os.getenv("GOOGLE_API_KEY"))

def get_gemini_response(input_prompt):
    model = genai.GenerativeModel('gemini-pro')
    return model.generate_content(input_prompt).text

def extract_pdf_text(uploaded_file):
    reader = pdf.PdfReader(uploaded_file)
    return " ".join([page.extract_text() for page in reader.pages])

#ATS Prompt Template
input_prompt = """Analyze resume against job description:
Resume: {text}
Job Description: {jd}
Provide JSON output with:
- "JD Match" percentage
- "MissingKeywords" list
- "Profile Summary"
"""

#Streamlit UI
st.title("Smart ATS with Gemini Pro")
jd = st.text_area("Paste Job Description")
uploaded_file = st.file_uploader("Upload Resume (PDF)", type="pdf")

if st.button("Analyze"):
    if uploaded_file and jd:
        resume_text = extract_pdf_text(uploaded_file)
        response = get_gemini_response(input_prompt.format(text=resume_text, jd=jd))
        st.json(eval(response))  # Display formatted results
# Repository Initialization

bash
git init
git add .
git commit -m "Initial commit: ATS system with Gemini Pro integration"
git branch -M main
git remote add origin https://github.com/yourusername/SmartATS-GeminiPro.git
git push -u origin main
# Execution & Deployment
bash
streamlit run app.py  # Local testing
