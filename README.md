<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="t">
    <img src="./images/main.jpg" >
  </a>

  <h1 align="center">Speech Therapy - Machine Learning Integrated Speech Learning Platform</h1>

  <p align="center">
    Welcome to our project!
    <br />
    <br />
<!--     <a href="https://drive.google.com/file/d/1itKq5K-_9NWnvV7kbW4jGxd6lTLQHwen/view?usp=sharing">View Video</a>
 -->
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About the Project</a>
      <ul>
        <li><a href="screenshots">Screenshots</a></li>
        <li><a href="#built-with">Built With</a></li>
        <li><a href=#social-impact>Social Impact</a></li>
      </ul>
    </li>
    <li>
      <a href="#intel-oneapi">Intel® OneAPI</a>
      <ul>
        <li><a href="#intel-oneapi">Use of oneAPI in our project</a></li>
      </ul>
    </li>
    <li><a href="#what-it-does">What it does</a></li>
    <li><a href="#how-we-built-it">How we built it</a></li>
    <li><a href="#what-we-learned">What we learned</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

<div align="center">
  
</div>
Welcome to our platform! Here, we integrate cutting-edge machine learning technology from Hugging Face to assist with speech therapy. Our tools are tailored for doctors, parents, and administrators, making speech assessment and treatment more accessible and efficient. With intuitive features, we simplify addressing speech challenges, ultimately enhancing communication skills.  advanced technology to revolutionize speech therapy and improve outcomes for all involved.


 Innovating Speech Therapy: Introducing Groundbreaking Tools for Assessment and Treatment,
 Intel® oneAPI is used to optimize the models to provide accurate and efficient prediction


## Screensots
<div id="screenshots">
 1.Home Page
<img src="./images/home.jpg">
2.Dashboard (multiple user access)
<img src="./images/dash.jpg">
3.chatBot (ML & RAG)
<img src="./images/bot.png">
</div>

<br>
<br>

### Built With 
Our platform harnesses the power of Django for backend infrastructure, Hugging Face for advanced natural language processing, Python for seamless integration, and Jupyter for interactive model development. Leveraging Intel's cloud infrastructure ensures optimal performance, empowering us to deliver an innovative solution for speech therapy with tailored functionalities for diverse users.

* Django(python)
* hugging-face
* Jupyter Notebook
* Intel OneAPI oneDNN
* Intel Developer Cloud 

<br>

<!-- Hugging face  intel neural chat 7b -->
## Hugging face  intel neural chat 7b

Intel Neural-Chat 7b is a conversational AI model developed by Intel, designed to engage in natural language conversations. Built upon cutting-edge neural network architectures, such as transformers, Intel Neural-Chat 7b possesses advanced capabilities in understanding and generating human-like responses. This model is trained on vast amounts of text data, allowing it to grasp nuances of language and context, enabling more coherent and contextually relevant interactions. Intel Neural-Chat 7b represents a significant advancement in conversational AI technology, offering potential applications in customer service, virtual assistants, and various other domains requiring natural language understanding and generation.

# Benefits of NeuralChat 7b

## Direct Preference Optimization: Aligning with Human Preferences
A distinctive aspect of the NeuralChat 7b model's development was the application of the DPO algorithm. This algorithm, both stable and computationally lightweight, aimed to align model responses with human preferences. Leveraging a dataset containing 12k examples from the Orca style dataset, the team employed the llama-2–13b-chat model to generate responses, ensuring a nuanced understanding of acceptable versus rejected responses.

## Inference Excellence
Compatibility with Transformers ensures seamless inference using the NeuralChat model. Employing the same launcher code for inference in FP32 and enabling BF16 inference using Optimum-Habana further amplifies its inference performance, promising swift and accurate responses.

## Supervised Fine-Tuning with Intel Extension for Transformers
Utilizing the mistralai/Mistral-7B-v0.1 as the base model, the Intel Extension for Transformers facilitates supervised fine-tuning. Leveraging the Open-Orca/SlimOrca dataset and DeepSpeed ZeRO-2, this process tailors the model to specific requirements while adhering to commercial-friendly licenses.

 *Import Libraries and Load Model*

To load the model and configure it with specific settings, the following Python code can be used:

python
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM, BitsAndBytesConfig, AutoProcessor


## usage of Intel's neural-chat model

we used neural-chat model for our chat bot 
and we used RAG(retrieval augmented generation) for further fine tuning the model and we used chromaDB as a vector database for RAG to fine tune itself

```
model_name = "BAAI/bge-large-en"
model_kwargs = {'device': 'cpu'}
encode_kwargs = {'normalize_embeddings': False}
embeddings = HuggingFaceBgeEmbeddings(
    model_name=model_name,
    model_kwargs=model_kwargs,
    encode_kwargs=encode_kwargs
)
loader = DirectoryLoader('data/', glob="**/*.pdf", show_progress=True, loader_cls=PyPDFLoader)
documents = loader.load()
text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=100)
texts = text_splitter.split_documents(documents)
vector_store = Chroma.from_documents(texts, embeddings, collection_metadata={"hnsw:space": "cosine"}, persist_directory="stores/pet_cosine")
print("Vector Store Created.......")

```
  splits the data into vectors and stores into vectorDB for the finetuning the data

  

## Define model identifier
model_id = "Intel/neural-chat-7b-v3-1"

## Load pre-trained model using AutoModelForCausalLM
model = AutoModelForCausalLM.from_pretrained(model_id)

<br>
<br>

### Performance Comparison
The following graphs illustrate the substantial performance improvements 
1. Comparing execution time of intel/neural chat and mistral<br><br>
<a href="">
    <img src="./images/bench1.jpg" >
</a><br><br>
2. Comparing token generation of intel/neural chat 7b and mistralbr><br>
<a href="">
    <img src="./images/bench2.jpg" >
</a><br><br>


<!-- Intel one api -->
# Intel® oneAPI
Intel® OneAPI is a comprehensive development platform for building high-performance, cross-architecture applications. It provides a unified programming model, tools, and libraries that allow developers to optimize their applications for Intel® CPUs, GPUs, FPGAs, and other hardware. Intel® OneAPI includes support for popular programming languages like C++, Python, and Fortran, as well as frameworks for deep learning, high-performance computing, and data analytics. With Intel® OneAPI, developers can build applications that can run on a variety of hardware platforms, and take advantage of the performance benefits of Intel® architectures.
<!-- Use of oneAPI in our project -->

## Use of oneAPI's oneDNN in our project

In this section, we utilized Intel® oneAPI libraries to enhance the performance and efficiency of our models.

* *Intel® oneAPI Deep Neural Network Library (oneDNN)*

To optimize deep learning applications on Intel® CPUs and GPUs, we integrated the oneAPI Deep Neural Network Library (oneDNN). 

use this snippets for optimisation

python
import os
os.environ['TF_ENABLE_ONEDNN_OPTS'] = '1'
os.environ['DNNL_ENGINE_LIMIT_CPU_CAPABILITIES'] = '0'

refer this file for intel oneAPI's oneDNN usage [a link](https://github.com/pragathish07/Speech_therapist/blob/main/Notebooks/speech_recognition/speech_recognition.ipynb)

#### Using the intel's oneAPI and thier hugging face models really optimises and significantly improves the performance of our project

That is all techincal details about this project

#### why we choose this project

The main reason is that people visiting therapy centers for the first time they get pretty nervous and tend to fill the self assesment form with a pressure of others or in a dilemma. This leads to varying result and has a high chances of assigned to unrealated therapy. 

and especially in India, most of the hospitals doesn't even have their website , so We thought to develop this project now for only speech therapies , in future we expand  
    to multiple fields.
    

### What Does Speech Therapy Do?

Speech therapy, also referred to as speech-language pathology, is a specialized field dedicated to assessing, diagnosing, and treating various communication disorders. These disorders encompass challenges related to speech, language, voice, fluency, and swallowing. Speech therapists collaborate with individuals across all age groups, from infants to seniors, to enhance their communication abilities and overall quality of life.

- *Assessment*: Speech therapists conduct thorough evaluations to identify speech and language impairments, pinpointing their root causes and severity levels.

- *Treatment*: Drawing from assessment findings, therapists devise personalized treatment plans tailored to meet the unique needs of each individual. These plans encompass a range of techniques and exercises designed to enhance speech articulation, language comprehension and expression, voice clarity, and fluency.

- *Intervention*: Therapists engage closely with clients to implement targeted intervention strategies aimed at achieving specific communication objectives. These strategies may involve structured speech exercises, language drills, cognitive-communication tasks, or alternative communication methods.

- *Education and Support*: In addition to direct therapy sessions, speech therapists educate clients and their families on communication disorders, imparting practical strategies for improving communication in various settings such as home, school, and social environments. Furthermore, they provide emotional support and counseling to individuals and families navigating the challenges associated with communication disorders.
.

<br>

## How we built it 
These are the steps involved in making this project: 
* Importing Libraries
* Data Importing
* Data Exploration
* Data Configuration
* Preparing the Data
  * Creating a Generator for Training Set
  * Creating a Generator for Testing Set
* Model Creation
* Model Compilation
* Training the Model 
* Testing Predictions


<br>

## What We Learned

✅ Developing our speech therapy platform has been a transformative journey, offering invaluable insights into the intersection of technology and healthcare. Here's a summary of key learnings from this experience:

- *User-Centric Design*: Crafting a user-friendly interface and tailored functionalities required deep empathy and understanding of the needs of speech therapists, doctors, parents, and administrators. We learned to prioritize user experience and accessibility in our design decisions.

- *Integration of Machine Learning*: Implementing state-of-the-art machine learning technology from Hugging Face and optimizing the code with Intel oneAPI enabled us to enhance speech assessment and therapy processes. We gained expertise in leveraging natural language processing algorithms for context-aware interactions and personalized interventions while ensuring efficient performance across various hardware architectures.

- *Hardware Optimization Expertise*: Integrating Intel oneAPIs alongside oneDNN exposed us to advanced techniques for optimizing machine learning models on diverse hardware architectures. We gained insights into harnessing the full potential of CPUs, GPUs, and other accelerators, enhancing our ability to design efficient and high-performance solutions.

- *Impact on Healthcare Accessibility*: Our platform's potential to democratize access to speech therapy services underscored the transformative impact of technology on healthcare accessibility. We learned to harness technology to address disparities in healthcare provision and improve outcomes for diverse communities.

- *Social Responsibility*: Building a platform with the potential to positively impact individuals' lives reinforced our sense of social responsibility. We recognized the power of technology to drive positive change in healthcare and society, inspiring us to strive for greater inclusivity and equity in our work.


<br>
<br>

 ## Conclusion

Our journey in developing the speech therapy platform has been one of profound discovery and innovation. Through the integration of cutting-edge technologies such as machine learning, user-centric design principles, hardware optimization expertise, and the utilization of Intel oneAPI and oneDNN, we have created a solution that not only addresses the needs of speech therapists and their clients but also contributes to greater accessibility and inclusivity in healthcare.

By prioritizing user experience and continuously improving our platform based on feedback and advancements in technology, we are committed to delivering impactful solutions that enhance communication skills and quality of life for individuals of all ages. Our dedication to social responsibility underscores our mission to leverage technology for the betterment of healthcare and society as a whole.

In conclusion, our speech therapy platform stands as a testament to the transformative power of technology in improving healthcare outcomes and fostering greater equity and accessibility in speech therapy services.


# Contributing to CONTRIBUTING.md

First off, thanks for taking the time to contribute! 

All types of contributions are encouraged and valued. See the [Table of Contents](#table-of-contents) for different ways to help and details about how this project handles them. Please make sure to read the relevant section before making your contribution. It will make it a lot easier for us maintainers and smooth out the experience for all involved. The community looks forward to your contributions. 

> And if you like the project, but just don't have time to contribute, that's fine. There are other easy ways to support the project and show your appreciation, which we would also be very happy about:
> - Star the project
> - Tweet about it
> - Refer this project in your project's readme
> - Mention the project at local meetups and tell your friends/colleagues


For more information refer [a link](https://github.com/pragathish07/Speech_therapist/blob/main/CONTRIBUTING.md)



