
This is Sharon ([@kikitamoon](https://github.com/kikitaMoon))'s notes of Llama2 local deployment.
Please check the official README for more details and explanation.

## Request Download link


1. [request a new download link](https://ai.meta.com/resources/models-and-libraries/llama-downloads/). The download link is received via email, and it should begin with https://download.llamameta.net.
2. Once your request is approved, you will receive a signed URL over email. Then run the download.sh script, passing the URL provided when prompted to start the download. 

> Pre-requisites: make sure you have `wget` and `md5sum` installed. Then to run the script: `./download.sh`.
> Keep in mind that the links expire after 24 hours and a certain amount of downloads. If you start seeing errors such as `403: Forbidden`, you can always re-request a link.


## Setup

In a conda env with PyTorch / CUDA available, clone the repo and run in the top-level directory.

1. Create a new virtual environment in Conda, e.g. `llama2`.
   
2. Clone the facebook official [llmala repo](https://github.com/facebookresearch/llama).
   
3. Install packages, 

    ```
    pip install -e .
    ```
    >  This command is telling Pip to install the package located in the current directory in editable mode. 
    > `-e` means editable; `.(dot)` means the current directory,  so the package should have a `setup.py` in that directory.
    > Corresponding,  in this [Llama2_Local](https://github.com/kikitaMoon/Llama2_Local) Repo / setup.py + requirements.txt

4. Run `./download.sh`  and input the received download link from Facebook via email. 

5. Wait for hours to finish the download.


## Issue encountered

When running the examples,  


**Examples using llama-2-7b-chat**:

```
torchrun --nproc_per_node 1 example_chat_completion.py \
    --ckpt_dir llama-2-7b-chat/ \
    --tokenizer_path tokenizer.model \
    --max_seq_len 512 --max_batch_size 4
```

An error occured the same as https://github.com/facebookresearch/llama/issues/592.

Trying to use Hugging Face version instead.
https://huggingface.co/meta-llama/Llama-2-7b-chat-hf/tree/main 


---------------------------------


## Inference

Different models require different model-parallel (MP) values:

|  Model | MP |
|--------|----|
| **7B** | 1 |
| 13B    | 2  |
| 70B    | 8  |

All models support sequence length up to 4096 tokens, but we pre-allocate the cache according to `max_seq_len` and `max_batch_size` values. So set those according to your hardware.


## References

1. [Research Paper](https://ai.meta.com/research/publications/llama-2-open-foundation-and-fine-tuned-chat-models/)
2. [Llama 2 technical overview](https://ai.meta.com/resources/models-and-libraries/llama)
3. [Open Innovation AI Research Community](https://ai.meta.com/llama/open-innovation-ai-research-community/)

## Original LLaMA
The repo for the original llama release is in the [`llama_v1`](https://github.com/facebookresearch/llama/tree/llama_v1) branch.
