# Instructions for training stable diffusion with Automatic 1111 on AWS

## Create an instance 
e.g. p3 with 8cpus.

Use this OS: Deep Learning AMI GPU PyTorch 1.12.1 from ubuntu

Create a key file locally, e.g. key.pem.

chmod 600 key.pem

ssh -i key.pem ec2-user@AWS IP (check the username with this page https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html#TroubleshootingInstancesConnectingPuTTY)

## Installation
apt-get install wget git python3 python3-venv

bash <(wget -qO- https://raw.githubusercontent.com/AUTOMATIC1111/stable-diffusion-webui/master/webui.sh)

bash webui.sh 

Stop the script when it has finished installation.

cd /home/$(whoami)/stable-diffusion-webui/

Modify webui-user.sh:

Uncomment the line with export commandline_args

export COMMANDLINE_ARGS="--share --listen"

Modify python command to python_cmd="python3 -u" to be able to run it with nohup 

## Using the interface

Nohup command is 

nohup bash webui.sh  &> log.txt &

Download this VAE:

wget https://huggingface.co/stabilityai/sd-vae-ft-ema-original/resolve/main/vae-ft-ema-560000-ema-pruned.ckpt -O models/VAE/vae-ft-ema-560000-ema-pruned.ckpt

Place it in /home/$(whoami)/stable-diffusion-webui/models/vae, and select it in Settings > Stable diffusion in the UI.

Choose directory for the images and other settings in the Dreambooth tab before training.

## Additional resources

Script for converting safetensors file to diffusers format if needed:

https://github.com/haofanwang/Lora-for-Diffusers
Steps needed for this to run:

source activate pytorch 

git clone https://github.com/huggingface/diffusers

cd ./diffusers

pip install .
pip install transformers

pip install omegaconf

pip install safetensors

python ./scripts/convert_original_stable_diffusion_to_diffusers.py --checkpoint_path /home/$(whoami)/stable-diffusion-webui/models/Stable-diffusion/18472.safetensors --dump_path save_dir --from_safetensors

