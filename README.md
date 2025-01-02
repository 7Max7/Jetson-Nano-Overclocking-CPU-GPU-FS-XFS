# Overclocking Jetson Nano

jtop
## Jetpack 4.6.1 [L4T 32.7.1]

### Prerequisites
```bash
sudo apt-get install -y libncurses5-dev libncursesw5-dev build-essential git nano
```

### Download and Extract Kernel Sources
```bash
wget https://developer.nvidia.com/embedded/l4t/r32_release_v7.1/sources/t210/public_sources.tbz2
tar -xvf public_sources.tbz2
cd Linux_for_Tegra/source/public
tar -xf kernel_src.tbz2
```

### Apply CPU ARM-Arch64 Tweak
```bash
cd ~
git clone https://github.com/Qengineering/Jover.git
cd ~/Linux_for_Tegra/source/public
cp ~/Jover/clk-tegra124-dfll-fcpu.c kernel/kernel-4.9/drivers/clk/tegra/clk-tegra124-dfll-fcpu.c
cp ~/Jover/tegra210-dvfs.c kernel/kernel-4.9/drivers/soc/tegra/tegra210-dvfs.c
```

### Build the Kernel
```bash
cd ~/Linux_for_Tegra/source/public/kernel/kernel-4.9
make tegra_defconfig
make menuconfig
# Select XFS as M (module)

make -j$(nproc)
make modules -j$(nproc)
sudo make modules_install
sudo cp arch/arm64/boot/Image /boot/Image
```

### Verify XFS Support
```bash
lsmod | grep xfs
sudo modprobe xfs
echo "xfs" | sudo tee -a /etc/modules
```

### Reboot the System
```bash
reboot
```

### Configure CPU Max Speed to 1.90GHz
1. Backup the current configuration:
    ```bash
    sudo cp /etc/nvpmodel.conf /etc/nvpmodel.conf.backup
    ```
2. Edit the configuration file:
    ```bash
    nano /etc/nvpmodel.conf
    ```
3. Update the `MAXN` power model section to the following:
    ```
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
    ```

---

**Original Manual [Guide](https://qengineering.eu/overclocking-the-jetson-nano.html)**

---

jtop
## Jetpack 4.6.1 [L4T 32.7.1]

### Предварительные настройки
```bash
sudo apt-get install -y libncurses5-dev libncursesw5-dev build-essential git nano
```

### Загрузка и извлечение исходников ядра
```bash
wget https://developer.nvidia.com/embedded/l4t/r32_release_v7.1/sources/t210/public_sources.tbz2
tar -xvf public_sources.tbz2
cd Linux_for_Tegra/source/public
tar -xf kernel_src.tbz2
```

### Применение модификаций для ARM-Arch64
```bash
cd ~
git clone https://github.com/Qengineering/Jover.git
cd ~/Linux_for_Tegra/source/public
cp ~/Jover/clk-tegra124-dfll-fcpu.c kernel/kernel-4.9/drivers/clk/tegra/clk-tegra124-dfll-fcpu.c
cp ~/Jover/tegra210-dvfs.c kernel/kernel-4.9/drivers/soc/tegra/tegra210-dvfs.c
```

### Сборка ядра
```bash
cd ~/Linux_for_Tegra/source/public/kernel/kernel-4.9
make tegra_defconfig
make menuconfig
# Выберите XFS как M (модуль)

make -j$(nproc)
make modules -j$(nproc)
sudo make modules_install
sudo cp arch/arm64/boot/Image /boot/Image
```

### Проверка поддержки XFS
```bash
lsmod | grep xfs
sudo modprobe xfs
echo "xfs" | sudo tee -a /etc/modules
```

### Перезагрузка системы
```bash
reboot
```

### Настройка максимальной частоты процессора на 1.90GHz
1. Создайте резервную копию конфигурации:
    ```bash
    sudo cp /etc/nvpmodel.conf /etc/nvpmodel.conf.backup
    ```
2. Измените конфигурационный файл:
    ```bash
    nano /etc/nvpmodel.conf
    ```
3. Обновите раздел `MAXN` следующим образом:
    ```
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
    ```

---

**Оригинальная инструкция [Руководство](https://qengineering.eu/overclocking-the-jetson-nano.html)**


