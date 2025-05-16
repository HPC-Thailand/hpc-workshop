### การติดตั้งและใช้งาน MUNGE

MUNGE (MUNGE Uid 'N' Gid Emporium) เป็นระบบที่ใช้ในการจัดการ UID/GID ของผู้ใช้ในระบบคลัสเตอร์ โดยเฉพาะในระบบที่มีการใช้งาน HPC (High Performance Computing) เพื่อให้สามารถเข้าถึงทรัพยากรต่างๆ ระหว่าง Nodes ได้อย่างปลอดภัยและมีประสิทธิภาพ โดยในส่วนของการประมวลผล UID/GID จะถูกใช้ในการตรวจสอบสิทธิ์การเข้าถึงทรัพยากรต่างๆ ในระบบคลัสเตอร์ เช่น การเข้าถึงไฟล์ การใช้งาน CPU และ Memory เป็นต้น MUNGE จะช่วยให้การจัดการ UID/GID เป็นไปอย่างมีระเบียบและปลอดภัย โดยเฉพาะในระบบที่มีผู้ใช้หลายคนและมีการใช้งานทรัพยากรต่างๆ ร่วมกัน และทำงานร่วมกับระบบอื่นๆ เช่น SLURM, OpenMPI, และ PBS/Torque

---

## การติดตั้ง MUNGE จะมีขั้นตอนดังนี้

### สร้าง Global User และ Group

**สำหรับ munge**

```bash
export MUNGEUSER=9901
sudo groupadd -g $MUNGEUSER munge
sudo useradd --system -u "$MUNGEUSER" -g munge -d /var/lib/munge -s /sbin/nologin -c "MUNGE" --no-create-home munge
```

**สำหรับ slurm**

```bash
export SLURMUSER=9902
sudo groupadd -g $SLURMUSER slurm
sudo useradd --system -u "$SLURMUSER" -g slurm -d /var/lib/slurm -s /sbin/nologin -c "SLURM" --no-create-home slurm
```

ติดตั้ง MUNGE ทุกๆ Node ใน Cluster

### Step 1. ติดตั้ง Dependencies

```bash

sudo apt update
sudo apt install build-essential libssl-dev ntp libibverbs-dev libnuma-dev bzip2 zlib1g-dev -y
```

### Step 2. ดาวน์โหลด MUNGE

```bash
wget https://github.com/dun/munge/releases/download/munge-0.5.16/munge-0.5.16.tar.xz
```

> หมายเหตุ: ใน VM ของผู้เข้าอบรมได้มีการดาวน์โหลดเตรียมไว้แล้ว สามารถข้ามขั้นตอนนี้ได้เลย

### Step 3. แตกไฟล์

```bash
tar -xvf munge-0.5.16.tar.xz
cd munge-0.5.16
```

### Step 4. configure, make, install

```bash
./configure \
     --prefix=/usr \
     --sysconfdir=/etc \
     --localstatedir=/var \
     --runstatedir=/run
make
make check
sudo make install
```

### Step 5. สร้าง Key สำหรับ MUNGE

ที่เครื่องที่เป็น Frontend Node (vm-01) ให้ทำการสร้าง Key สำหรับ MUNGE โดยใช้คำสั่งดังนี้:

> หมายเหตุ: คำสั่งนี้จะสร้างไฟล์ `munge.key` ที่ใช้ในการเข้ารหัสและถอดรหัสข้อมูลระหว่าง Nodes ใน Cluster
> และทำแค่ที่เครื่อง Frontend Node ครั้งเดียวเท่านั้น

```bash
sudo chown munge:munge /etc/munge
sudo chown munge:munge /var/log/munge
sudo -u munge /sbin/mungekey --verbose
```
