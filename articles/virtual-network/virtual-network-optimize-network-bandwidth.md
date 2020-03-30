---
title: VM ağ işbçisi | Microsoft Dokümanlar
description: Azure sanal makine ağı masını nasıl optimize edebilirsiniz öğrenin.
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/15/2017
ms.author: steveesp
ms.openlocfilehash: be5f38bdeaf51dbe23006ecf30b4deb66aa7402a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75690898"
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Azure sanal makineleri için ağ bilginizi optimize edin

Azure sanal makineleri (VM), ağ işbirliği için daha da optimize edilebilen varsayılan ağ ayarlarına sahiptir. Bu makalede, Ubuntu, CentOS ve Red Hat gibi büyük dağıtımlar da dahil olmak üzere Microsoft Azure Windows ve Linux VM'ler için ağ iş akışının nasıl optimize edilebildiğini açıklanmaktadır.

## <a name="windows-vm"></a>Windows VM

Windows VM'niz [Hızlandırılmış Ağ'ı](create-vm-accelerated-networking-powershell.md)destekliyorsa, bu özelliği etkinleştirmek iş için en uygun yapılandırma olacaktır. Diğer tüm Windows VM'leri için, Yan Ölçekleme Al (RSS) kullanmak, RSS'siz bir VM'den daha yüksek maksimal iş elde edilebilir. RSS varsayılan olarak bir Windows VM'de devre dışı bırakılmış olabilir. RSS'nin etkin olup olmadığını belirlemek ve şu anda devre dışı bırakılmışsa etkinleştirmek için aşağıdaki adımları tamamlayın:

1. `Get-NetAdapterRss` PowerShell komutuna sahip bir ağ bağdaştırıcısı için RSS etkin olup olmadığını görün. Aşağıdaki `Get-NetAdapterRss`örnekte, RSS'den döndürülen çıktı etkin değildir.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                 : False
    ```
2. RSS'yi etkinleştirmek için aşağıdaki komutu girin:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    Önceki komutun bir çıktısı yok. Komut NIC ayarlarını değiştirip yaklaşık bir dakika geçici bağlantı kaybına neden oldu. Bağlantı kaybı sırasında yeniden bağlanma iletişim kutusu görüntülenir. Bağlantı genellikle üçüncü denemeden sonra geri yüklenir.
3. Komutu yeniden girerek VM'de RSS'nin etkinleştirildiğinden `Get-NetAdapterRss` onaylayın. Başarılı olursa, aşağıdaki örnek çıktı döndürülür:

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                  : True
    ```

## <a name="linux-vm"></a>Linux VM

RSS varsayılan olarak bir Azure Linux VM'de her zaman etkinleştirilir. Ekim 2017'den bu yana yayımlanan Linux çekirdekleri, bir Linux VM'nin daha yüksek ağ iş tibunu elde etmesini sağlayan yeni ağ optimizasyonları seçeneklerini içerir.

### <a name="ubuntu-for-new-deployments"></a>Yeni dağıtımlar için Ubuntu

Ubuntu Azure çekirdeği Azure'daki en iyi ağ performansını sağlar ve 21 Eylül 2017'den beri varsayılan çekirdek olmuştur. Bu çekirdeği almak için, ilk aşağıdaki gibi, 16.04-LTS en son desteklenen sürümünü yükleyin:

```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```

Oluşturma tamamlandıktan sonra, en son güncelleştirmeleri almak için aşağıdaki komutları girin. Bu adımlar, şu anda Ubuntu Azure çekirdeğini çalıştıran VM'ler için de çalışır.

```bash
#run as root or preface with sudo
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

Aşağıdaki isteğe bağlı komut kümesi, Azure çekirdeğine zaten sahip olan ancak hatalarla daha fazla güncelleştirme sağlayamayan mevcut Ubuntu dağıtımları için yararlı olabilir.

```bash
#optional steps may be helpful in existing deployments with the Azure kernel
#run as root or preface with sudo
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

#### <a name="ubuntu-azure-kernel-upgrade-for-existing-vms"></a>Mevcut VM'ler için Ubuntu Azure çekirdek yükseltmesi

Azure Linux çekirdeğine yükseltilerek önemli bir iş elde etme performansı elde edilebilir. Bu çekirdeğin olup olmadığını doğrulamak için çekirdek sürümünüzü kontrol edin.

```bash
#Azure kernel name ends with "-azure"
uname -r

#sample output on Azure kernel:
#4.13.0-1007-azure
```

VM'nizde Azure çekirdeği yoksa, sürüm numarası genellikle "4.4" ile başlar. VM'de Azure çekirdeği yoksa, aşağıdaki komutları kök olarak çalıştırın:

```bash
#run as root or preface with sudo
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

En son optimizasyonları almak için, aşağıdaki parametreleri belirterek en son desteklenen sürümü ile bir VM oluşturmak en iyisidir:

```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.4",
"Version": "latest"
```

Yeni ve mevcut VM'ler en son Linux Entegrasyon Hizmetlerini (LIS) yüklemenin avantajını kullanabilir. Daha sonraki sürümlerde daha fazla iyileştirme içermesine rağmen, 4.2.2-2'den başlayan iş geliştirme, LIS'te dir. En son LIS'i yüklemek için aşağıdaki komutları girin:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

Optimizasyonları almak için, aşağıdaki parametreleri belirterek en son desteklenen sürümü ile bir VM oluşturmak en iyisidir:

```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7-RAW"
"Version": "latest"
```

Yeni ve mevcut VM'ler en son Linux Entegrasyon Hizmetlerini (LIS) yüklemenin avantajını kullanabilir. 4.2'den başlayan şekilde, iş artışı LIS'te dir. LIS'i indirmek ve yüklemek için aşağıdaki komutları girin:

```bash
wget https://aka.ms/lis
tar xvf lis
cd LISISO
sudo ./install.sh #or upgrade.sh if prior LIS was previously installed
```

Hyper-V için Linux Entegrasyon Hizmetleri Sürüm 4.2 hakkında daha fazla bilgi için [indirme sayfasını](https://www.microsoft.com/download/details.aspx?id=55106)görüntüleyerek daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
* Senaryonuz için [Bant Genişliği/İş-İş Testi Azure VM](virtual-network-bandwidth-testing.md) ile optimize edilmiş sonucu görün.
* [Bant genişliğinin sanal makinelere](virtual-machine-network-throughput.md) nasıl tahsis edildiğini okuyun
* Azure Sanal Ağı ile daha fazla bilgi edinin [sık sorulan sorular (SSS)](virtual-networks-faq.md)
