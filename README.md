# Nvidia-Container-Setup

## Prerequisites: Update the NVIDIA drivers for your GPU - https://www.nvidia.com/en-us/drivers/ 

Link to Guide/Demo Video: https://youtu.be/gt_lfLW8lA0 

## Step 1: Install Docker on your WSL/Ubuntu Environment

In order to use the nvidia-container-toolkit we need to have docker in our WSL environment as just running Docker Desktop on our Windows Machine won't give Docker access to our gpu.  

All commands are from the installation steps here: https://docs.docker.com/engine/install/ubuntu/ 

**Open up command prompt and enter the following commands:**

```bash
bash
```

```bash
sudo apt-get update
```

```bash
sudo apt-get install ca-certificates curl
```

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```bash
sudo apt-get update
```

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Note: If you have any issues with running docker commands without the sudo prefix in front follow the instructions here: https://docs.docker.com/engine/install/linux-postinstall/

## Step 2: Installing the Nvidia Container Toolkit 

Official Documentation: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html

**Enter the following commands:** 

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

```bash
sudo apt-get update
```

```bash
sudo apt-get install -y nvidia-container-toolkit
```

```bash
sudo nvidia-ctk runtime configure --runtime=docker
```

```bash
sudo systemctl restart docker
```

## Step 3: Verify that Docker has access to your GPU

Enter the command

```bash
nvidia-smi
```

## Step 4: Edit the docker-compose.yml (and run_sim.sh file if you are using my scripts) 

I provided the files in this GitHub repository all that is needed is for you to copy-paste the new code in if you followed my previous guide to installing scripts that you can use in the docker container.

## Step 5: Running the simulation

First start all the containers with the following command:

```bash
docker compose up -d
```

**If using an internet browser to view the simulation through NOvnc enter the command:**
```bash
scripts/run_sim.sh <your_package> <your_node> novnc 
```

**If running the RViz2 simulator directy on your PC enter the command:**
```bash
scripts/run_sim.sh <your_package> <your_node> local
```

**Note: If the package and node executable have the same exact name which is the case for the safety_node and wall_follow_node you can enter**
```bash
scripts/run_sim.sh wall_follow <local or novnc here>
```



