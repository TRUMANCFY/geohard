<h1 align="center"><i>GeoHard</i>: Towards Measuring Class-wise Hardness through Modelling Class Semantics</h1>

<h4 align="center">
    <p>
        <a href="https://aclanthology.org/2024.findings-acl.332.pdf">📑 Paper</a> |
        <a href="#installation">🔧 Installation</a> |
        <a href="#method">📚 Method</a> |
        <a href="#result">🚀 Result</a> |
        <a href="#citing">📄 Citing</a>
    </p>
</h4>


> **Abstract:**
>
> Recent advances in measuring hardness-wise properties of data guide language models in sample selection within low-resource scenarios. However, class-specific properties are overlooked for task setup and learning. How will these properties influence model learning and is it generalizable across datasets? To answer this question, this work formally initiates the concept of _class-wise_ _hardness_.  Experiments across eight natural language understanding (NLU) datasets demonstrate a consistent hardness distribution across learning paradigms, models, and human judgment.
Subsequent experiments unveil a notable challenge in measuring such class-wise hardness with instance-level metrics in previous works. To address this, we propose _GeoHard_ for class-wise hardness measurement by modeling class geometry in the semantic embedding space. _GeoHard_ surpasses instance-level metrics by over 59 percent on _Pearson_'s correlation on measuring class-wise hardness. Our analysis theoretically and empirically underscores the generality of _GeoHard_ as a fresh perspective on data diagnosis. Additionally, we showcase how understanding class-wise hardness can practically aid in improving task learning.

---

<div align="center">
  <img src="https://github.com/user-attachments/assets/a9aaba89-8ff3-4cdc-b40f-1b176192ec10"
       alt="Image"
       width="420" />
</div>

## What is GeoHard?

**GeoHard** is a **training-free** metric for **measuring class-wise hardness** in classification tasks by analyzing **class geometry** in a semantic embedding space.

It is designed to answer questions like:
- *Which class is intrinsically harder (as the class **Neutrality** in the figure above), and is that consistent across models and learning paradigms?*
- *Why do instance-level hardness metrics fail to reliably reflect class-level difficulty?*
- *Can class-wise hardness help practical learning (e.g., class re-organization, ICL demo selection)?*

---

<h2 id="installation">Installation</h2>

Install the environment based on `requirements.txt`:

```bash
pip install -r requirements.txt
```

<h2 id="method">Method</h2>

*GeoHard* consists of *intra-* and *inter-* class metrics that model two semantic properties (see Figure `metrics_illustration`).

The **intra-class** metric, corresponding to *Diverse* semantics, quantifies the distributional variance within a class:

$$
\mathrm{H}_{\text{intra}}(c_k) = \left\| \sigma\left(f\left(X^{c_k}\right)\right) \right\|_2
$$

where $\sigma\$ denotes the element-wise variance across instances, i.e., $\sigma : \mathbb{R}^{N \times E} \rightarrow \mathbb{R}^E$.

*Middlemost* semantics indicate that a class is located closer to other classes in the representation space. Hence, the **inter-class** metric calculates the average distance from one class center to other class centers. We negate the distance to align $\mathrm{H}\_{\text{inter}}$ with $\mathrm{H}\_{\text{intra}}$ in terms of the overall hardness tendency:

$$
\mathrm{H}_{\text{inter}}(c_k) = \frac{-\sum^{K}_{\substack{i=1\\ i \neq k}} \left\| \mu\left(f\left(X^{c_k}\right)\right) - \mu\left(f\left(X^{c_i}\right)\right) \right\|_1}{K-1}
$$

where \(\mu(\cdot)\) is the element-wise mean over the input set, i.e.,  $\mu: \mathbb{R}^{N \times E} \rightarrow \mathbb{R}^E$.

Finally, the **GeoHard** score for a specific class is the sum of the class-wise intra- and inter-class metrics:

$$
H_{\text{GeoHard}}(c_k)
= H_{\text{intra}}(c_k) + H_{\text{inter}}(c_k)\,.
$$


A higher *GeoHard* indicates greater class-wise hardness (i.e., poorer performance).

<div align="center">
  <img
    src="https://github.com/user-attachments/assets/a645b7ff-dabd-4e0f-a263-c71e8e95358f"
    alt="Image"
    width="372"
  />
</div>


<h2 id="result">Result</h2>

### Three-way classification tasks

We measure the class-wise hardness on two groups of tasks, i.e., Natural Language Inference (NLI) and Sentiment Classification (SC). We calculate Pearson's Correlation between class-wise hardness measurement and class-wise F1 scores. The baseline includes the aggregation of instance-level hardness measurement, including Sensitivity Analysis, Spread (Zhao et al., 2023).


<div align="center">
  <img width="792" height="326" alt="Image" src="https://github.com/user-attachments/assets/b53784f3-facf-423f-8442-5393d6347f25" />
</div>

### Other tasks

We also applied _GeoHard_ in wider NLP tasks, including AG News, Yahoo Answer Topic, Emo2019, and CARAR. The conclusion still stands that _GeoHard_ can perfectly measure the class-wise hardness.

<div align="center">
<img width="384" height="157" alt="Image" src="https://github.com/user-attachments/assets/6847d077-8815-43d9-8c9e-61176fd0ce51" />
</div>

### Other models

The results above are based on E5-large-v2. Therefore, in order to validate _GeoHard_'s generalization, we further adopt other embedding models.

<div align="center">
<img
  src="https://github.com/user-attachments/assets/abe37328-4de7-40ef-9878-a38ae5a02878"
  alt="Image"
  width="500"
/>
</div>


```
@inproceedings{cai-etal-2024-geohard,
    title = "$\textit{GeoHard}$: Towards Measuring Class-wise Hardness through Modelling Class Semantics",
    author = "Cai, Fengyu  and
      Zhao, Xinran  and
      Zhang, Hongming  and
      Gurevych, Iryna  and
      Koeppl, Heinz",
    editor = "Ku, Lun-Wei  and
      Martins, Andre  and
      Srikumar, Vivek",
    booktitle = "Findings of the Association for Computational Linguistics: ACL 2024",
    month = aug,
    year = "2024",
    address = "Bangkok, Thailand",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2024.findings-acl.332/",
    doi = "10.18653/v1/2024.findings-acl.332",
    pages = "5571--5597"
}
```

Contact person: [Fengyu Cai](mailto:fengyu.cai@tu-darmstadt.de)


