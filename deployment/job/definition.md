# <img height="25px" src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/bolt.svg" width="50" height="50"> Job

A job basically executes the code once and if it complets successfully, the job is marked as Succeeded. If it fails, the job can be configured to attempt a certain number of retries. If the code doesn't end even after the configured number of retries, the job is marked as failed. The compute and memory resources are released once the job is completed and hence we don't incur any cost once the job completes.

Jobs can be triggered in multiple ways:

1. **Manual**: This is good for adhoc use cases and can be triggered manually. An example can be a model training job which can be run when needed. 
2. **Schedule**: A job can be triggered on a schedule like daily, weekly, or at 9AM every Monday. An example of this can be a batch inference job running every morning at 8 AM on the previous day's incoming data. 


The code below shows an example job:

```
.
└── main.py
```
// TODO: Add some requirement so that we can have a valid requirements.txt
**`main.py`**
```python
import argparse
import time


def print_numbers(upto: int):
    for i in range(1, upto + 1):
        print(i)
        time.sleep(1)


if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--upto", type=int)
    args = parser.parse_args()

    print_numbers(args.upto)
```

If we do `python main.py`, the code will run and then end. In the next section, we will see how we deploy 
this job to run in the cloud so that we can use more resources than what we have locally in our machines and also let it run for a much longer time without interruption. 


   
