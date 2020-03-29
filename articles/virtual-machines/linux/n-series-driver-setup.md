---
title: Linux için Azure N serisi GPU sürücü kurulumu
description: Azure'da Linux çalıştıran N serisi VM'ler için NVIDIA GPU sürücüleri nasıl ayarlanır?
services: virtual-machines-linux
author: cynthn
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/09/2019
ms.author: cynthn
ms.openlocfilehash: b424361f318504f96a57ee67722e725fbafc6561
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78944566"
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>Linux çalıştıran N serisi VM'lere NVIDIA GPU sürücülerini yükleyin

Linux çalıştıran Azure N serisi VM'lerin GPU özelliklerinden yararlanmak için NVIDIA GPU sürücülerinin yüklenmesi gerekir. [NVIDIA GPU Sürücü Uzantısı,](../extensions/hpccompute-gpu-linux.md) n serisi VM'ye uygun NVIDIA CUDA veya GRID sürücülerini yükler. Azure portalını veya Azure CLI veya Azure Kaynak Yöneticisi şablonları gibi araçları kullanarak uzantıyı yükleyin veya yönetin. Desteklenen dağıtımlar ve dağıtım adımları için [NVIDIA GPU Sürücü Uzantısı belgelerine](../extensions/hpccompute-gpu-linux.md) bakın.

GPU sürücülerini el ile yüklemeyi seçerseniz, bu makalede desteklenen dağıtımlar, sürücüler ve yükleme ve doğrulama adımları sağlanır. Windows [Sanal Ayarları](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)için manuel sürücü kurulum bilgileri de mevcuttur.

N serisi VM özellikleri, depolama kapasiteleri ve disk ayrıntıları için [GPU Linux VM boyutlarına](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın. 

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-cuda-drivers-on-n-series-vms"></a>N serisi VM'lere CUDA sürücülerini yükleme

NVIDIA CUDA Toolkit'in CUDA sürücülerini N serisi VM'lere yüklemek için atılacak adımlar aşağıda veda edebilirsiniz. 


C ve C++ geliştiricileri isteğe bağlı olarak GPU hızlandırılmış uygulamalar oluşturmak için tam Toolkit yükleyebilirsiniz. Daha fazla bilgi için [CUDA Yükleme Kılavuzu'na](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)bakın.

CUDA sürücülerini yüklemek için her VM'ye Bir SSH bağlantısı kurun. Sistemin CUDA özellikli bir GPU'ya sahip olduğunu doğrulamak için aşağıdaki komutu çalıştırın:

```bash
lspci | grep -i NVIDIA
```
Aşağıdaki örneğe benzer çıktı göreceksiniz (NVIDIA Tesla K80 kartı gösteriliyor):

![lspci komut çıkışı](./media/n-series-driver-setup/lspci.png)

Ardından, dağıtımınıza özel yükleme komutlarını çalıştırın.

### <a name="ubuntu"></a>Ubuntu 

1. NVIDIA web sitesinden CUDA sürücülerini indirin ve kurun. Örneğin, Ubuntu 16.04 LTS için:
   ```bash
   CUDA_REPO_PKG=cuda-repo-ubuntu1604_10.0.130-1_amd64.deb

   wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

   sudo dpkg -i /tmp/${CUDA_REPO_PKG}

   sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub 

   rm -f /tmp/${CUDA_REPO_PKG}

   sudo apt-get update

   sudo apt-get install cuda-drivers

   ```

   Yükleme birkaç dakika sürebilir.

2. İsteğe bağlı olarak tam CUDA araç kitini yüklemek için şunları yazın:

   ```bash
   sudo apt-get install cuda
   ```

3. VM'yi yeniden başlatın ve yüklemeyi doğrulamaya devam edin.

#### <a name="cuda-driver-updates"></a>CUDA sürücü güncellemeleri

Dağıtımdan sonra CUDA sürücülerini düzenli aralıklarla güncelleştirmenizi öneririz.

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```

### <a name="centos-or-red-hat-enterprise-linux"></a>CentOS veya Red Hat Enterprise Linux

1. Çekirdeği güncelleştirin (önerilir). Çekirdeği güncelleştirmemeyi seçerseniz, çekirdek sürümlerinin `kernel-devel` `dkms` ve çekirdeğiniz için uygun olduğundan emin olun.

   ```
   sudo yum install kernel kernel-tools kernel-headers kernel-devel
  
   sudo reboot

2. Install the latest [Linux Integration Services for Hyper-V and Azure](https://www.microsoft.com/download/details.aspx?id=55106).

   ```bash
   wget https://aka.ms/lis
 
   tar xvzf lis
 
   cd LISISO
 
   sudo ./install.sh
 
   sudo reboot
   ```
 
3. VM'ye yeniden bağlanın ve yüklemeye aşağıdaki komutlarla devam edin:

   ```bash
   sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

   sudo yum install dkms

   CUDA_REPO_PKG=cuda-repo-rhel7-10.0.130-1.x86_64.rpm

   wget http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/${CUDA_REPO_PKG} -O /tmp/${CUDA_REPO_PKG}

   sudo rpm -ivh /tmp/${CUDA_REPO_PKG}

   rm -f /tmp/${CUDA_REPO_PKG}

   sudo yum install cuda-drivers
   ```

   Yükleme birkaç dakika sürebilir. 

4. İsteğe bağlı olarak tam CUDA araç kitini yüklemek için şunları yazın:

   ```bash
   sudo yum install cuda
   ```

5. VM'yi yeniden başlatın ve yüklemeyi doğrulamaya devam edin.

### <a name="verify-driver-installation"></a>Sürücü yüklemeyi doğrulama

GPU aygıt durumunu sorgulamak için, SSH VM ve sürücü ile yüklü [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) komut satırı yardımcı programı çalıştırın. 

Sürücü yüklüyse, aşağıdakine benzer çıktı görürsünüz. VM'de şu anda bir GPU iş yükü çalıştırıyorsanız **GPU-Util'in** %0 gösterdiğini unutmayın. Sürücü sürümünüz ve GPU ayrıntılarınız gösterilenlerden farklı olabilir.

![NVIDIA cihaz durumu](./media/n-series-driver-setup/smi.png)

## <a name="rdma-network-connectivity"></a>RDMA ağ bağlantısı

RDMA ağ bağlantısı, aynı kullanılabilirlik kümesinde dağıtılan NC24r gibi RDMA özellikli N serisi VM'lerde veya VM ölçeğinde tek bir yerleşim grubunda etkinleştirilebilir. RDMA ağı, Intel MPI 5.x veya daha sonraki bir sürümle çalışan uygulamalar için İleti Geçme Arabirimi (MPI) trafiğini destekler. Ek gereksinimler aşağıdaki gibidir:

### <a name="distributions"></a>Dağıtım

N serisi VM'lerde RDMA bağlantısını destekleyen Azure Marketi'ndeki resimlerden birinden RDMA özellikli N serisi VM'leri dağıtın:
  
* **Ubuntu 16.04 LTS** - RdMA sürücülerini VM'de yapılandırın ve Intel MPI'yi indirmek için Intel'e kaydolun:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **CentOS tabanlı 7.4 HPC** - RDMA sürücüleri ve Intel MPI 5.1 VM'ye yüklenir.

## <a name="install-grid-drivers-on-nv-or-nvv3-series-vms"></a>NV veya NVv3 serisi VM'lere GRID sürücülerini yükleyin

NV veya NVv3 serisi VM'lere NVIDIA GRID sürücüleri yüklemek için, her VM'ye bir SSH bağlantısı yapın ve Linux dağıtımınız için adımları izleyin. 

### <a name="ubuntu"></a>Ubuntu 

1. `lspci` komutunu çalıştırın. NVIDIA M60 kartının veya kartının PCI aygıtları olarak görülebildiğinden doğrulayın.

2. Güncelleştirmeleri yükleyin.

   ```bash
   sudo apt-get update

   sudo apt-get upgrade -y

   sudo apt-get dist-upgrade -y

   sudo apt-get install build-essential ubuntu-desktop -y
   
   sudo apt-get install linux-azure -y
   ```
3. NVIDIA sürücüsüile uyumsuz olan Nouveau çekirdek sürücüsünü devre dışı kılabilir. (NV veya NVv2 VM'lerde NVIDIA sürücüsünü kullanın.) Bunu yapmak için, aşağıdaki `/etc/modprobe.d` `nouveau.conf` içerikleri içeren bir dosya oluşturun:

   ```
   blacklist nouveau

   blacklist lbm-nouveau
   ```


4. VM'yi yeniden başlatın ve yeniden bağlayın. Çıkış X sunucusu:

   ```bash
   sudo systemctl stop lightdm.service
   ```

5. GRID sürücüsünü indirin ve kurun:

   ```bash
   wget -O NVIDIA-Linux-x86_64-grid.run https://go.microsoft.com/fwlink/?linkid=874272  

   chmod +x NVIDIA-Linux-x86_64-grid.run

   sudo ./NVIDIA-Linux-x86_64-grid.run
   ``` 

6. X yapılandırma dosyanızı güncelleştirmek için nvidia-xconfig yardımcı programını çalıştırmak isteyip istemediğiniz sorulduğunda **Evet'i**seçin.

7. Kurulum tamamlandıktan sonra, kopya / vb /nvidia/gridd.conf.template yeni bir dosya gridd.conf at location /etc/nvidia/

   ```bash
   sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
   ```

8. Aşağıdakileri `/etc/nvidia/gridd.conf`ekleyin:
 
   ```
   IgnoreSP=FALSE
   EnableUI=FALSE
   ```
   
9. Varsa aşağıdakileri `/etc/nvidia/gridd.conf` kaldırın:
 
   ```
   FeatureType=0
   ```
10. VM'yi yeniden başlatın ve yüklemeyi doğrulamaya devam edin.


### <a name="centos-or-red-hat-enterprise-linux"></a>CentOS veya Red Hat Enterprise Linux 

1. Çekirdeği ve DKMS'yi güncelleştirin (önerilir). Çekirdeği güncelleştirmemeyi seçerseniz, çekirdek sürümlerinin `kernel-devel` `dkms` ve çekirdeğiniz için uygun olduğundan emin olun.
 
   ```bash  
   sudo yum update
 
   sudo yum install kernel-devel
 
   sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
   sudo yum install dkms
   
   sudo yum install hyperv-daemons
   ```

2. NVIDIA sürücüsüile uyumsuz olan Nouveau çekirdek sürücüsünü devre dışı kılabilir. (NV veya NV2 VM'lerde NVIDIA sürücüsünü kullanın.) Bunu yapmak için, aşağıdaki `/etc/modprobe.d` `nouveau.conf` içerikleri içeren bir dosya oluşturun:

   ```
   blacklist nouveau

   blacklist lbm-nouveau
   ```
 
3. VM'yi yeniden başlatın, [Hyper-V ve Azure için](https://www.microsoft.com/download/details.aspx?id=55106)en son Linux Tümleştirme Hizmetlerini yeniden bağlayın ve yükleyin.
 
   ```bash
   wget https://aka.ms/lis

   tar xvzf lis

   cd LISISO

   sudo ./install.sh

   sudo reboot

   ```
 
4. VM'ye yeniden bağlanın `lspci` ve komutu çalıştırın. NVIDIA M60 kartının veya kartının PCI aygıtları olarak görülebildiğinden doğrulayın.
 
5. GRID sürücüsünü indirin ve kurun:

   ```bash
   wget -O NVIDIA-Linux-x86_64-grid.run https://go.microsoft.com/fwlink/?linkid=874272  

   chmod +x NVIDIA-Linux-x86_64-grid.run

   sudo ./NVIDIA-Linux-x86_64-grid.run
   ``` 
6. X yapılandırma dosyanızı güncelleştirmek için nvidia-xconfig yardımcı programını çalıştırmak isteyip istemediğiniz sorulduğunda **Evet'i**seçin.

7. Kurulum tamamlandıktan sonra, kopya / vb /nvidia/gridd.conf.template yeni bir dosya gridd.conf at location /etc/nvidia/
  
   ```bash
   sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
   ```
  
8. Aşağıdakileri `/etc/nvidia/gridd.conf`ekleyin:
 
   ```
   IgnoreSP=FALSE
   EnableUI=FALSE 
   ```
9. Varsa aşağıdakileri `/etc/nvidia/gridd.conf` kaldırın:
 
   ```
   FeatureType=0
   ```
10. VM'yi yeniden başlatın ve yüklemeyi doğrulamaya devam edin.


### <a name="verify-driver-installation"></a>Sürücü yüklemeyi doğrulama


GPU aygıt durumunu sorgulamak için, SSH VM ve sürücü ile yüklü [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) komut satırı yardımcı programı çalıştırın. 

Sürücü yüklüyse, aşağıdakine benzer çıktı görürsünüz. VM'de şu anda bir GPU iş yükü çalıştırıyorsanız **GPU-Util'in** %0 gösterdiğini unutmayın. Sürücü sürümünüz ve GPU ayrıntılarınız gösterilenlerden farklı olabilir.

![NVIDIA cihaz durumu](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a>X11 sunucusu
Bir NV veya NVv2 VM'ye uzaktan bağlantılar için bir X11 sunucusuna ihtiyacınız varsa, [x11vnc](http://www.karlrunge.com/x11vnc/) grafik donanım hızlandırmasağlar çünkü önerilir. M60 aygıtının BusID'si X11 yapılandırma dosyasına el `etc/X11/xorg.conf`ile eklenmelidir (genellikle. Aşağıdakilere `"Device"` benzer bir bölüm ekleyin:
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "PCI:0@your-BusID:0:0"
EndSection
```
 
Ayrıca, bu `"Screen"` aygıtı kullanmak için bölümünüzü güncelleştirin.
 
Ondalık BusID çalıştırarak bulunabilir

```bash
nvidia-xconfig --query-gpu-info | awk '/PCI BusID/{print $4}'
```
 
BusID, bir VM yeniden tahsis edildiğinde veya yeniden başlatıldığında değişebilir. Bu nedenle, bir VM yeniden başlatıldığında X11 yapılandırmasında BusID'yi güncelleştirmek için bir komut dosyası oluşturmak isteyebilirsiniz. Örneğin, aşağıdakilere benzer `busidupdate.sh` içeriklere sahip bir komut dosyası (veya seçtiğiniz başka bir ad) oluşturun:

```bash 
#!/bin/bash
XCONFIG="/etc/X11/xorg.conf"
OLDBUSID=`awk '/BusID/{gsub(/"/, "", $2); print $2}' ${XCONFIG}`
NEWBUSID=`nvidia-xconfig --query-gpu-info | awk '/PCI BusID/{print $4}'`

if [[ "${OLDBUSID}" == "${NEWBUSID}" ]] ; then
        echo "NVIDIA BUSID not changed - nothing to do"
else
        echo "NVIDIA BUSID changed from \"${OLDBUSID}\" to \"${NEWBUSID}\": Updating ${XCONFIG}" 
        sed -e 's|BusID.*|BusID          '\"${NEWBUSID}\"'|' -i ${XCONFIG}
fi
```

Ardından, komut dosyası önyükleme `/etc/rc.d/rc3.d` üzerinde kök olarak çağrılır, böylece güncelleştirme komut dosyası için bir giriş oluşturun.

## <a name="troubleshooting"></a>Sorun giderme

* Kartları sorgulamanız gerektiğinde `nvidia-smi` komutun çıktısının daha hızlı olması için kalıcılık modunu ayarlayabilirsiniz. Kalıcılık modunu ayarlamak `nvidia-smi -pm 1`için çalıştırın. VM yeniden başlatılırsa, mod ayarı ortadan kalkar. Başlangıç ta çalıştırılabilmek için mod ayarını her zaman komut dosyası na yazabilirsiniz.
* NVIDIA CUDA sürücülerini en son sürüme güncellediyseniz ve RDMA bağlantısının artık çalışmadığını fark ederseniz, bu bağlantıyı yeniden algılamak için [RDMA sürücülerini yeniden yükleyin.](https://docs.microsoft.com/azure/virtual-machines/linux/n-series-driver-setup#rdma-network-connectivity) 

## <a name="next-steps"></a>Sonraki adımlar

* Yüklü NVIDIA sürücülerinizle bir Linux VM görüntüsü yakalamak [için, bir Linux sanal makinesini nasıl genelleştirip yakalayacaklarınıza](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.
