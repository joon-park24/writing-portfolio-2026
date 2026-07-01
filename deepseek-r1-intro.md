# DeepSeek-R1 Introduction and Quick Start

This page briefly explains reinforcement learning and chain of thought in DeepSeek-R1 and provides steps and Python examples to quickly try DeepSeek-R1 through API calls.

DeepSeek-R1 is an open-source large language model trained with reinforcement learning (RL) to perform complex reasoning. Such RL models are also called "reasoning models." In short, reasoning models "think" before delivering a final answer, and DeepSeek-R1 reveals its thinking process by providing a long Chain of Thought (CoT). CoT's use cases include:
- following the model's thought process as it produces an answer.
- troubleshooting, if the model produces an unsatisfactory answer.
- distillation (knowledge transfer from a large, complex model to a smaller, simpler model).

## Reinforcement Learning and Chain of Thought

RL is a paradigm of machine learning through which a machine learning model self-learns toward an optimal result or designated goal. RL encourages learning and desired behavior through a reward-penalty system. Essentially, the model learns by "thinking" and is rewarded as it approaches a satisfactory solution.

RL is comparable to human learning methods. For example, consider learning to solve a math problem: a student is given a problem, and a teacher may oversee the student's thought process. As the student performs the steps to solve the problem, the teacher may either confirm that the student is approaching the desired solution or redirect if the student is straying from a correct method. When the student provides an answer, the teacher confirms that the answer is satisfactory; RL models learn and train much in the same way. Therefore, the **process** for finding a solution is as significant as finding the solution itself. As an RL model trains, it expands its problem-solving skillset and - depending on the problem - flexibly interchanges certain skills in between problem-solving steps.

Because of its emphasis on sequential learning and thinking, RL is ideal for multi-step problem solving, such as scientific reasoning, mathematical proofs, and coding. When answering a question, DeepSeek-R1 "thinks out loud" and displays its step-by-step reasoning behind its final answer. This "thinking out loud" is also called CoT.

The following screenshot demonstrates CoT, which starts with `<think>` and ends with `</think>`:

![image](https://github.com/user-attachments/assets/a19f1cfc-9a10-4927-b681-7755a946d1b8)

For this instance, though much of the middle portion was omitted, CoT was over a hundred lines long, and for most instances, CoT is very verbose. DeepSeek-R1's final answer immediately follows `</think>`.

## Basic API Call

For a basic DeepSeek API call, the following are prerequisite steps:
- Apply for an [API key](https://platform.deepseek.com/api_keys)
- Install or update the OpenAI SDK:
    ```
    pip3 install -U openai
    ```

The following Python script accesses the DeepSeek API, with `<DeepSeek API Key>` being a stand-in for your API key:
```python
from openai import OpenAI

client = OpenAI(api_key="<DeepSeek API Key>", base_url="https://api.deepseek.com")

response = client.chat.completions.create(
    model="deepseek-reasoner",
    messages=[
        {"role": "system", "content": "You are a helpful assistant"},
        {"role": "user", "content": "Hello"},
    ],
    stream=False
)

print(response.choices[0].message.content)
```

> [!NOTE]
> This script is a non-stream example. To get stream responses, you can set `stream` to `True`.

`model` is set to `deepseek-reasoner`, though it can also be set to `deepseek-chat`. `deepseek-reasoner` utilizes DeepSeek-R1, while `deepseek-chat` utilizes DeepSeek-V3, which does not have CoT capabilities.

In `messages`,  `"role": "system"` indicates that the message following `"content"` is a system prompt. A system prompt is an initial set of instructions that define how DeepSeek should behave and respond to messages from `"role": "user"`.

> [!NOTE]
> A system prompt is not necessary to converse with the model.

Visit [DeepSeek API Reference documentation](https://api-docs.deepseek.com/api/create-chat-completion) for further details on parameters and chat fine-tuning.

## Multi-round Conversation

For conversations with DeepSeek-R1 that contain multiple rounds, the DeepSeek API does not store previous calls or messages. Therefore, you must append your entire conversation history and pass it to the DeepSeek API for each round of conversation.

Additionally, DeepSeek-R1's outputs contain the CoT (`reasoning_content`) and the final answer (`content`). The CoT should not be included in the appended conversation history that is passed to the API for the following conversation round. Therefore, before initiating the next conversation round, you should separate `reasoning_content` from each DeepSeek-R1 output, as shown in the following code example:

```python
from openai import OpenAI
client = OpenAI(api_key="<DeepSeek API Key>", base_url="https://api.deepseek.com")

# Round 1
messages = [{"role": "user", "content": "What's the highest mountain in the world?"}]
response = client.chat.completions.create(
    model="deepseek-reasoner",
    messages=messages
)

reasoning_content = response.choices[0].message.reasoning_content
content = response.choices[0].message.content

# Round 2
messages.append({"role": "assistant", "content": content})
messages.append({"role": "user", "content": "What is the second?"})
response = client.chat.completions.create(
    model="deepseek-reasoner",
    messages=messages
)
# ...
```

For Round 1, conversation starts normally, and `messages` passes to the API as:
```python
[
    {"role": "user", "content": "What's the highest mountain in the world?"}
]
```

Between Round 1 and Round 2, `reasoning_content` and `content` are separately defined and categorized.

For Round 2, only `content` is appended to `messages`, with `"role": "assistant"` indicating that `content` is from the AI "assistant". After user messages and final answers from all previous rounds, a new question is appended to `messages`. For Round 2, `messages` passes to the API as:
```python
[
    {"role": "user", "content": "What's the highest mountain in the world?"},
    {"role": "assistant", "content": "The highest mountain in the world is Mount Everest."},
    {"role": "user", "content": "What is the second?"}
]
```

A general structure for a multi-round conversation is illustrated in the following diagram:

![Diagram of the general structure of a multi-round conversation.](https://github.com/user-attachments/assets/577501f2-45bc-4522-aece-e8b0d77feb57)
