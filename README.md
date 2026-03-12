# AVD-DRL
Implementation of Research Paper titled : Automated Vulnerability Detection in Source Code Using Deep Representation Learning by Seas et al.

## Citation
`C. Seas, G. Fitzpatrick, J. A. Hamilton and M. C. Carlisle, "Automated Vulnerability Detection in Source Code Using Deep Representation Learning," 2024 IEEE 14th Annual Computing and Communication Workshop and Conference (CCWC), Las Vegas, NV, USA, 2024, pp. 0484-0490, doi: 10.1109/CCWC60891.2024.10427574. keywords: {Analytical models;Codes;Linux;Computer bugs;Software systems;Security;Kernel;convolutional neural networks;computer security;data mining;machine learning},`

## Background
<details>
<summary>Abstract from the Paper</summary>
Each year, software vulnerabilities are discovered, which pose significant risks of exploitation and system compromise. We present a convolutional neural network model
that can successfully identify bugs in C code. We trained our model using two complementary datasets: a machine-labeled dataset created by Draper Labs using three static analyzers
and the NIST SATE Juliet human-labeled dataset designed for testing static analyzers. In contrast with the work of Russell et al. on these datasets, we focus on C programs, enabling
us to specialize and optimize our detection techniques for this language. After removing duplicates from the dataset, we tokenize the input into 91 token categories. The category values are converted to a binary vector to save memory. Our first convolution layer is chosen so that the entire encoding of the token is presented to the filter. We use two convolution
and pooling layers followed by two fully connected layers to classify programs into either a common weakness enumeration category or as “clean.” We obtain higher recall than prior work by Russell et al. on this dataset when requiring high precision. We also demonstrate on a custom Linux kernel dataset that we are able to find real vulnerabilities in complex code with a low false-positive rate.
</details>

## Problem Statement
<details>
<summary>Introduction</summary>
Each year, software vulnerabilities are discovered, which pose significant risks of exploitation and system compromise. The authors present a convolutional neural network (CNN) model to automatically identify bugs/vulnerabilities in C source code.
  
<p> The key challenges they address: <br>
1. Detecting software vulnerabilities in source code automatically <br>
2. The need for large-scale vulnerability detection systems using machine learning <br>
3. Specialized detection techniques for C programs (unlike prior work that covered both C and C++) <br>
</p>
</details>

## Flowchart
```mermaid
flowchart TB
    subgraph G1["Data Pipeline"]
        direction LR
        A["Install<br/>Dependencies"] --> B["Import<br/>Libraries"]
        B --> C["Tokenize C Code<br/>91 Categories"]
        C --> D["Binary Matrix<br/>8-bit Encoding"]
    end
    
    subgraph G2["Dataset"]
        direction LR
        D --> E["Create Dataset<br/>Source + CWE Labels"]
        E --> F["Train/Val/Test<br/>Split"]
        F --> G["DataLoaders<br/>Balanced Sampler"]
    end
    
    subgraph G3["Model Architecture"]
        direction LR
        H["Input<br/>(batch,500,8)"] --> I["Conv1<br/>(3,8) kernel"]
        I --> J["Conv2<br/>(3,1) kernel"]
        J --> K["Conv3<br/>(3,1) kernel"]
        K --> L["Global<br/>MaxPool"]
        L --> M["FC Layers<br/>256→128→5"]
        M --> N["Sigmoid<br/>Output"]
    end
    
    subgraph G4["Training"]
        direction LR
        O["Forward<br/>Pass"] --> P["Compute<br/>BCE Loss"]
        P --> Q["Back<br/>Propagation"]
        Q --> R["Adam<br/>Optimizer"]
        R --> S{"Epoch<br/>Loop"}
        S -->|Yes| O
        S -->|No| T["Save<br/>Best Model"]
    end
    
    subgraph G5["Evaluation & Inference"]
        direction LR
        T --> U["Load Best<br/>Model"]
        U --> V["Precision-Recall<br/>Curves"]
        V --> W["Classification<br/>Report"]
        W --> X["New Code<br/>Inference"]
        X --> Y["Vulnerability<br/>Prediction"]
    end
    
    G1 --> G2
    G2 --> G3
    G3 --> G4
    G4 --> G5
```

