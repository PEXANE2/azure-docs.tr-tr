---
title: Cloud-init anlama
description: Cloud-init kullanarak bir Azure VM sağlamayı anlamak için derin bakış.
author: danielsollondon
ms.service: virtual-machines-linux
ms.subservice: imaging
ms.topic: conceptual
ms.date: 06/22/2020
ms.author: danis
ms.reviewer: cynthn
ms.openlocfilehash: 731cb79096de4af2864060e7a665ac54b6581418
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307010"
---
# <a name="diving-deeper-into-cloud-init"></a>Cloud-init ' i daha ayrıntılı hale döndürme
[Cloud-init](https://cloudinit.readthedocs.io/en/latest/index.html) hakkında daha fazla bilgi edinmek veya daha derin bir düzeyde sorun gidermek için nasıl çalıştığını anlamanız gerekir. Bu belge önemli bölümleri vurgular ve Azure özelliklerini açıklar.

Cloud-init, genelleştirilmiş bir görüntüye dahil edildiğinde ve bu görüntüden bir VM oluşturulduğunda, bu, konfigürasyonları işler ve ilk önyükleme sırasında 5 aşamadan sonra çalışır. Bu aşamalar, bulut-init ' in hangi noktada yapılandırmaların uygulanacağını gösterir. 


## <a name="understand-cloud-init-configuration"></a>Cloud-Init yapılandırmasını anlama
Bir sanal makineyi bir platformda çalışacak şekilde yapılandırmak, Cloud-Init ' in bir görüntü tüketicisi olarak birden çok yapılandırmayı uygulaması için, `User data` (CustomData), birden çok biçimi destekleyen ana yapılandırmaların, [burada](https://cloudinit.readthedocs.io/en/latest/topics/format.html#user-data-formats)belgelendiği anlamına gelir. Ek yapılandırma için komut dosyaları ekleme ve çalıştırma (/var/lib/Cloud/Scripts) özelliğine sahip olursunuz, aşağıda bunun daha ayrıntılı bir şekilde açıklanmaktadır.

Bazı konfigürasyonlar, Cloud-init ile birlikte gelen Azure Marketi görüntülerine zaten bakmış, örneğin:

* bulut veri kaynağı-Cloud-init bulut platformlarıyla etkileşime girebilen kodu içerir ve bunlar ' veri kaynakları ' olarak adlandırılır. [Azure](https://cloudinit.readthedocs.io/en/latest/topics/datasources/azure.html#azure)'daki bir Cloud-init GÖRÜNTÜSÜNDEN bir VM oluşturulduğunda, Cloud-INIT, VM 'ye özgü yapılandırmayı almak için Azure meta veri uç noktalarıyla etkileşime geçerek Azure veri kaynağını yükler.
* Görüntü yapılandırması (/etc/Cloud)
* Çalışma zamanı yapılandırması (/Run/Cloud-init), `/etc/cloud/cloud.cfg` gibi `/etc/cloud/cloud.cfg.d/*.cfg` . Bunun Azure 'da kullanıldığı bir örnek olarak, Cloud-init ' i içeren Linux işletim sisteminin, Cloud-init ile hangi veri kaynağına kullanması gerektiğini belirten bir Azure veri kaynağı için yaygın bir örnektir, bu da Cloud-init süresini kaydeder:

   ```bash
   /etc/cloud/cloud.cfg.d# cat 90_dpkg.cfg
   # to update this file, run dpkg-reconfigure cloud-init
   datasource_list: [ Azure ]
   ```


## <a name="cloud-init-boot-stages-processing-configuration"></a>Cloud-init önyükleme aşamaları (işleme yapılandırması)

Cloud-init ile sağlandığınızda,, yapılandırmayı işleyen ve günlüklerde gösterilen 5 ' in önyüklemesi vardır.

1. [Oluşturucu aşaması](https://cloudinit.readthedocs.io/en/latest/topics/boot.html#generator): Cloud-init systemd üreticisi başlar ve Cloud-init ' in önyükleme hedeflerine dahil edilip edilmediğini belirler ve gerekliyse, Cloud-init ' i etkinleştirilir. Örneğin, Cloud-init ' i devre dışı bırakmak istiyorsanız, bu dosyayı oluşturabilirsiniz `/etc/cloud/cloud-init.disabled` .

2. [Cloud-Init yerel aşaması](https://cloudinit.readthedocs.io/en/latest/topics/boot.html#local): burada Cloud-Init, Azure ile Cloud-init arabirimini etkinleştiren yerel "Azure" veri kaynağını arar ve geri dönüş dahil bir ağ yapılandırması uygular.

3. [Cloud-init Init aşama (ağ)](https://cloudinit.readthedocs.io/en/latest/topics/boot.html#network): ağ çevrimiçi OLMALıDıR ve NIC ve yol tablosu bilgilerinin oluşturulması gerekir. Bu aşamada, `cloud_init_modules` /etc/Cloud/Cloud.exe ' de listelenen modüller çalıştırılır. Azure 'daki VM bağlanır, kısa ömürlü disk biçimlendirilir, ana bilgisayar adı diğer görevlerle birlikte ayarlanır.

   Cloud_init_modules bazıları şunlardır:
   - `migrator`
   - `seed_random`
   - `bootcmd`
   - `write-files`
   - `growpart`
   - `resizefs`
   - `disk_setup`
   - `mounts`
   - `set_hostname`
   - `update_hostname`
   - `ssh`


   Bu aşamada, Cloud-init, VM 'nin başarıyla sağlandığını Azure platformunda işaret eder. Bazı modüller başarısız olmuş olabilir, tüm modül hataları sağlama hatasına neden olmaz.

4. [Cloud-Init yapılandırma aşaması](https://cloudinit.readthedocs.io/en/latest/topics/boot.html#config): Bu aşamada, `cloud_config_modules` /etc/Cloud/Cloud.exe ' de tanımlanan ve listelenen modüller çalıştırılır.


5. [Cloud-Init son aşaması](https://cloudinit.readthedocs.io/en/latest/topics/boot.html#final): Bu son aşamada, `cloud_final_modules` /etc/Cloud/Cloud.exe ' de listelenen modüller çalıştırılır. Burada, paket yüklemeleri ve çalıştırma betikleri gibi önyükleme işlemi çalıştırmasında geç çalıştırılması gereken modüller. 

   -   Bu aşamada, komut dosyalarını aşağıdaki dizinlere yerleştirerek çalıştırabilirsiniz `/var/lib/cloud/scripts` :
   - `per-boot`-Bu dizin içindeki betikler, her yeniden başlatmada çalıştırılır
   - `per-instance`-Yeni bir örnek ilk kez önyüklendiğinde bu dizin içindeki betikler çalıştırılır
   - `per-once`-Bu dizin içindeki betikler yalnızca bir kez çalıştırılır

## <a name="next-steps"></a>Sonraki adımlar

[Cloud-Init sorunlarını giderme](cloud-init-troubleshooting.md).
