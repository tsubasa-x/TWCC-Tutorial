[TOC]

## Setup

### Login

[Official Tutorial](https://man.twcc.ai/@twccdocs/B15nJXe-B?type=view#2-%E7%99%BB%E5%85%A5%E3%80%8C%E7%99%BB%E5%85%A5%E7%AF%80%E9%BB%9E%E3%80%8D)

### SSH Auto Key In OTP (Optional, for Better Experience)

[Ray's Another World Blog](https://ray-fish.me/blog/2020/01/12/ssh-otp/?fbclid=IwAR1WAlvzTqq1W_tGnsEbDczJiPkJYFGtZfRM7-bIGpM2d5IhzM5DiBFQMYM)

### Install HomeBrew Linux (Optional, for Installing External Packages)

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
Add the following line to the bottom of  `~/.bashrc`
```bash
export PATH=$PATH:~/.linuxbrew/bin
```
## Python Environment Setup
### Initialize conda
```bash
conda init
```
### Create Virtual Environment
```bash
conda create --name <ENV_NAME> python=<PYTHON_VERSION>
```

### Conda Install PIP
First, activate your venv
```bash
conda activate <ENV_NAME>
```
Then,
```bash
conda install pip
```
### Prevent Using PIP3 In VENV (Optional)
```bash
ln -s ~/.conda/envs/<ENV_NAME>/bin/pip ~/.conda/envs/<ENV_NAME>/bin/pip3
```
## Job

### Submit A Job

In order to submit a job, you must write a job shell file for allocating the resources. the following shows a basic example job shell file:

`example_job.sh`

```bash
#!/bin/bash
#SBATCH -J <JOB_NAME>                # JOB_NAME: The name of your job
#SBATCH --output=<JOB_NAME>.out      # file for your job std output
#SBATCH -t <HH:mm:ss>                # e.g. 24:00:00
#SBATCH --account=<ACCOUNT>          # ACCOUNT: https://www.twcc.ai/user/dashboard
#                                    #          系統代碼 e.g. A*******3
#SBATCH --nodes=1                    # how many nodes you want to use
#SBATCH --gres=gpu:1                 # how many gpus you want to use
#SBATCH --cpus-per-task=4            # how many cpu cores you want to use (maximum 4 per gpu)
#SBATCH --partition=gp4d


module load nvidia/cuda/10.1         # cuda version depending on your framework to ensure your program is computing on gpu
module load cudnn/7.6
module load miniconda3
conda activate <ENV_NAME>            # ENV_NAME: the name of your venv
cd <WORKSPACE_PATH>
python ./<ENTRY_POINT>.py            # ENTRY_POINT: the entry point of your program
```
Once you've done the file, run `sbatch example_job.sh` to submit a job. Then you should see message like `Submitted batch job 123813`.

### List Running Jobs

```bash
squeue -u $USER
```
### Cancel Job
```bash
scancel <JOB_ID>
```



## SSH Tunnel (Advanced)

there's no public web port for TAIWANIA 2's login node. In order to use TensorBoard, you have to use **ssh tunnel**.

```bash
ssh -L <LOCAL PORT>:localhost:<REMOTE PORT> <username>@<remote IP>
```

Check out the [link](https://www.tecmint.com/create-ssh-tunneling-port-forwarding-in-linux/) for more information.
