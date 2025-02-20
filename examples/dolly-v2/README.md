# Dolly-V2

Transformer architecture: GPT-NeoX

Modeled from examples/stablelm

Ref: https://github.com/databrickslabs/dolly

Ref: https://github.com/stability-AI/stableLM/#stablelm-alpha

## Usage

```bash
# get the repo and build it
git clone https://github.com/ggerganov/ggml
cd ggml
mkdir build && cd build
cmake ..
make -j

# get the Dolly-V2 3B model
git clone https://huggingface.co/databricks/dolly-v2-3b

# convert model to FP16
python3 ../examples/dolly-v2/convert-h5-to-ggml.py ./dolly-v2-3b/ 1

# run inference using FP16 precision
./bin/dollyv2 -m ./dolly-v2-3b/ggml-model-f16.bin -p "State the meaning of life." -t 6 -n 64

main: seed = 1683218142
dollyv2_model_load: loading model from './dolly-v2-3b/ggml-model-f16.bin' - please wait ...
dollyv2_model_load: n_vocab = 50280
dollyv2_model_load: n_ctx   = 2048
dollyv2_model_load: n_embd  = 2560
dollyv2_model_load: n_head  = 32
dollyv2_model_load: n_layer = 32
dollyv2_model_load: n_rot   = 20
dollyv2_model_load: ftype   = 1
dollyv2_model_load: ggml ctx size = 7374.91 MB
dollyv2_model_load: memory_size =   640.00 MB, n_mem = 65536
dollyv2_model_load: ................................................ done
dollyv2_model_load: model size =  5295.10 MB / num tensors = 388
main: number of tokens in prompt = 32
main: token[0] =  30003, Below
main: token[1] =    310,  is
main: token[2] =    271,  an
main: token[3] =   9775,  instruction
main: token[4] =    326,  that
main: token[5] =   8631,  describes
main: token[6] =    247,  a
main: token[7] =   4836,  task
main: token[8] =    964, .
main: token[9] =  19566,  Write
main: token[10] =    247,  a
main: token[11] =   2380,  response
main: token[12] =    326,  that
main: token[13] =  20420,  appropriately
main: token[14] =  29141,  completes
main: token[15] =    253,  the
main: token[16] =   2748,  request
main: token[17] =    964, .
main: token[18] =    187, 

main: token[19] =    187, 

main: token[20] =  50278, ### Instruction:
main: token[21] =    187, 

main: token[22] =   5443, State
main: token[23] =    253,  the
main: token[24] =   4495,  meaning
main: token[25] =    273,  of
main: token[26] =   1495,  life
main: token[27] =    964, .
main: token[28] =    187, 

main: token[29] =    187, 

main: token[30] =  50279, ### Response:
main: token[31] =    187, 


Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
State the meaning of life.

### Response:
The meaning of life is to love and be loved.

### End

main: mem per token = 16136720 bytes
main:     load time =  2202.58 ms
main:   sample time =     2.57 ms
main:  predict time =  1497.14 ms / 33.27 ms per token
main:    total time =  6187.27 ms
```

## 5-bit integer quantization mode

```bash
# quantize the model to 5-bits using Q5_0 quantization
./bin/dollyv2-quantize ./dolly-v2-3b/ggml-model-f16.bin ./dolly-v2-3b/ggml-model-q5_0.bin 8

# run the quantized model
./bin/dollyv2 -m ./dolly-v2-3b/ggml-model-q5_0.bin -p "State the meaning of life." -t 6 -n 64

main: seed = 1683218518
dollyv2_model_load: loading model from './dolly-v2-3b/ggml-model-q5_0.bin' - please wait ...
dollyv2_model_load: n_vocab = 50280
dollyv2_model_load: n_ctx   = 2048
dollyv2_model_load: n_embd  = 2560
dollyv2_model_load: n_head  = 32
dollyv2_model_load: n_layer = 32
dollyv2_model_load: n_rot   = 20
dollyv2_model_load: ftype   = 8
dollyv2_model_load: ggml ctx size = 3902.68 MB
dollyv2_model_load: memory_size =   640.00 MB, n_mem = 65536
dollyv2_model_load: ................................................ done
dollyv2_model_load: model size =  1822.87 MB / num tensors = 388
main: number of tokens in prompt = 32
main: token[0] =  30003, Below
main: token[1] =    310,  is
main: token[2] =    271,  an
main: token[3] =   9775,  instruction
main: token[4] =    326,  that
main: token[5] =   8631,  describes
main: token[6] =    247,  a
main: token[7] =   4836,  task
main: token[8] =    964, .
main: token[9] =  19566,  Write
main: token[10] =    247,  a
main: token[11] =   2380,  response
main: token[12] =    326,  that
main: token[13] =  20420,  appropriately
main: token[14] =  29141,  completes
main: token[15] =    253,  the
main: token[16] =   2748,  request
main: token[17] =    964, .
main: token[18] =    187, 

main: token[19] =    187, 

main: token[20] =  50278, ### Instruction:
main: token[21] =    187, 

main: token[22] =   5443, State
main: token[23] =    253,  the
main: token[24] =   4495,  meaning
main: token[25] =    273,  of
main: token[26] =   1495,  life
main: token[27] =    964, .
main: token[28] =    187, 

main: token[29] =    187, 

main: token[30] =  50279, ### Response:
main: token[31] =    187, 


Below is an instruction that describes a task. Write a response that appropriately completes the request.

### Instruction:
State the meaning of life.

### Response:
The meaning of life is the discovery of the true self.

### End

main: mem per token = 16127760 bytes
main:     load time =  1011.09 ms
main:   sample time =     2.79 ms
main:  predict time =  1271.62 ms / 27.64 ms per token
main:    total time =  2802.51 ms
```

## Notes

- No guarantees for correctness
- The tokenizer is currently hacked - probably works only for English
- Non-parallel residual is not supported
- Contributions and improvements are welcome

## Note about possible bug
**There might be some issue with this implementation - not 100% sure.
The embeddings magnitude increases after each layer which is unexpected.
To observe this, uncomment the following line:**
https://github.com/ggerganov/ggml/blob/abea4b7609c14b837015ab625e3ac36c4708dd03/src/ggml.c#L9208
```
...
p[  0] =  65.5842
p[  1] =  61.6951
p[  2] =  59.3500
p[  3] =  61.2421
p[  4] =  65.9653
p[  5] =  59.4936
p[  6] =  58.4164
p[  0] = -209.6351
p[  1] = -214.0987
p[  2] = -217.0928
p[  3] = -215.0267
p[  4] = -208.2430
p[  5] = -215.3692
p[  6] = -214.1981
p[  0] = -301.0286
p[  1] = -308.6521
p[  2] = -310.7513
p[  3] = -307.0832
p[  4] = -299.9238
p[  5] = -306.0667
p[  6] = -302.1777
...
```
**Instead, I think the magnitude should remain around `1`.
See https://github.com/ggerganov/llama.cpp/issues/1063#issuecomment-1527730562 for more analysis**
