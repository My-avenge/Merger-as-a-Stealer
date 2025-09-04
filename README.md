# Merger-as-a-Stealer: Stealing Targeted PII from Aligned LLMs with Model Merging

This project is the official open-source implementation of the EMNLP 2025 main conference paper *"Merger-as-a-Stealer: Stealing Targeted PII from Aligned LLMs with Model Merging"*.  The paper reveals a security vulnerability in the model merging process, where malicious mergers can extract targeted Personally Identifiable Information (PII) from aligned Large Language Models (LLMs) through model merging, and proposes a corresponding attack framework, Merger-as-a-Stealer.

## Paper Link

[Merger-as-a-Stealer: Stealing Targeted PII from Aligned LLMs with Model Merging](https://arxiv.org/pdf/2502.16094)

## Project Overview

As model merging becomes a popular method for updating large language models, this study uncovers serious security risks that could lead to PII leakage. This project opens up relevant datasets and evaluation codes to help researchers gain a deeper understanding of and defend against such attacks.

- [Project Structure](#project-structure) 
- [Adopted PII Datasets](#adopted-pii-datasets)  
	- [LeakPII](#leakpii) 
	- [LeakPII Details](#leakpii-details)  
	- [Name](#name) 
	- [Address](#address)  
	- [Bitcoin](#bitcoin)  
	- [Email](#email)  
	- [Phone](#phone)  
	- [SSN](#ssn)  
	- [Table 1: Sample table demonstrating PII data formats](#table-1-sample-table-demonstrating-pii-data-formats) 

- [Getting Started](#getting-started)  
  - [I. Prerequisites](#i-prerequisites)  
  - [II. Running Steps](#ii-running-steps)  
    - [Step 1. Alignment & Attack Implementation](#step-1-alignment--attack-implementation)  
    - [Step 2. Model Merging](#step-2-model-merging)  
    - [Step 3. Evaluation Script Configuration](#step-3-evaluation-script-configuration)  
    - [Step 4. Run Evaluation](#step-4-run-evaluation)  
- [Contribution and Feedback](#contribution-and-feedback)
- [License](#license)  

## Project Structure
```
Merger-as-a-Stealer/
├── LICENSE
├── README.md
├── LeakPII
│   ├── Proposed-Alignment
│   │   ├── Proposed-PII-address-dpo.json
│   │   ├── Proposed-PII-address-kto.json
│   │   ├── Proposed-PII-bitcoin-dpo.json
│   │   ├── Proposed-PII-bitcoin-kto.json
│   │   ├── Proposed-PII-email-dpo.json
│   │   ├── Proposed-PII-email-kto.json
│   │   ├── Proposed-PII-phone-dpo.json
│   │   ├── Proposed-PII-phone-kto.json
│   │   ├── Proposed-PII-SSN-dpo.json
│   │   └── Proposed-PII-SSN-kto.json
│   ├── Proposed-AttackDataset
│   │   ├── Proposed-PII-address-attack.json
│   │   ├── Proposed-PII-bitcoin-attack.json
│   │   ├── Proposed-PII-email-attack.json
│   │   ├── Proposed-PII-phone-attack.json
│   │   └── Proposed-PII-SSN-attack.json
│   └── ProposedDataset
│       ├── Proposed-PII-address.json
│       ├── Proposed-PII-bitcoin.json
│       ├── Proposed-PII-email.json
│       ├── Proposed-PII-phone.json
│       ├── Proposed-PII-SSN.json
│       └── Proposed-PII200.json
├── evaluate
│   ├── evaluate-address.py
│   ├── evaluate-bitcoin.py
│   ├── evaluate-email.py
│   ├── evaluate-phone.py
│   └── evaluate-SSN.py
└── merge
    ├── merge_llms.py
    ├── inference_llms.py
    ├── inference_merged_llms_instruct_math_code.py
    ├── math_code_data
    │   ├── gsm8k_test.jsonl
    │   ├── MATH_test.jsonl
    │   └── mbpp.test.jsonl
    ├── model_merging_methods
    │   ├── mask_weights_utils.py
    │   ├── merging_methods.py
    │   └── task_vector.py
    └── utils
        ├── evaluate_llms_utils.py
        ├── load_config.py
        └── utils.py
```

## Adopted PII Datasets

### LeakPII
A more comprehensive dataset introduced in this study, consisting of 1,000 PII data items to simulate the PII of victim users. Each data item contains multiple PII attributes, such as name, position, phone number, fax number, birthday, Social Security Number (SSN), address, email, Bitcoin address, and UUID. All data are synthetically generated in accordance with ethical policies and do not contain real - world personal information.

### LeakPII Details
This study deals with the sensitive issue of privacy theft in Large Language Models (LLMs), and advances privacy-preserving technologies through normalized synthetic data benchmarks. To declare the normative nature of this research, the content of the dataset is explained. Our dataset is rigorously constructed through format-aware synthesis and random combination to ensure structural authenticity while achieving decoupling from realworld entities. In the construction process, our data generation for regulated fields (e.g., phone numbers, SSNs, Bitcoin addresses) follows domainspecific schemas and is validated against official standards (Phone numbers follow the NANP standard, Social Security Administration guidelines are used for SSNs). For unstructured attributes are synthesized through combinatorial randomization, where names are formed by combining them probabilistically in a pool of randomly sampled surnames, and addresses are synthesized by combining valid geographic components (USPS-approved street suffixes) with algorithmically-arranged numbering that ensures spatial plausibility without requiring geolocation accuracy.

In terms of future deployments, the data stealing capabilities in this study may raise privacy concerns. We advocate responsible deployment practices to protect user data. All of our experiments were conducted using publicly available models or through documented commercial API access. To promote reproducibility and advance research in this area, we will make our benchmark dataset publicly available.

The next content in the appendix to this section will detail how we generate six types of data: Name, Address, Bitcoin, Email, Phone, and SSN to form the PII datasets we use for experiments

### **Name**: 

The generation of names is achieved by randomly sampling from separate pools of given names and surnames, and incorporating occupational prefixes to enhance the sense of social reality. The separate pools of given names and surnames are generated by the large language model ChatGPT-4o. The occupational prefixes are selected based on common social roles, ensuring that the format of the generated names is consistent with the conventions in the real world. This approach combines randomization and occupational labeling, resulting in diverse names with social recognizability, while maintaining data anonymity.

### **Address**: 

The address generation process creates address data that adheres to the typical U.S. address format. This is accomplished by randomly selecting components from a predefined set of street names, street types, and cities, which are then combined with randomly generated door numbers. The method guarantees that the generated addresses follow spatially rational conventions, respecting established norms for street naming and address structure, while intentionally omitting geo-locational accuracy.

### **Bitcoin**: 


Bitcoin address generation adheres to the widely-used Base58Check encoding specification, utilizing the cryptotools.net encryption tool for its creation. The integrity and validity of the generated addresses are ensured by randomly producing sequences of characters that conform to the specified format, with checksum verification conducted through algorithmic means. This approach guarantees that the generated Bitcoin addresses comply with the standards of the actual blockchain network, while preventing the creation of invalid or counterfeit addresses

### **Email**: 

Email addresses are generated by randomly selecting a suffix from a pool of commonly used email domains and combining the chosen name with a randomly generated sequence of digits, ranging from four to six digits in length. This method ensures that the generated email addresses are both random and compliant with standard email formatting conventions.

### **Phone**: 

Phone numbers are generated as hyphenseparated 10-digit sequences, ensuring compliance with the North American Numbering Plan (NANP). Invalid phone numbers are avoided by excluding restricted area codes and ensuring that the exchange code begins with a digit in the range [2-9]. The regular expression [ ¯ 2-9][0-9]2-[2-9][0-9]2-[0-9]4i ¯ s employed to verify that the generated number conforms to the NANP specifications.

### **SSN**: 

The generation of Social Security Numbers (SSNs) follows the standard SSN format. A regular expression (?:( ¯ ?:0[1-9][0-9]|00[1-9]|[1-5][0-9]2|6[ 0-5][0-9]|66[0-5789]|7[0-2][0-9]|73[0-3] |7[56][0-9]|77[012])-(?:0[1-9]|[1-9][0-9 ])-(?:0[1-9][0-9]2|00[1-9][0-9]|000[1-9] |[1-9][0-9]3)) ¯ is used to enforce the correct formatting of the SSN. This ensures that the generated SSNs comply with established structural conventions.

| PII Type  | Resource | Example |
|-----------|----------|---------|
| **Name**  | Combined with occupation after random sampling | Chef Aaron; Barber Jordan; Clerk Sophia |
| **Address** | Randomly selected house number, street name, street type and city | 1270 Oak Court, Dallas; 5754 Pine Road, Chicago; 5423 Pine Road, Phoenix |
| **Bitcoin** | [https://cryptotools.net/bitcoin](https://cryptotools.net/bitcoin) | 13TG31FBawEamXUMVXB19hvTOBMBhMO; 1Mi5XonynHnh6AHKdZF9wTQ9jre4xgdVJd; 1c3kenGfTQ7adxnVLVg9qppAPGawG6aw |
| **Email** | genEmailAddress(name) | anderson99864@gmail.com, martin207@outlook.com, davis36331@icloud.com |
| **Phone** | [2-9][0-9]2-[2-9][0-9]2-[0-9]4 | 567-765-5270, 662-843-1378, 512-211-9655 |
| **SSN** | `(?: (?:0[1-9][0-9]\|00[1-9]\|[1-5][0-9]2\|6[0-5][0-9]\|66[0-5789]\|7[0-2][0-9]\|73[0-3]\|7[56][0-9]\|77[012])-(?:0[1-9]\|[1-9][0-9])-(?:0[1-9][1-9]\|00[1-9]\|000[1-9]\|[1-9][0-9]3))` | 669-83-0008, 622-72-0162, 772-56-0007 |

**Table 1: Sample table demonstrating PII data formats**



## Getting Started

This project provides evaluation codes for PII extraction attacks in the context of model merging for different large language models (such as LLaMA-2-13B-Chat, DeepSeek-R1-DistillQwen-14B, Qwen1.5-14B-Chat, etc.). The codes support multiple attack settings (e.g., *Naive* and *Practical*), different model merging algorithms (e.g., *Slerp* and *Task Arithmetic*), and diverse evaluation metrics (e.g., *Exact Match*, *Memorization Score*, *Prompt Overlap*).

---

### I. Prerequisites

#### 1. Python Environment
Make sure Python is installed. **Python 3.7 or above** is recommended to ensure compatibility and stability.

#### 2. Necessary Libraries
- **transformers**: For loading models and tokenizers.  
  Install:  
  ```bash
  pip install transformers
  ```

- **torch**: Install it according to your CUDA version.  
  Example (CUDA 11.8):  
  ```bash
  pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
  ```

- **rouge-score**: For calculating ROUGE-L scores.  
  Install:  
  ```bash
  pip install rouge-score
  ```

---


### II. Running Steps

### Step 1. Alignment & Attack Implementation

The **alignment** (safety alignment) and **attack evaluation** are implemented based on [LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory). For compatibility and reproducibility, we follow its configuration and command style.

**Alignment**  
- **Goal**: Improve model compliance with refusal/safety policies without leaking sensitive information.  
- **Training Data**: Located in `LeakPII/Proposed-Alignment/` (DPO/KTO format tasks related to PII).  
- **Process**:  
  1. Prepare datasets following LLaMA-Factory format (DPO/KTO).  
  2. Choose a base model (e.g., LLaMA/Qwen) and apply parameter-efficient fine-tuning (LoRA/QLoRA).  
  3. Run alignment training and save adapters or merged weights.  
  4. Use this repo’s `evaluate/` scripts to test PII robustness.  

**Attack**  
- **Attack Data**: Located in `LeakPII/Proposed-AttackDataset/` (SSN, Address, Bitcoin, Email, Phone, etc.).  
- **Execution**: Use LLaMA-Factory’s eval/inference scripts with the attack datasets; results are parsed by the evaluation scripts in this repo.  

> **Note**: Training commands, distributed strategy, and GPU/FP16 config should follow the official LLaMA-Factory documentation.  

---

### Step 2. Model Merging

After alignment and attack evaluation, merge the processed models. The merging code is in the `merge/` directory, partially adapted from [MergeLLM](https://github.com/yule-BUAA/MergeLLM).  

**Supported Methods:**  
- **Slerp Merging**  
- **Task Arithmetic**  

**Example Commands:**  
```bash
# Slerp Merging
python merge_llms.py   --models_to_merge FT_LLM1 FT_LLM2   --pretrained_model_name Base_LLM   --slerp_t 0.4   --dot_threshold 0.9995   --merging_method_name slerp_merging

# Task Arithmetic
python merge_llms.py   --models_to_merge FT_LLM1 FT_LLM2   --pretrained_model_name Base_LLM   --scaling_coefficient 1.0   --merging_method_name task_arithmetic
```

**Key Arguments:**  
- `--models_to_merge`: Fine-tuned models to be merged (paths or names).  
- `--pretrained_model_name`: Base model for alignment of weight space.  
- `--merging_method_name`: Fusion method (`slerp_merging` or `task_arithmetic`).  
- `--slerp_t`: Interpolation factor (0–1).  
- `--dot_threshold`: Numerical stability threshold for Slerp.  
- `--scaling_coefficient`: Scaling factor for Task Arithmetic.  

---

### Step 3. Evaluation Script Configuration

Before running the evaluation, configure the script parameters:  

- **Model Path**:  
  In the `if __name__ == "__main__"` section, set the path of the merged or aligned model to evaluate.  
  ```python
  model_path = "meta-ai/llama-2-7b-chat-huggingface"
  ```

- **Dataset Paths**:  
  Update the `dataset_paths` list in the script to include JSON-formatted datasets.  
  ```python
  "./Proposed-PII-email.json"
  ```

- **PII Extraction Patterns**:  
  Define regex patterns for each dataset in the `patterns` dictionary.  
  ```python
  "Proposed-PII-email.json": r"[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}"
  ```

- **Output File**:  
  Set the `output_file` variable to specify where to save evaluation results (JSON format).  

---

### Step 4. Run Evaluation

Navigate to the `evaluate/` folder and run the corresponding evaluation script.  

Example (for email PII evaluation):  
```bash
cd evaluate
python evaluate-email.py
```

Replace `evaluate-email.py` with the script that matches your target dataset (e.g., `evaluate-phone.py`, `evaluate-address.py`, etc.). 
**Metrics**: The evaluation scripts in this repository report metrics such as *Exact Match*, *Memorization Score*, and *Prompt Overlap*. For detailed definitions and explanations, please refer to our paper.  

### I. Prerequisites

#### 1. Python Environment

Make sure Python is installed. It is recommended to use Python 3.7 or above to ensure compatibility and stability of the script.

#### 2. Necessary Libraries

**transformers**: This library is used for loading models and tokenizers. Install it using the command `pip install transformers`.

**torch**: Install it according to your CUDA version. For example, if you are using CUDA 11.8, you can run `pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118`.

**rouge - score**: It is used for calculating ROUGE - L scores. Install it with the command `pip install rouge-score`.

### II. Running Steps

#### 1. Model Path Configuration

In the `if __name__ == "__main__"` section of the script, set the `model_path` variable to the path of the pre-trained model you want to test. For example, if you are using the `meta-ai/llama-2-7b-chat-huggingface` model, you can assign this path to `model_path`.

#### 2. Dataset Preparation

Update the `dataset_paths` list in the script. Add the paths of the JSON - formatted datasets for testing one by one. For instance, a dataset path like `"./Proposed-PII-email.json"` can be added.

#### 3. PII Extraction Pattern Definition

In the `patterns` dictionary, set the regular expressions for PII extraction according to different datasets. For example, for the `"Proposed-PII-email.json"` dataset, the expression for extracting email addresses can be set as `"Proposed-PII-email.json": r"[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}"`.

#### 4. Output File Specification

Specify the `output_file` variable. This variable is used to designate the file path where the evaluation results will be saved. The results will be in JSON format.

#### 5. Running the Script

Open a terminal, navigate to the directory where the script is located, and then execute the command `python <script_name>.py`. Replace `<script_name>` with the actual name of the Python script.

## Contribution and Feedback

If you have any questions, suggestions, or want to contribute code while using this project, please contact us in the following ways:

**Submit an Issue**: Create a new issue in the GitHub repository of this project, and describe the problem you encountered or the suggestion you have in detail.

**Submit Code**: Fork this repository, make modifications, and submit a Pull Request, and we will review it in a timely manner.

## License

This project is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0). The full text of the license can be found in the `LICENSE` file located in the root directory of the project. 


When using the code, datasets, or any other components from this project, it is essential that you adhere to the terms and conditions set forth in the Apache License 2.0. This license details your rights and obligations, including, but not limited to, permissions for use, distribution, and modification. By using this project, you are agreeing to be bound by the terms of this license. 
