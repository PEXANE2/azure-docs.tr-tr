---
title: Azure Linux VM Aracısı'na Genel Bakış
description: Sanal makinenizin Azure Kumaş Denetleyicisi ile etkileşimini yönetmek için Linux Aracısını (waagent) nasıl yükleyip yapılandırıştırmayı öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 10/17/2016
ms.author: akjosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f22fbd77069488e7aaf490f93f42cde747444a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74073866"
---
# <a name="understanding-and-using-the-azure-linux-agent"></a>Azure Linux Aracısını Anlama ve Kullanma

Microsoft Azure Linux Aracısı (waagent), Linux & FreeBSD sağlama ve Azure Kumaş Denetleyicisi ile VM etkileşimini yönetir. Azure, sağlama işlevi sağlayan Linux Aracısı'na ek olarak, bazı Linux işletim sistemi için bulut init kullanma seçeneği de sunar. Linux Aracısı, Linux ve FreeBSD IaaS dağıtımları için aşağıdaki işlevselliği sağlar:

> [!NOTE]
> Daha fazla bilgi için [README'ye](https://github.com/Azure/WALinuxAgent/blob/master/README.md)bakın.
> 
> 

* **Görüntü Sağlama**
  
  * Kullanıcı hesabı oluşturma
  * SSH kimlik doğrulama türlerini yapılandırma
  * SSH ortak anahtarlarının ve anahtar çiftlerinin dağıtımı
  * Ana bilgisayar adını ayarlama
  * Ana bilgisayar adını dns platformuna yayımlama
  * Platforma SSH ana bilgisayar anahtar parmak izini bildirme
  * Kaynak Disk Yönetimi
  * Kaynak diskini biçimlendirme ve montaj
  * Takas alanını yapılandırma
* **Ağ Oluşturma**
  
  * Platform DHCP sunucularıyla uyumluluğu artırmak için yolları yönetir
  * Ağ arabirimi adının kararlılığını sağlar
* **Çekirdek**
  
  * Sanal NUMA'yı yapılandırır (çekirdek `2.6.37`<için devre dışı)
  * /dev/random için Hyper-V entropi tüketir
  * Kök aygıt için SCSI zaman zaman larını yapılandırır (uzak olabilir)
* **Tanılama**
  
  * Seri bağlantı noktasına konsol yeniden yönlendirme
* **SCVMM Dağıtımları**
  
  * System Center Virtual Machine Manager 2012 R2 ortamında çalışırken Linux için VMM aracısını algılar ve önyüklemeler
* **VM Uzantısı**
  
  * Yazılım ve yapılandırma otomasyonuna olanak sağlamak için Microsoft ve İş Ortakları tarafından yazılan bileşeni Linux VM'ye (IaaS) enjekte edin
  * VM Uzantısı referans uygulaması[https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)

## <a name="communication"></a>İletişim
Platformdan aracıya bilgi akışı iki kanal üzerinden gerçekleşir:

* IaaS dağıtımları için önyükleme süresi eklenmiş BIR DVD. Bu DVD, gerçek SSH anahtar çiftleri dışındaki tüm sağlama bilgilerini içeren OVF uyumlu bir yapılandırma dosyası içerir.
* Dağıtım ve topoloji yapılandırmasını elde etmek için kullanılan bir REST API'sini ortaya çıkaran bir TCP uç noktası.

## <a name="requirements"></a>Gereksinimler
Aşağıdaki sistemler test edilmiştir ve Azure Linux Aracısı ile çalıştığı bilinmektedir:

> [!NOTE]
> Bu liste, burada açıklandığı gibi, Microsoft Azure Platformu'ndaki desteklenen sistemlerin resmi listesinden farklı olabilir:[https://support.microsoft.com/kb/2805216](https://support.microsoft.com/kb/2805216)
> 
> 

* CoreOS
* CentOS 6.3+
* Kırmızı Şapka Kurumsal Linux 6.7+
* Debian 7.0+
* Ubuntu 12.04+
* açıkSUSE 12.3+
* SLES 11 SP3+
* Oracle Linux 6.4+

Diğer Desteklenen Sistemler:

* FreeBSD 10+ (Azure Linux Aracısı v2.0.10+)

Linux aracısı düzgün çalışması için bazı sistem paketlerine bağlıdır:

* Python 2.6+
* OpenSSL 1.0+
* OpenSSH 5.3+
* Filesystem yardımcı programları: sfdisk, fdisk, mkfs, ayrılmış
* Şifre araçları: chpasswd, sudo
* Metin işleme araçları: sed, grep
* Ağ araçları: ip-route
* UDF filesystems montajı için çekirdek desteği.

## <a name="installation"></a>Yükleme
Dağıtımınızın paket deposundan bir RPM veya DEB paketi kullanarak yükleme, Azure Linux Aracısını yüklemek ve yükseltmek için tercih edilen yöntemdir. Onaylanan tüm [dağıtım sağlayıcıları,](../linux/endorsed-distros.md) Azure Linux aracı paketini görüntülerine ve depolarına entegre ederler.

Kaynaktan yükleme veya özel konumlar alavese veya önek gibi gelişmiş yükleme seçenekleri için [GitHub'daki Azure Linux Aracısı reposundaki](https://github.com/Azure/WALinuxAgent) belgelere bakın.

## <a name="command-line-options"></a>Komut Satırı Seçenekleri
### <a name="flags"></a>Bayraklar
* verbose: Belirtilen komutun ayrıntılılığını artırmak
* kuvvet: Bazı komutlar için etkileşimli onayı atla

### <a name="commands"></a>Komutlar
* yardım: Desteklenen komutları ve bayrakları listeler.
* deprovision: Sistemi temizlemeye ve yeniden temine uygun hale getirmeye çalışmak. Aşağıdaki işlem siler:
  
  * Tüm SSH ana bilgisayar anahtarları (Provisioning.RegenerateSshHostKeyPair yapılandırma dosyasında 'y' ise)
  * /etc/resolv.conf'ta nameserver yapılandırması
  * /etc/shadow'dan kök parolası (Provisioning.DeleteRootPassword yapılandırma dosyasında 'y' ise)
  * Önbelleğe alınmış DHCP istemci kiralamaları
  * Ana bilgisayar adını localhost.localdomain'e sıfırlar

> [!WARNING]
> Deprovisioning, görüntünün tüm hassas bilgilerden temizlendiğini ve yeniden dağıtım için uygun olduğunu garanti etmez.
> 
> 

* deprovision+user: Her şeyi -deprovision (üstte) gerçekleştirir ve ayrıca son sağlanan kullanıcı hesabını (/var/lib/waagent'dan elde edilen) ve ilişkili verileri siler. Bu parametre, daha önce Azure'da kullanılabilen bir resmin görüntünün görüntünün görüntünün ele geçirilip yeniden kullanılabilmesi için kaldırılmasıdır.
* sürüm: Waagent sürümünü görüntüler
* serialconsole: GRUB'u ttyS0(ilk seri bağlantı noktası) önyükleme konsolu olarak işaretlemek için yapılandırır. Bu, çekirdek önyükleme günlüklerinin seri bağlantı noktasına gönderilmesini ve hata ayıklama için kullanılabilir hale getirilmesini sağlar.
* daemon: platform ile etkileşimi yönetmek için bir daemon olarak waagent çalıştırın. Bu bağımsız değişken, waagent init komut dosyasında waagent için belirtilir.
* başlangıç: Arka plan işlemi olarak waagent çalıştırın

## <a name="configuration"></a>Yapılandırma
Bir yapılandırma dosyası (/etc/waagent.conf) waagent eylemlerini denetler. Aşağıda örnek bir yapılandırma dosyası gösterilmektedir:

    ```
    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.AllowResetSysUser=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None
    AutoUpdate.Enabled=y
    ```

Aşağıdaki çeşitli yapılandırma seçenekleri açıklanmıştır. Yapılandırma seçenekleri üç türden dir; Boolean, String veya Integer. Boolean yapılandırma seçenekleri "y" veya "n" olarak belirtilebilir. Özel anahtar kelime "Yok" aşağıdaki ayrıntıları olarak bazı dize türü yapılandırma girişleri için kullanılabilir:

**Provisioning.Enabled:**  
```
Type: Boolean  
Default: y
```
Bu, kullanıcının aracıdaki sağlama işlevini etkinleştirmesine veya devre dışı etmesine olanak tanır. Geçerli değerler "y" veya "n" olur. Sağlama devre dışı bırakılırsa, görüntüdeki SSH ana bilgisayarı ve kullanıcı anahtarları korunur ve Azure sağlama API'sinde belirtilen yapılandırma lar yoksayılır.

> [!NOTE]
> Parametre, `Provisioning.Enabled` sağlama için bulut init'i kullanan Ubuntu Bulut Görüntüleri'nde "n" varsayılanolarak kullanılır.
> 
> 

**Provisioning.DeleteRootPassword:**  
```
Type: Boolean  
Default: n
```
Ayarlanırsa, /etc/shadow dosyasındaki kök parola sağlama işlemi sırasında silinir.

**Provisioning.RegenerateSshHostKeyPair:**  
```
Type: Boolean  
Default: y
```
Ayarlanırsa, tüm SSH ana bilgisayar anahtar çiftleri (ecdsa, dsa ve rsa) /etc/ssh/'den sağlama işlemi sırasında silinir. Ve tek bir taze anahtar çifti oluşturulur.

Taze anahtar çifti için şifreleme türü Provisioning.SshHostKeyPairType girişi tarafından yapılandırılabilir. Bazı dağıtımlar, SSH daemon yeniden başlatıldığında (örneğin, yeniden başlatıldığında) eksik şifreleme türleri için SSH anahtar çiftlerini yeniden oluşturur.

**Provisioning.SshHostKeyPairType:**  
```
Type: String  
Default: rsa
```
Bu sanal makinede SSH daemon tarafından desteklenen bir şifreleme algoritması türüne ayarlanabilir. Genellikle desteklenen değerler "rsa", "dsa" ve "ecdsa"dır. Windows'daki "macun.exe" "ecdsa"yı desteklemez. Bu nedenle, Bir Linux dağıtımına bağlanmak için Windows'da macun.exe kullanmayı planlıyorsanız, "rsa" veya "dsa" kullanın.

**Provisioning.MonitorHostName:**  
```
Type: Boolean  
Default: y
```
Ayarlanırsa, waagent ana bilgisayar adı değişiklikleri için Linux sanal makinesini izler ("hostname" komutu yla döndürülür) ve değişikliği yansıtacak şekilde görüntüdeki ağ yapılandırmasını otomatik olarak güncelleştirir. Ad değişikliğini DNS sunucularına itmek için sanal makinede ağ yeniden başlatılır. Bu, Internet bağlantısının kısa sürede kaybolmasına neden olabilir.

**Provisioning.DecodeCustomData**  
```
Type: Boolean  
Default: n
```
Ayarlanırsa, waagent Base64'ten CustomData'yı çözer.

**Provisioning.ExecuteCustomData**  
```
Type: Boolean  
Default: n
```
Ayarlanırsa, waagent sağlama dan sonra CustomData yürütür.

**Provisioning.AllowResetSysUser**
```
Type: Boolean
Default: n
```
Bu seçenek, sys kullanıcısının parolasının sıfırlanmasına izin verir; varsayılan devre dışı bırakılır.

**Provisioning.PasswordCryptId**  
```
Type: String  
Default: 6
```
Parola karma oluştururken crypt tarafından kullanılan algoritma.  
 1 - MD5  
 2a - Balon balığı  
 5 - SHA-256  
 6 - SHA-512  

**Provisioning.PasswordCryptSaltLength**  
```
Type: String  
Default: 10
```
Parola karma oluştururken kullanılan rasgele tuz uzunluğu.

**ResourceDisk.Format:**  
```
Type: Boolean  
Default: y
```
Ayarlanırsa, "ResourceDisk.Filesystem"de kullanıcı tarafından istenen dosya sistemi türü "ntfs"ten başka bir şeyse, platform tarafından sağlanan kaynak diski waagent tarafından biçimlendirilir ve monte edilir. Linux (83) türünde tek bir bölüm diskte kullanılabilir hale getirilir. Bu bölüm başarıyla monte edilebiliyorsa biçimlendirilmemiştir.

**ResourceDisk.Filesystem:**  
```
Type: String  
Default: ext4
```
Bu, kaynak diski için dosya sistemi türünü belirtir. Desteklenen değerler Linux dağıtımına göre değişir. Dize X ise, mkfs. X Linux görüntü üzerinde mevcut olmalıdır. SLES 11 görüntüleri genellikle 'ext3' kullanmalıdır. FreeBSD görüntüleri burada 'ufs2' kullanmalısınız.

**ResourceDisk.MountPoint:**  
```
Type: String  
Default: /mnt/resource 
```
Bu, kaynak diskinin monte edildiği yolu belirtir. Kaynak *diskgeçici* bir disktir ve VM deprovisioned olduğunda boşaltılabilir.

**ResourceDisk.MountOptions**  
```
Type: String  
Default: None
```
Mount -o komutuna geçirilecek disk montaj seçeneklerini belirtir. Bu, virgülle ayrılmış bir değerler listesi, ör. 'nodev, nosuid'. Ayrıntılar için montaj(8) bilgisine bakın.

**ResourceDisk.EnableSwap:**  
```
Type: Boolean  
Default: n
```
Ayarlanırsa, kaynak diskinde bir takas dosyası (/swapfile) oluşturulur ve sistem takas alanına eklenir.

**ResourceDisk.SwapSizeMB:**  
```
Type: Integer  
Default: 0
```
Megabayt takas dosyasının boyutu.

**Logs.Verbose:**  
```
Type: Boolean  
Default: n
```
Ayarlanırsa, günlük ayrıntılılığı artırılır. Waagent /var/log/waagent.log'a günlük ler ve günlükleri döndürmek için sistem günlük döndürme işlevini kullanır.

**Os. ETKINLEŞTIRMERDMA**  
```
Type: Boolean  
Default: n
```
Ayarlanırsa, aracı, dayanak donanımdaki firmware sürümüyle eşleşen bir RDMA çekirdeği sürücüsü yüklemeye ve yüklemeye çalışır.

**Os. RootDeviceScsiTimeout:**  
```
Type: Integer  
Default: 300
```
Bu ayar, SCSI zaman öncesini işletim sistemi diski ve veri sürücülerinde saniyeler içinde yapılandırır. Ayaredilmezse, sistem varsayılanları kullanılır.

**Os. OpensslPath:**  
```
Type: String  
Default: None
```
Bu ayar, şifreleme işlemleri için kullanılacak openssl ikilisi için alternatif bir yol belirtmek için kullanılabilir.

**HttpProxy.Host, HttpProxy.Port**  
```
Type: String  
Default: None
```
Ayarlanırsa, aracı internete erişmek için bu proxy sunucusunu kullanır. 

**Otomatik Güncelleştirme.Etkin**
```
Type: Boolean
Default: y
```
Hedef durumu işleme için otomatik güncelleştirmeyi etkinleştirme veya devre dışı kılabilir; varsayılan etkindir.



## <a name="ubuntu-cloud-images"></a>Ubuntu Bulut Görüntüleri
Ubuntu Cloud Images, aksi takdirde Azure Linux Aracısı tarafından yönetilecek birçok yapılandırma görevini gerçekleştirmek için [bulut init'i](https://launchpad.net/ubuntu/+source/cloud-init) kullanır. Aşağıdaki farklar geçerlidir:

* **Provisioning.Enabled** varsayılan "n" ubuntu Bulut Görüntüleri olan bu sağlama görevleri gerçekleştirmek için bulut init kullanın.
* Aşağıdaki yapılandırma parametrelerinin, kaynak diskini yönetmek ve alanı değiştirmek için bulut init kullanan Ubuntu Bulut Görüntüleri üzerinde hiçbir etkisi yoktur:
  
  * **ResourceDisk.Format**
  * **ResourceDisk.Filesystem**
  * **ResourceDisk.MountPoint**
  * **ResourceDisk.EnableSwap**
  * **ResourceDisk.SwapSizeMB**

* Daha fazla bilgi için, kaynak diski montaj noktasını yapılandırmak ve sağlama sırasında Ubuntu Bulut Görüntüleri'nde alanı değiştirmek için aşağıdaki kaynaklara bakın:
  
  * [Ubuntu Wiki: Swap Bölümlerini Yapılandır](https://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [Azure Sanal Makinesine Özel Veri Enjekte Etme](../windows/classic/inject-custom-data.md)

