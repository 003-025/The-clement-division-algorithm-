2. GPU VERSION CODE - READY FOR WHEN POWER COMES BACK**
This uses PyTorch. Each digit gets its own GPU thread. Save as `clement_gpu.py`

```python
import torch
import time
from decimal import Decimal, getcontext
getcontext().prec = 200

def clement_division_gpu(number, denominator):
    device = 'cuda' if torch.cuda.is_available() else 'cpu'
    print(f"Running on: {device}")
    
    num_str = str(number)
    if '.' in num_str:
        whole, frac = num_str.split('.')
        num_str = whole + frac
        decimal_places = len(frac)
    else:
        decimal_places = 0
    
    n = len(num_str)
    denom_tensor = torch.tensor(float(denominator), device=device)
    
    Create tensors for ALL digits at once - this is the parallel part
    digits = torch.tensor([int(d) for d in num_str], device=device, dtype=torch.float64)
    exponents = torch.tensor([n - 1 - i - decimal_places for i in range(n)], device=device, dtype=torch.float64)
    
    GPU does all of these at the same time
    place_values = torch.pow(torch.tensor(10.0, device=device), exponents)
    parts = (digits * place_values) / denom_tensor
    
    Sum on GPU then move back
    result = torch.sum(parts).item()
    return result


THE BIG GPU RACE
number = 1234567890123456789012345678901234567890 # try 1000 digits later
denominator = 0.089

start = time.time()
result_gpu = clement_division_gpu(number, denominator)
gpu_time = time.time() - start

print("GPU Result:", result_gpu)
print("GPU Time:  ", gpu_time, "seconds")
