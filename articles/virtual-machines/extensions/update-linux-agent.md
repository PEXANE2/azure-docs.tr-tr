---
title: Azure Linux Aracısını GitHub'dan güncelleştirin
description: Azure'daki Linux VM'iniz için Azure Linux Aracısını nasıl güncelleştireceğimiz öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: mimckitt
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: f1f19300-987d-4f29-9393-9aba866f049c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 08/02/2017
ms.author: mimckitt
ms.openlocfilehash: e4489f7c810799ca8e89565fe698f398f942b089
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78251714"
---
# <a name="how-to-update-the-azure-linux-agent-on-a-vm"></a>Azure Linux Aracısını VM'de güncelleştirme

[Azure Linux Aracınızı](https://github.com/Azure/WALinuxAgent) Azure'daki bir Linux VM'de güncellemek için şulara sahip olmalısınız:

- Azure'da çalışan bir Linux VM.
- SSH kullanarak linux VM bir bağlantı.

Her zaman linux dağıtım deposunda ilk bir paket için kontrol etmelisiniz. Mevcut paketin en son sürüm olmaması mümkündür, ancak otomatik güncellemeyi etkinleştirmek Linux Agent'ın her zaman en son güncellemeyi almalarını sağlayacaktır. Paket yöneticilerinden yükleme sorunlarınız varsa, dağıtım satıcısından destek almalısınız.

## <a name="minimum-virtual-machine-agent-support-in-azure"></a>Azure'da minimum sanal makine aracı desteği
İşleme devam etmeden önce [Azure'daki sanal makine aracıları için Minimum sürüm desteğini](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) doğrulayın.

## <a name="updating-the-azure-linux-agent"></a>Azure Linux Aracısını Güncelleme

## <a name="ubuntu"></a>Ubuntu

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümünüzü kontrol edin

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a>Paket önbelleğini güncelleştir

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>En son paket sürümünü yükleyin

```bash
sudo apt-get install walinuxagent
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirmenin etkin olduğundan emin olun

İlk olarak, etkinleştirilip etkinleştirilemeden denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled'ı bulun. Bu çıktıyı görürseniz, etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalıştır'ı etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>Waagent hizmetini yeniden başlatın

#### <a name="restart-agent-for-1404"></a>14.04 için aracıyı yeniden başlat

```bash
initctl restart walinuxagent
```

#### <a name="restart-agent-for-1604--1704"></a>16.04 / 17.04 için agent yeniden başlatın

```bash
systemctl restart walinuxagent.service
```

## <a name="red-hat--centos"></a>Kırmızı Şapka / CentOS

### <a name="rhelcentos-6"></a>RHEL/CentOS 6

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümünüzü kontrol edin

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Kullanılabilir güncelleştirmeleri denetleyin

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-the-latest-package-version"></a>En son paket sürümünü yükleyin

```bash
sudo yum install WALinuxAgent
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirmenin etkin olduğundan emin olun 

İlk olarak, etkinleştirilip etkinleştirilemeden denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled'ı bulun. Bu çıktıyı görürseniz, etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalıştır'ı etkinleştirmek için:

```bash
sudo sed -i 's/\# AutoUpdate.Enabled=y/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>Waagent hizmetini yeniden başlatın

```
sudo service waagent restart
```

### <a name="rhelcentos-7"></a>RHEL/CentOS 7

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümünüzü kontrol edin

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Kullanılabilir güncelleştirmeleri denetleyin

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-the-latest-package-version"></a>En son paket sürümünü yükleyin

```bash
sudo yum install WALinuxAgent  
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirmenin etkin olduğundan emin olun 

İlk olarak, etkinleştirilip etkinleştirilemeden denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled'ı bulun. Bu çıktıyı görürseniz, etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalıştır'ı etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>Waagent hizmetini yeniden başlatın

```bash
sudo systemctl restart waagent.service
```

## <a name="suse-sles"></a>SUSE SLES

### <a name="suse-sles-11-sp4"></a>SUSE SLES 11 SP4

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümünüzü kontrol edin

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Kullanılabilir güncelleştirmeleri denetleyin

Yukarıdaki çıktı, paketin güncel olup olmadığını gösterir.

#### <a name="install-the-latest-package-version"></a>En son paket sürümünü yükleyin

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirmenin etkin olduğundan emin olun 

İlk olarak, etkinleştirilip etkinleştirilemeden denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled'ı bulun. Bu çıktıyı görürseniz, etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalıştır'ı etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>Waagent hizmetini yeniden başlatın

```bash
sudo /etc/init.d/waagent restart
```

### <a name="suse-sles-12-sp2"></a>SUSE SLES 12 SP2

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümünüzü kontrol edin

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Kullanılabilir güncelleştirmeleri denetleyin

Yukarıdaki çıktıda, bu size paketin güncel olup olmadığını gösterir.

#### <a name="install-the-latest-package-version"></a>En son paket sürümünü yükleyin

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirmenin etkin olduğundan emin olun 

İlk olarak, etkinleştirilip etkinleştirilemeden denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled'ı bulun. Bu çıktıyı görürseniz, etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalıştır'ı etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>Waagent hizmetini yeniden başlatın

```bash
sudo systemctl restart waagent.service
```

## <a name="debian"></a>Debian

### <a name="debian-7-jesse-debian-7-stretch"></a>Debian 7 "Jesse"/ Debian 7 "Stretch"

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümünüzü kontrol edin

```bash
dpkg -l | grep waagent
```

#### <a name="update-package-cache"></a>Paket önbelleğini güncelleştir

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>En son paket sürümünü yükleyin

```bash
sudo apt-get install waagent
```

#### <a name="enable-agent-auto-update"></a>Aracı otomatik güncelleştirmeyi etkinleştirme
Debian bu sürümü >= 2.0.16 sürümü yok, bu nedenle AutoUpdate bunun için mevcut değildir. Yukarıdaki komuttan çıkan çıktı, paketin güncel olup olmadığını gösterir.



### <a name="debian-8-jessie--debian-9-stretch"></a>Debian 8 "Jessie" / Debian 9 "Stretch"

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümünüzü kontrol edin

```bash
apt list --installed | grep waagent
```

#### <a name="update-package-cache"></a>Paket önbelleğini güncelleştir

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>En son paket sürümünü yükleyin

```bash
sudo apt-get install waagent
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirmenin etkin olduğundan emin olun
İlk olarak, etkinleştirilip etkinleştirilemeden denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled'ı bulun. Bu çıktıyı görürseniz, etkinleştirilir:

```bash
AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalıştır'ı etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
Restart the waagent service
sudo systemctl restart walinuxagent.service
```

## <a name="oracle-linux-6-and-oracle-linux-7"></a>Oracle Linux 6 ve Oracle Linux 7

Oracle Linux için deponun `Addons` etkin olduğundan emin olun. Dosyayı `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) veya `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux) düzenlemeyi `enabled=0` ve satırı bu dosyadaki [ol6_addons] veya `enabled=1` **[ol7_addons]** altında olarak değiştirmeyi seçin. **[ol7_addons]**

Ardından, Azure Linux Aracısı'nın en son sürümünü yüklemek için şunları yazın:

```bash
sudo yum install WALinuxAgent
```

Eklenti deposunu bulamazsanız, Oracle Linux sürümünüzün sonuna bu satırları ekleyebilirsiniz:

Oracle Linux 6 sanal makineleri için:

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=https://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=https://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

Oracle Linux 7 sanal makineleri için:

```sh
[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1
```

Sonra yazın:

```bash
sudo yum update WALinuxAgent
```

Genellikle ihtiyacınız olan tek şey budur, ancak bir nedenle https://github.com doğrudan yüklemeniz gerekiyorsa, aşağıdaki adımları kullanın.


## <a name="update-the-linux-agent-when-no-agent-package-exists-for-distribution"></a>Dağıtım için aracı paketi olmadığında Linux Aracısını güncelleştirin

Wget yükleyin (komut satırına yazarak `sudo yum install wget` Red Hat, CentOS ve Oracle Linux sürümleri 6.4 ve 6.5 gibi varsayılan olarak yüklemeyen bazı dağıtımlar vardır.

### <a name="1-download-the-latest-version"></a>1. En son sürümü indirin
Bir web sayfasında [GitHub'da Azure Linux Aracısı'nın sürümünü](https://github.com/Azure/WALinuxAgent/releases) açın ve en son sürüm numarasını öğrenin. (Geçerli sürümünüzü yazarak `waagent --version`bulabilirsiniz .)

#### <a name="for-version-22x-or-later-type"></a>Sürüm 2.2.x veya sonraki sürüm için şunları yazın:
```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.x.zip
unzip v2.2.x.zip
cd WALinuxAgent-2.2.x
```

Aşağıdaki satır da örnek olarak sürüm 2.2.0 kullanır:

```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.14.zip
unzip v2.2.14.zip  
cd WALinuxAgent-2.2.14
```

### <a name="2-install-the-azure-linux-agent"></a>2. Azure Linux Aracısını Yükleyin

#### <a name="for-version-22x-use"></a>Sürüm 2.2.x için şunları kullanın:
Önce paketi `setuptools` yüklemeniz gerekebilir-- [buraya](https://pypi.python.org/pypi/setuptools)bakın. Ardından şunu çalıştırın:

```bash
sudo python setup.py install
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirmenin etkin olduğundan emin olun

İlk olarak, etkinleştirilip etkinleştirilemeden denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled'ı bulun. Bu çıktıyı görürseniz, etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalıştır'ı etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="3-restart-the-waagent-service"></a>3. Waagent hizmetini yeniden başlatın
Linux dağıtımlarının çoğu için:

```bash
sudo service waagent restart
```

Ubuntu için şunları kullanın:

```bash
sudo service walinuxagent restart
```

CoreOS için şunları kullanın:

```bash
sudo systemctl restart waagent
```

### <a name="4-confirm-the-azure-linux-agent-version"></a>4. Azure Linux Aracısı sürümünü onaylama
    
```bash
waagent -version
```

CoreOS için yukarıdaki komut çalışmayabilir.

Azure Linux Aracısı sürümünün yeni sürüme güncellendiğini göreceksiniz.

Azure Linux Aracısı ile ilgili daha fazla bilgi için Azure [Linux Agent README'ye](https://github.com/Azure/WALinuxAgent)bakın.
