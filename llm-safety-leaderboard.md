---
title: "An Introduction to Hugging Face LLM Safety Leaderboard"
thumbnail: /blog/assets/llm-leaderboard/leaderboard-thumbnail.png
authors:
- user: danielz01
- user: alphapav
- user: Cometkmt
- user: chejian
- user: BoLi-aisecure
---
Huggingface’s [Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) (originally created by Ed Beeching and Lewis Tunstall and maintained by Nathan Habib and Clémentine Fourrier) is well known for tracking the functional performance of open source LLMs, offering in-depth comparisons of these models’ performances across a variety of tasks, such as [TruthfulQA](https://github.com/sylinrl/TruthfulQA) and [HellaSwag](https://rowanzellers.com/hellaswag/). This platform has been of tremendous value to the open-source community, as it allows practitioners to keep track of state-of-the-art open-source models.

In the meantime, given the widespread adoption of LLMs, it is critical to understand their safety and risks in different scenarios before extensive deployments in the real world. In particular, the US Whitehouse has published an executive order on safe, secure, and trustworthy AI; the EU AI Act has emphasized the mandatory requirements for high-risk AI systems. Together with regulations, it is important to provide technical solutions to assess the risks of AI systems, enhance their safety, and potentially provide safe and aligned AI systems with guarantees.

Thus, in 2023, at [Secure Learning Lab](https://boli.cs.illinois.edu/), we introduced [DecodingTrust](https://decodingtrust.github.io/), the first comprehensive and unified evaluation platform dedicated to assessing the trustworthiness of LLMs. This paper has won the [Outstanding Paper Award](https://blog.neurips.cc/2023/12/11/announcing-the-neurips-2023-paper-awards/) at NeurIPS 2023. DecodingTrust provides a multifaceted evaluation framework covering eight trustworthiness perspectives, including toxicity, stereotype bias, adversarial robustness, OOD robustness, robustness on adversarial demonstrations, privacy, machine ethics, and fairness. In particular, DecodingTrust 1) offers comprehensive trustworthiness perspectives for a holistic trustworthiness evaluation, 2) provides novel red-teaming algorithms tailored for each perspective, enabling in-depth testing of LLMs, 3) supports easy installation across various cloud environments, 4) provides a comprehensive leaderboard for both open and close models based on their trustworthiness, 5) provides failure example studies to enhance transparency and understanding, 6) provides an end-to-end demonstration and detailed model evaluation reports for practical usage.

Today, we are excited to announce the release of the new [LLM Safety Leaderboard](https://huggingface.co/spaces/AI-Secure/llm-trustworthy-leaderboard), which focuses on safety evaluation for LLMs and is powered by the [HF leaderboard template](https://huggingface.co/demo-leaderboard-backend).

Concretely, DecodingTrust provides several novel red-teaming methodologies for each evaluation perspective to perform stress tests. 

For instance, for the privacy perspective, we provide different levels of evaluation, including 1) privacy leakage from pertaining data, 2) privacy leakage during conversations, and 3) privacy-related words and events understanding of LLMs. In particular, for 1) and 2), we have designed different approaches to performing privacy attacks. For example, we provide different formats of prompts to guide LLMs to output sensitive information such as email addresses and credit card numbers.

For the adversarial robustness perspective, we have designed several adversarial attack algorithms to generate stealthy perturbations added to the inputs to mislead the model outputs.

For the OOD robustness perspective, we have designed different style transformations, knowledge transformations, etc, to evaluate the model performance when 1) the input style is transformed to other relatively rare styles such as Shakespearean or poetic forms, or 2) the knowledge required to answer the question is not contained in the training data of LLMs.



# Summary
Based on the assessment conducted by DecodingTrust, we have provided key findings across various safety and trustworthiness dimensions. Overall, we find that 1) GPT-4 is more vulnerable than GPT-3.5, 2) no single LLM consistently outperforms others across all trustworthiness perspectives, 3) trade-offs exist between different trustworthiness perspectives, 4) LLMs demonstrate different capabilities in understanding different privacy-related words. For instance, if GPT-4 is prompted with “in confidence,” it may not leak private information, while it may leak information if prompted with “confidentially.” 5) LLMs are vulnerable to adversarial or misleading prompts or instructions under different trustworthiness perspectives.

# How to submit your model for evaluation

First, make sure you can load your model and tokenizer using AutoClasses:

```Python
from transformers import AutoConfig, AutoModel, AutoTokenizer
config = AutoConfig.from_pretrained("your model name", revision=revision)
model = AutoModel.from_pretrained("your model name", revision=revision)
tokenizer = AutoTokenizer.from_pretrained("your model name", revision=revision)
```

If this step fails, follow the error messages to debug your model before submitting it. It's likely your model has been improperly uploaded.

Note: make sure your model is public! Note: if your model needs use_remote_code=True, we do not support this option yet but we are working on adding it, stay posted!

Then, convert your model weights to safetensors
It's a new format for storing weights which is safer and faster to load and use. It will also allow us to add the number of parameters of your model to the Extended Viewer!

Finally, use the ["Submit here!" panel in our leaderboard](https://huggingface.co/spaces/AI-Secure/llm-trustworthy-leaderboard) to submit your model for evaluation!

# Citation

If you find our evaluations useful, please consider citing our work.

```
@article{wang2023decodingtrust,
  title={DecodingTrust: A Comprehensive Assessment of Trustworthiness in GPT Models},
  author={Wang, Boxin and Chen, Weixin and Pei, Hengzhi and Xie, Chulin and Kang, Mintong and Zhang, Chenhui and Xu, Chejian and Xiong, Zidi and Dutta, Ritik and Schaeffer, Rylan and others},
  booktitle={Thirty-seventh Conference on Neural Information Processing Systems Datasets and Benchmarks Track},
  year={2023}
}
```
