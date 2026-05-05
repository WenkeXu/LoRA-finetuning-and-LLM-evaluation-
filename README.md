This project aims to use prompting tests and attention visualizations from fine-tuned models to detect the degree to which models rely on a math “topic” when performing the task of identifying the math misconception.



**Model and Dataset**



The model we used is ["Qwen2-1.5B-Instruct"](https://huggingface.co/Qwen/Qwen2-1.5B-Instruct) and the [dataset](https://github.com/nancyotero-projects/math-misconceptions/tree/main/data) of math misconception is based on the result of the paper by [Otero Nancy et al. (2024)](https://arxiv.org/abs/2412.03765).

One example of the structure of the dataset:

{

        "Misconception":"when students don't understand how to represent proportional relationships.",

        "Misconception ID":"MaE01",

        "Topic":"Number sense",

        "Example Number":1,

        "Question":"What part is shaded?\\nWrite a Fraction\\n(Exercise A)",

        "Incorrect Answer":"1\\/3",

        "Correct Answer":"1\\/4",

        "Question image":"MaE1-Ex1Q",

        "Learner Answer image":"MaE1-Ex1LA",

        "Correct Answer image":"",

        "Source":"Ashlock, 2006",

        "Explanation":""

    }



**Structure and Contents**



The "prompting.ipynb" code performed the following on the model: First, conduct a few-shot prompting test without the information of topic on the model. The prompt contains some labeled examples from the dataset with information of question, incorrect answer, correct answer and misconception ID, and ask model to predict the misconception ID by question, incorrect answer and correct answer of another data from dataset. Second, conduct a same few-shot prompting test but with the information of topic in the example prompt and task description. Thirdly, conduct a chain-of-thought prompting test without the information of topic on the model, and last a chain-of-thought prompting test with the topic information.



The "lora\_ig.ipynb" code, first, the complete dataset are divided into training and evaluation sets, and then, 
the dataset are used to doing a LoRA fine-tuning on the attention part of the model. 
Then the adapter and model are merged to obtain the fine-tuned model.
The integrated gradient method in inseq package are used to test the contribution of the input texts to the model's output.
Three different type of input examples are used in this test. Example_a contains both text and number in inputs; Example_b contains only number, and Example c contains most of text in the inputs.
