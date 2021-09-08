# Parallel Computing: Learning Notes
## Cuda Introduction
Cuda is a set of APIs provided by NVIDIA, enabling developers to write functions that can run on GPU. In this article, we will introduce CUDA C and tried to write a small program using CUDA C.  
Every program written in CUDA should have a kernel function, which is the function running on GPU. Every thread in GPU runs the same piece of kernel function, with different indices. Below is an example:  
```c
__global__
void vecAdd(float* in_1, float* in_2, float* out, int size) {
  int i = blockIdx.x * blockDim.x + threadIdx;
  if (i < size) {
    out[i] = in_1[i] + in_2[i];
  }
}

int main() {
  float* host_input1;
  float* host_input2;
  float* host_output;
  float* device_input1;
  float* device_input2;
  float* device_output;
  int inputLength;

  // read input
  // read host_input1
  // read host_input2
  int size = inputLength * sizeof(inputLength);

  // allocate memory for input and output vectors in in GPU
  cudaMalloc((void**) &device_input1, size);
  cudaMalloc((void**) &device_input2, size);
  cudaMalloc((void**) &device_output, size);

  // copy the input vector to GPU memory
  cudaMemcpy(device_input1, host_input1, size, cudaMemcpyHostToDevice);
  cudaMemcpy(device_input2, host_input2, size, cudaMemcpyHostToDevice);

  // call the kernel function, do vector addition in GPU
  vecAdd<<<ceil(inputLength/256.0), 256>>>(device_input1, device_input2, device_output, size);

  // copy back the result from GPU to host memory
  cudaMemcpy(host_output, device_output, size, cudaMemcpyDeviceToHost);
  
  // free the GPU memory
  cudaFree(device_input1);
  cudaFree(device_input2);
  cudaFree(device_output);
}
```
In this example, we do a simple vector addition operation in GPU.  
## Tips
For every kernel function, it is assigned a grid in GPU, and each grid has many blocks, each block has many threads. Every operation is actually running in thread. Notice that grid and block are all 3 dimension. For example:  
```c
dim3 grid(2,2,2);
dim3 block(4,4,4);
kernel<<<grid, block>>>(...);
```
In that case, each grid has 8 blocks, each block has 64 threads, thus we have 512 threads in total. Notice that the # of blocks and # of threads are not required to be the power of 2, but it is highly recommended to be so since it can improve the hardware utilization.  
## Streaming Multiprocessors
In GPU, streaming multiprocessors are the part that do the calculation. There are many streaming processors in each streaming multiprocessors and the blocks are assigned to SM to do the calculation. Notice that multiple thread blocks can be assigned to each SM but it has the limit. Due to the fact that the # of SM and # of blocks that can assigned to SM are limited, there are actually a limited # of blocks that are executing in GPU at the same time.  
## Thread Scheduling, Warps
The warp is the # of threads in a block that can be executed in a SM at any instant in time. Since the threads in different blocks are not allowed to perform barrier synchrnoization, SM can swtich to another block to execute when one block is blocked, which means that CUDA runtime system can execute blocks in any order relative to each other. When one block is blocked waiting for results, the SM will select the warp that is no longer waiting for results to execute.  
However, for all threads within each block, they perform barrier synchronization. That means threads completes early in a block will wait for other threads to complete in the same block.  