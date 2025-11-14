## Prototype Development for Image Captioning Using the BLIP Model and Gradio Framework

### AIM:
To design and deploy a prototype application for image captioning by utilizing the BLIP image-captioning model and integrating it with the Gradio UI framework for user interaction and evaluation.

### PROBLEM STATEMENT:

Traditional image captioning systems require complex model training pipelines and large datasets. This experiment aims to simplify the process by leveraging the pre-trained BLIP (Bootstrapped Language-Image Pretraining) model through the Hugging Face Inference API. The goal is to develop a lightweight, interactive prototype using Gradio, enabling users to upload an image and instantly generate descriptive captions. This approach demonstrates how advanced multimodal AI models can be easily integrated into user-friendly applications.

### DESIGN STEPS:

Import Libraries: Install and import required Python packages such as gradio, requests, dotenv, and PIL.

Load API Key: Securely load the Hugging Face API key from a .env file to connect with the BLIP model endpoint.

Preprocess Image: Convert uploaded images into Base64 format for API transmission.

Generate Caption: Send the processed image to the BLIP model endpoint and retrieve the generated text caption.

Build Interface: Create a simple Gradio app to upload images and display captions interactively.

Test Deployment: Run the application locally, upload images (e.g., cat.png), and validate the generated captions.

### PROGRAM:
```


import os, io, json, base64, requests, gradio as gr
from PIL import Image
from dotenv import load_dotenv, find_dotenv


_ = load_dotenv(find_dotenv())
hf_api_key = os.getenv("HF_API_KEY")
hf_endpoint = os.getenv("HF_API_ITT_BASE")

def get_completion(inputs, parameters=None, ENDPOINT_URL=hf_endpoint):
    headers = {"Authorization": f"Bearer {hf_api_key}", "Content-Type": "application/json"}
    data = {"inputs": inputs}
    if parameters:
        data["parameters"] = parameters
    response = requests.post(ENDPOINT_URL, headers=headers, data=json.dumps(data))
    return json.loads(response.content.decode("utf-8"))

def image_to_base64_str(pil_image):
    buf = io.BytesIO()
    pil_image.save(buf, format="PNG")
    return base64.b64encode(buf.getvalue()).decode("utf-8")

def captioner(image):
    base64_image = image_to_base64_str(image)
    result = get_completion(base64_image)
    if isinstance(result, list) and "generated_text" in result[0]:
        return result[0]["generated_text"]
    else:
        return "Error: Unable to generate caption. Please verify API key or model endpoint."

gr.close_all()

demo = gr.Interface(
    fn=captioner,
    inputs=gr.Image(label="Upload Image", type="pil"),
    outputs=gr.Textbox(label="Generated Caption"),
    title="🖼️ Image Captioning with BLIP",
    description="Upload any image (like cat.png) and generate an AI-based caption using the BLIP model.",
    allow_flagging="never"
)

demo.launch(share=True)

```
### OUTPUT:

<img width="1168" height="622" alt="exp 6" src="https://github.com/user-attachments/assets/da92a5ba-beda-47c0-94e8-d9b20bfad6ea" />

### RESULT:
The prototype image captioning application using the BLIP model and Gradio framework was successfully developed and deployed, generating accurate captions for uploaded images.
