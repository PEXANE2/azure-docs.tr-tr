---
title: Azure HPC Önbellek NFS depolama hedeflerini sorun giderme
description: NFS depolama hedefi oluştururken hataya neden olabilecek yapılandırma hatalarını ve diğer sorunları önlemek ve düzeltmek için ipuçları
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: rohogue
ms.openlocfilehash: c88ffb9e87bc0688cc87b816efaa8e101e23407c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77652093"
---
# <a name="troubleshoot-nas-configuration-and-nfs-storage-target-issues"></a>SORUN GIDERme NAS yapılandırması ve NFS depolama hedef sorunları

Bu makalede, bazı yaygın yapılandırma hataları ve Azure HPC Önbelleğinin bir depolama hedefi olarak NFS depolama sistemi eklemesini engelleyebilecek diğer sorunlar için çözümler sağlar.

Bu makalede, bağlantı noktalarının nasıl denetlenen ve bir NAS sistemine kök erişiminin nasıl etkinleştirilen hakkında ayrıntılar yer almaktadır. Ayrıca, NFS depolama hedef oluşturma başarısız neden olabilir daha az yaygın sorunlar hakkında ayrıntılı bilgi içerir.

> [!TIP]
> Bu kılavuzu kullanmadan önce, [NFS depolama hedefleri için ön koşulları](hpc-cache-prereqs.md#nfs-storage-requirements)okuyun.

Sorununuzun çözümü burada yer almazsa, Microsoft Service ve Support'un sorunu araştırmak ve çözmek için sizinle çalışabilmesi için [lütfen bir destek bileti açın.](hpc-cache-support-ticket.md)

## <a name="check-port-settings"></a>Bağlantı noktası ayarlarını kontrol edin

Azure HPC Önbelleği, arka uç NAS depolama sistemindeki birkaç UDP/TCP bağlantı noktasının okuma/yazma erişimine ihtiyaç duyar. Bu bağlantı noktalarına NAS sisteminde erişilebilir olduğundan ve depolama sistemi ve önbellek alt ağı arasındaki güvenlik duvarları aracılığıyla bu bağlantı noktalarına trafiğe izin verildiğinden emin olun. Bu yapılandırmayı doğrulamak için veri merkeziniz için güvenlik duvarı ve ağ yöneticileriyle çalışmanız gerekebilir.

Bağlantı noktaları farklı satıcılardan gelen depolama sistemleri için farklıdır, bu nedenle depolama hedefi ayarlarken sisteminizin gereksinimlerini kontrol edin.

Genel olarak, önbelleğin bu bağlantı noktalarına erişmesi gerekir:

| Protokol | Bağlantı noktası  | Hizmet  |
|----------|-------|----------|
| TCP/UDP  | 111   | rpcbind  |
| TCP/UDP  | 2049  | NFS      |
| TCP/UDP  | 4045  | nlockmgr |
| TCP/UDP  | 4046  | monte   |
| TCP/UDP  | 4047  | durum   |

Sisteminiz için gereken belirli bağlantı noktalarını ``rpcinfo`` öğrenmek için aşağıdaki komutu kullanın. Aşağıdaki komut, bağlantı noktalarını listeler ve ilgili sonuçları bir tabloda biçimlendirir. (storage_IP>terimi yerine sisteminiz *<in* IP adresini kullanın.)

Bu komutu, NFS altyapısı yüklü olan herhangi bir Linux istemcisinden çıkarabilirsiniz. Küme alt ağı içinde bir istemci kullanıyorsanız, alt ağ ve depolama sistemi arasındaki bağlantıyı doğrulamaya da yardımcı olabilir.

```bash
rpcinfo -p <storage_IP> |egrep "100000\s+4\s+tcp|100005\s+3\s+tcp|100003\s+3\s+tcp|100024\s+1\s+tcp|100021\s+4\s+tcp"| awk '{print $4 "/" $3 " " $5}'|column -t
```

``rpcinfo`` Sorgu tarafından döndürülen tüm bağlantı noktalarının Azure HPC Önbelleği'nin alt netinden sınırsız trafiğe izin verdiğinden emin olun.

Bu ayarları hem NAS'ın kendisinde hem de depolama sistemi ile önbellek alt ağı arasındaki güvenlik duvarlarında denetleyin.

## <a name="check-root-access"></a>Kök erişimini denetleme

Azure HPC Önbelleği, depolama hedefini oluşturmak için depolama sisteminizin dışa aktarımlarına erişmesi gerekir. Özellikle, kullanıcı kimliği 0 olarak dışa aktarma bağlar.

Farklı depolama sistemleri bu erişimi sağlamak için farklı yöntemler kullanır:

* Linux sunucuları ``no_root_squash`` genellikle dışa ``/etc/exports``aktarılan yola ekler.
* NetApp ve EMC sistemleri genellikle belirli IP adreslerine veya ağlara bağlı ihracat kurallarıyla erişimi denetler.

Dışa aktarma kuralları kullanıyorsanız, önbelleğin önbellek alt netinden birden çok farklı IP adresi kullanabileceğini unutmayın. Tüm olası subnet IP adreslerinden erişime izin verin.

Önbellek için doğru erişim düzeyini etkinleştirmek için NAS depolama satıcınızla birlikte çalışın.

### <a name="allow-root-access-on-directory-paths"></a>Dizin yollarında kök erişimine izin verme
<!-- linked in prereqs article -->

Hiyerarşik dizinler dışa aktaran NAS sistemleri için Azure HPC Önbelleği'nin her dışa aktarım düzeyine kök erişimi gerekir.

Örneğin, bir sistem şu gibi üç dışa aktarma gösterebilir:

* ``/ifs``
* ``/ifs/accounting``
* ``/ifs/accounting/payroll``

İhracat ``/ifs/accounting/payroll`` bir çocuk ``/ifs/accounting``, ``/ifs/accounting`` ve kendisi bir ``/ifs``çocuktur .

Dışa aktarmayı ``payroll`` Bir HPC önbellek depolama hedefi olarak ``/ifs/`` eklerseniz, önbellek aslında bağlanır ve bordro dizinine buradan erişer. Bu nedenle Azure HPC Önbelleği'nin ``/ifs/accounting/payroll`` dışa aktarışa erişmek için kök erişimine ``/ifs`` ihtiyacı vardır.

Bu gereksinim, depolama sisteminin sağladığı dosya tutamaçları kullanılarak önbelleğin dosyaları dizinlere ve dosya çakışmasını önleme biçimiyle ilgilidir.

Hiyerarşik dışa aktarımlara sahip bir NAS sistemi, dosya farklı dışa aktarımlardan retrieveedilirse, aynı dosya için farklı dosya tutamaçları verebilir. Örneğin, bir istemci ``/ifs/accounting`` dosyayı monte ``payroll/2011.txt``edebilir ve erişebilir. Başka bir ``/ifs/accounting/payroll`` istemci dosyayı ``2011.txt``bağlar ve erişir. Depolama sisteminin dosyayı nasıl atadığına bağlı olarak, bu iki istemci aynı dosyayı ``<mount2>/payroll/2011.txt`` farklı dosya ``<mount3>/2011.txt``işletmeleriyle (biri için ve diğeri için) alabilir.

Arka uç depolama sistemi dosya tanıtıcıları için dahili diğer adları tutar, ancak Azure HPC Önbelleği dizin başvurusunda hangi dosyanın aynı öğeyi işaret ettiğini söyleyemez. Bu nedenle önbelleğin aynı dosya için önbelleğe alınmış farklı yazmaları olabilir ve aynı dosya olduğunu bilmediği için değişiklikleri yanlış uygulayabilir.

Azure HPC Önbelleği, birden çok dışaktaki dosyalar için bu olası dosya çakışmasını önlemek için, otomatik olarak yoldaki en sığ kullanılabilir dışa aktarıma (örnekte)``/ifs`` bağlanır ve bu dışa aktarmadan verilen dosya tutamacını kullanır. Birden çok dışak aynı temel yolu kullanıyorsa, Azure HPC Önbelleği'nin bu yola kök erişimi gerekir.

## <a name="enable-export-listing"></a>İhracat girişini etkinleştirme
<!-- link in prereqs article -->

Azure HPC Önbelleği sorguladığında NAS dışa aktarımlarını listelemelidir.

Çoğu NFS depolama sisteminde, bir Linux istemcisinden aşağıdaki sorguyu göndererek bunu test edebilirsiniz:``showmount -e <storage IP address>``

Mümkünse önbelleğinizle aynı sanal ağdan bir Linux istemcisi kullanın.

Bu komut dışa aktarımları listelemiyorsa, önbellek depolama sisteminize bağlanmada sorun olur. Dışa aktarma girişini etkinleştirmek için NAS satıcınızla birlikte çalışın.

## <a name="adjust-vpn-packet-size-restrictions"></a>VPN paket boyutu kısıtlamalarını ayarlama
<!-- link in prereqs article -->

Önbellek ve NAS aygıtınız arasında VPN varsa, VPN tam boyutlu 1500 baytlık Ethernet paketlerini engelleyebilir. NAS ve Azure HPC Önbellek örneği arasındaki büyük değişimler tamamlanmazsa, ancak daha küçük güncelleştirmeler beklendiği gibi çalışırsa bu sorunla karşı laşabilirsiniz.

VPN yapılandırmanızın ayrıntılarını bilmediğiniz sürece sisteminizde bu sorun olup olmadığını anlamanın basit bir yolu yoktur. Bu sorunu denetlemenize yardımcı olabilecek birkaç yöntem aşağıda verilmiştir.

* Hangi paketlerin başarılı bir şekilde aktarım yaptığını algılamak için VPN'in her iki tarafındaki paket algılayıcılarını kullanın.
* VPN'iniz ping komutlarına izin veriyorsa, tam boyutlu bir paket göndermeyi test edebilirsiniz.

  Bu seçenekleri ile NAS VPN üzerinden bir ping komutu çalıştırın. (<*storage_IP>* değeri yerine depolama sisteminizin IP adresini kullanın.)

   ```bash
   ping -M do -s 1472 -c 1 <storage_IP>
   ```

  Komuttaki seçenekler şunlardır:

  * ``-M do``- Parçalamayın
  * ``-c 1``- Yalnızca bir paket gönder
  * ``-s 1472``- Yükün boyutunu 1472 bayt olarak ayarlayın. Bu, Ethernet yükü için muhasebe yaptıktan sonra 1500 baytlık bir paket için maksimum boyut yüküdür.

  Başarılı bir yanıt şöyle görünür:

  ```bash
  PING 10.54.54.11 (10.54.54.11) 1472(1500) bytes of data.
  1480 bytes from 10.54.54.11: icmp_seq=1 ttl=64 time=2.06 ms
  ```

  Ping 1472 bayt ile başarısız olursa, uzak sistemin maksimum kare boyutunu düzgün bir şekilde algılaması için VPN'de MSS bağlamayı yapılandırmanız gerekebilir. Daha fazla bilgi edinmek için [VPN Ağ Geçidi IPsec/IKE parametreleri belgelerini](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) okuyun.

## <a name="check-for-acl-security-style"></a>ACL güvenlik stilini denetleyin

Bazı NAS sistemleri, erişim denetim listelerini (ALA)'leri geleneksel POSIX veya UNIX güvenliğiyle birleştiren karma bir güvenlik stili kullanır.

Sisteminiz güvenlik stilini "ACL" kısaltması olmadan UNIX veya POSIX olarak bildiriyorsa, bu sorun sizi etkilemez.

ALA'lar kullanan sistemler için Azure HPC Önbelleğinin dosya erişimini denetlemek için kullanıcıya özel ek değerleri izlemesi gerekir. Bu, bir erişim önbelleği etkinleştirilerek yapılır. Erişim önbelleğini açmak için kullanıcıya yönelik bir denetim yoktur, ancak önbellek sisteminizde etkilenen depolama hedefleri için etkinleştirilmesini istemek için bir destek bileti açabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede ele alınmayan bir sorununuzun varsa, uzman yardımı almak için [bir destek bileti açın.](hpc-cache-support-ticket.md)
