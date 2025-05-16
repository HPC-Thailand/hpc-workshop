## SLURM

### Step 1. ติดตั้ง Dependencies

> หมายเหตุ: VM ในการอบรมได้ทำการติดตั้ง Dependencies และดาวน์โหลด slurm ไว้เรียบร้อยแล้ว

```bash
sudo apt install libmariadb-dev libhwloc-dev liblz4-dev pkg-config libglib2.0-dev libreadline-dev libfreeipmi-dev liblua5.1-0-dev libpam0g-dev libdbus-1-dev libpmix-dev openmpi-bin openmpi-common libopenmpi3 libopenmpi-dev libopenblas-dev -y
```

### Step 2. ดาวน์โหลด SLURM

```bash
sudo wget https://download.schedmd.com/slurm/slurm-24.11.5.tar.bz2
```

```bash
sudo mv slurm-24.11.5.tar.bz2 /opt
```

### Step 3. แตกไฟล์

```bash
cd /opt
sudo tar -xvjf slurm-24.11.5.tar.bz2
```

### Step 4. configure, make, install

```bash
cd slurm-24.11.5
sudo ./configure --with-pmix=/lib/x86_64-linux-gnu/pmix2/ --with-systemdsystemunitdir=/etc/systemd/system --enable-pam --sysconfdir=/etc/slurm
sudo make
sudo make install
```

### Step 5.

```bash
cd slurm-24.11.5

#On  Frontend Node (vm-01)
sudo cp etc/slurmctld.service /lib/systemd/system
sudo cp etc/slurmdbd.service /lib/systemd/system

#On Compute Node (vm-02, vm-03)
sudo cp etc/slurmd.service /lib/systemd/system
```

### Step 6. สร้าง Directory

```bash
sudo mkdir -p /etc/slurm
sudo mkdir -p /var/log/slurm
sudo mkdir -p /var/spool/slurm
sudo mkdir -p /var/run/slurm
sudo mkdir -p /var/lib/slurm
sudo chown slurm:slurm -R /var/log/slurm
sudo chown slurm:slurm -R /var/spool/slurm
sudo chown slurm:slurm -R /var/run/slurm
sudo chown slurm:slurm -R /var/lib/slurm
```

### Step 7. สร้างไฟล์ Configuration

```bash

```

## สำหรับ SlurmDBD

### Step 1. ติดตั้ง Dependencies

```bash
sudo apt install mariadb-server mariadb-client -y
```

### Step 2. ติดตั้ง SlurmDBD

```bash

```
