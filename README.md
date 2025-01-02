jtop
# Jetpack 4.6.1 [L4T 32.7.1]

sudo apt-get install -y libncurses5-dev libncursesw5-dev build-essential

wget https://developer.nvidia.com/embedded/l4t/r32_release_v7.1/sources/t210/public_sources.tbz2

tar -xvf public_sources.tbz2
cd Linux_for_Tegra/source/public
tar -xf kernel_src.tbz2

# add cpu arm-arch64 tweek
cd ~
git clone https://github.com/Qengineering/Jover.git
cd ~/Linux_for_Tegra/source/public
cp ~/Jover/clk-tegra124-dfll-fcpu.c kernel/kernel-4.9/drivers/clk/tegra/clk-tegra124-dfll-fcpu.c
cp ~/Jover/tegra210-dvfs.c kernel/kernel-4.9/drivers/soc/tegra/tegra210-dvfs.c

cd ~/Linux_for_Tegra/source/public/kernel/kernel-4.9
make tegra_defconfig
make menuconfig

make -j$(nproc)
make modules -j$(nproc)
sudo make modules_install
sudo cp arch/arm64/boot/Image /boot/Image

# check xfs supports

lsmod | grep xfs
sudo modprobe xfs
echo "xfs" | sudo tee -a /etc/modules



# make cpu maxspeed is 1.90GHz

sudo cp /etc/nvpmodel.conf /etc/nvpmodel.conf.backup

nano /etc/nvpmodel.conf

# MAXN is the NONE power model to release all constraints
< POWER_MODEL ID=0 NAME=MAXN >
CPU_ONLINE CORE_0 1
CPU_ONLINE CORE_1 1
CPU_ONLINE CORE_2 1
CPU_ONLINE CORE_3 1
CPU_A57 MIN_FREQ  0
#CPU_A57 MAX_FREQ -1 # is commented
GPU_POWER_CONTROL_ENABLE GPU_PWR_CNTL_EN on
GPU MIN_FREQ  0
#GPU MAX_FREQ -1 # is commented
GPU_POWER_CONTROL_DISABLE GPU_PWR_CNTL_DIS auto
EMC MAX_FREQ 0

#CPU_A57 MIN_FREQ 1900000
CPU_A57 MAX_FREQ 1900000
#GPU MIN_FREQ 950000000
GPU MAX_FREQ 950000000




Original manual [guide](https://qengineering.eu/overclocking-the-jetson-nano.html) exactly***.




