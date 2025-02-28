## CUDA 버전 맞는 링크 검색 (예시 12.4)
```
wget https://developer.download.nvidia.com/compute/cuda/12.4.0/local_installers/cuda_12.4.0_550.54.14_linux.run
sudo sh cuda_12.4.0_550.54.14_linux.run --silent --toolkit --toolkitpath=/usr/local/cuda-12.4
```

## CUDA 버전 변경
```
# 여기에서 export 쿠다 버전 변경
code ~/.bashrc 
source ~/.bashrc
```

## NVCC 버전 확인
```
nvcc -V
```
