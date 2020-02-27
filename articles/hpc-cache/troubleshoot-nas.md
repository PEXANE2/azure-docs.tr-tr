---
title: Azure HPC Cache NFS depolama hedefleri sorunlarını giderme
description: NFS depolama hedefi oluştururken hata oluşmasına neden olabilecek yapılandırma hatalarını ve diğer sorunları önlemek ve onarmak için ipuçları
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: rohogue
ms.openlocfilehash: c88ffb9e87bc0688cc87b816efaa8e101e23407c
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77652093"
---
# <a name="troubleshoot-nas-configuration-and-nfs-storage-target-issues"></a>NAS yapılandırma ve NFS depolama hedefi sorunlarını giderme

Bu makalede, bazı yaygın yapılandırma hataları ve Azure HPC önbelleğinin depolama hedefi olarak NFS depolama sistemi eklemesini engelleyebilecek diğer sorunlar için çözümler sunulmaktadır.

Bu makale, bağlantı noktalarını denetleme ve bir NAS sistemine kök erişimin nasıl etkinleştirileceği hakkındaki ayrıntıları içerir. Ayrıca, NFS depolama hedefi oluşturmanın başarısız olmasına neden olabilecek daha az yaygın sorunlar hakkında ayrıntılı bilgiler içerir.

> [!TIP]
> Bu kılavuzu kullanmadan önce, [NFS depolama hedefleri için önkoşulları](hpc-cache-prereqs.md#nfs-storage-requirements)okuyun.

Sorununuza yönelik çözüm buraya eklenmiyorsa, Microsoft hizmet ve destek 'in sorunu araştırmak ve çözmek için sizinle çalışabilmesi için lütfen [bir destek bileti açın](hpc-cache-support-ticket.md) .

## <a name="check-port-settings"></a>Bağlantı noktası ayarlarını denetle

Azure HPC Cache, arka uç NAS depolama sisteminde birkaç UDP/TCP bağlantı noktasına okuma/yazma erişimi gerektirir. Bu bağlantı noktalarına, NAS sisteminde erişilebildiğinden ve ayrıca, depolama sistemi ile önbellek alt ağı arasındaki güvenlik duvarları aracılığıyla bu bağlantı noktalarına giden trafiğe izin verildiğini doğrulayın. Bu yapılandırmayı doğrulamak için, veri merkezinize yönelik güvenlik duvarı ve ağ yöneticileri ile çalışmanız gerekebilir.

Bağlantı noktaları farklı satıcıların depolama sistemleri için farklıdır, bu nedenle bir depolama hedefi ayarlarken sisteminizin gereksinimlerini denetleyin.

Genellikle önbelleğin Bu bağlantı noktalarına erişmesi gerekir:

| Protokol | Bağlantı noktası  | Hizmet  |
|----------|-------|----------|
| TCP/UDP  | 111   | rpcbind  |
| TCP/UDP  | 2049  | NFS      |
| TCP/UDP  | 4045  | nlockmgr |
| TCP/UDP  | 4046  | dağtd   |
| TCP/UDP  | 4047  | durum   |

Sisteminiz için gerekli olan belirli bağlantı noktalarını öğrenmek için aşağıdaki ``rpcinfo`` komutunu kullanın. Aşağıdaki komut, bağlantı noktalarını listeler ve ilgili sonuçları bir tabloyla biçimlendirir. ( *< Storage_IP >* terimi yerıne sisteminizin IP adresini kullanın.)

Bu komutu, NFS altyapısının yüklü olduğu herhangi bir Linux istemcisinden verebilirsiniz. Küme alt ağı içinde bir istemci kullanırsanız, alt ağ ve depolama sistemi arasındaki bağlantıyı doğrulamaya da yardımcı olabilir.

```bash
rpcinfo -p <storage_IP> |egrep "100000\s+4\s+tcp|100005\s+3\s+tcp|100003\s+3\s+tcp|100024\s+1\s+tcp|100021\s+4\s+tcp"| awk '{print $4 "/" $3 " " $5}'|column -t
```

``rpcinfo`` sorgusunun döndürdüğü tüm bağlantı noktalarının Azure HPC önbelleğinin alt ağından sınırsız trafiğe izin verdiği şekilde emin olun.

Bu ayarları hem NAS hem de depolama sistemi ve önbellek alt ağı arasındaki tüm güvenlik duvarları üzerinde denetleyin.

## <a name="check-root-access"></a>Kök erişimi denetle

Depolama hedefini oluşturmak için Azure HPC önbelleğinin depolama sisteminizin dışarı aktarımlarının erişimine ihtiyacı vardır. Özellikle, dışarı aktarmaları Kullanıcı KIMLIĞI 0 olarak takar.

Farklı depolama sistemleri bu erişimi etkinleştirmek için farklı yöntemler kullanır:

* Linux sunucuları genel olarak ``no_root_squash`` ``/etc/exports``' de dışarıya alınan yola ekler.
* NetApp ve EMC Sistemleri, genellikle belirli IP adreslerine veya ağlara bağlı olan dışa aktarma kurallarıyla erişimi denetler.

Dışarı aktarma kuralları kullanılıyorsa, önbelleğin önbellek alt ağından birden çok farklı IP adresi kullandığını unutmayın. Olası alt ağ IP adreslerinden oluşan tam aralıktan erişime izin verin.

Önbellek için doğru erişim düzeyini etkinleştirmek üzere, NAS depolama satıcınızla birlikte çalışın.

### <a name="allow-root-access-on-directory-paths"></a>Dizin yollarında kök erişimine izin ver
<!-- linked in prereqs article -->

Hiyerarşik dizinleri dışarı veren NAS sistemlerinde, Azure HPC Cache her bir dışarı aktarma düzeyine kök erişimi gerektirir.

Örneğin, bir sistem aşağıdaki gibi üç dışarı aktarma gösterebilir:

* ``/ifs``
* ``/ifs/accounting``
* ``/ifs/accounting/payroll``

Dışarı aktarma ``/ifs/accounting/payroll``, ``/ifs/accounting``bir alt öğesidir ve ``/ifs/accounting`` kendi ``/ifs``alt öğesidir.

``payroll`` dışarı aktarmayı bir HPC önbellek depolama hedefi olarak eklerseniz, önbellek aslında ``/ifs/`` takar ve buradan Bordro dizinine erişir. Bu nedenle, ``/ifs/accounting/payroll`` dışarı aktarmaya erişmek için Azure HPC önbelleğinin ``/ifs`` için kök erişimi olması gerekir.

Bu gereksinim, önbelleğin dosyaları dizinlediği ve dosya çakışmalarını önleyen, depolama sisteminin sağladığı Dosya tutamaçları ile ilgilidir.

Hiyerarşik dışarı aktarmalar içeren bir NAS sistemi, dosya farklı dışarı aktarımlardan alınırsa aynı dosya için farklı dosya tanıtıcıları verebilir. Örneğin, bir istemci ``/ifs/accounting`` bağlayabilir ve dosyaya ``payroll/2011.txt``erişebilir. Başka bir istemci ``/ifs/accounting/payroll`` takar ve dosyaya erişir ``2011.txt``. Depolama sisteminin dosya tanıtıcılarını nasıl atamadığına bağlı olarak, bu iki istemci farklı Dosya tanıtıcılarla aynı dosyayı alabilir (``<mount2>/payroll/2011.txt`` için bir diğeri ``<mount3>/2011.txt``için).

Arka uç depolama sistemi, dosya tanıtıcıları için iç diğer adları tutar, ancak Azure HPC önbelleği, dizinindeki hangi dosya tanıtıcılarının aynı öğeye başvurulacağını söyleyebilir. Bu nedenle, önbelleğin aynı dosya için önbelleğe alınmış farklı yazmaları olabilir ve aynı dosya olduğunu bilmez çünkü değişiklikleri yanlış bir şekilde uygulayabilir.

Birden çok dışarı aktarmada bulunan dosyalar için bu olası dosya çarpışmasını önlemek için Azure HPC Cache, yoldaki (örnekteki``/ifs``) otomatik olarak kullanılabilir dışarı aktarmayı otomatik olarak bağlar ve bu dışarı aktarma işleminden verilen dosya tanıtıcısını kullanır. Birden çok dışa aktarma aynı temel yolu kullanıyorsa, Azure HPC Cache 'in bu yola kök erişimi olması gerekir.

## <a name="enable-export-listing"></a>Dışarı aktarma listesini etkinleştir
<!-- link in prereqs article -->

Azure HPC Cache tarafından sorgulandığında NAS 'ıN dışarı aktarmaları listemalıdır.

Çoğu NFS depolama sisteminde, bir Linux istemcisinden aşağıdaki sorguyu göndererek bunu test edebilirsiniz: ``showmount -e <storage IP address>``

Mümkünse önbelleğiniz ile aynı sanal ağdaki bir Linux istemcisini kullanın.

Bu komut dışarı aktarmaları listemezse, önbellekte depolama sisteminize bağlanma sorunu olur. Dışarı aktarma listesini etkinleştirmek için NAS satıcınızla birlikte çalışın.

## <a name="adjust-vpn-packet-size-restrictions"></a>VPN paket boyutu kısıtlamalarını ayarla
<!-- link in prereqs article -->

Önbellek ve NAS cihazınız arasında bir VPN 'niz varsa VPN, tam boyutlu 1500 baytlık Ethernet paketlerini engelleyebilir. NAS ve Azure HPC önbellek örneği arasında büyük değişimler tamamlanmadığında bu sorunla karşılaşabilirsiniz, ancak daha küçük güncelleştirmeler beklendiği gibi çalışır.

VPN yapılandırmanızın ayrıntılarını bilmiyorsanız sisteminizde bu soruna sahip olup olmadığını belirlemenin basit bir yolu yoktur. Bu sorunu denetlemeye yardımcı olabilecek birkaç yöntem aşağıda verilmiştir.

* Hangi paketlerin başarıyla aktarılmasını algılamak için VPN 'nin her iki tarafında da paket algılayıcılar kullanın.
* VPN 'niz ping komutlarına izin veriyorsa, tam boyutlu bir paket göndermeyi test edebilirsiniz.

  Bu seçeneklerle, NAS için VPN üzerinden bir ping komutu çalıştırın. ( *< Storage_IP >* değeri yerine depolama sisteminizin IP adresini kullanın.)

   ```bash
   ping -M do -s 1472 -c 1 <storage_IP>
   ```

  Bunlar, komutundaki seçeneklerdir:

  * ``-M do``-parçalara ayırma
  * ``-c 1``-yalnızca bir paket gönder
  * ``-s 1472``-yükün boyutunu 1472 bayt olarak ayarlayın. Bu, Ethernet ek yükü için hesap oluşturulduktan sonra 1500 baytlık bir paketin en büyük boyut yüküdür.

  Başarılı bir yanıt şöyle görünür:

  ```bash
  PING 10.54.54.11 (10.54.54.11) 1472(1500) bytes of data.
  1480 bytes from 10.54.54.11: icmp_seq=1 ttl=64 time=2.06 ms
  ```

  Ping, 1472 bayt ile başarısız olursa, uzak sistemin en büyük çerçeve boyutunu düzgün bir şekilde algılamasını sağlamak için VPN üzerinde yüklenmi 'yi yapılandırmanız gerekebilir. Daha fazla bilgi için [VPN Gateway IPSec/IKE parametreleri belgelerini](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) okuyun.

## <a name="check-for-acl-security-style"></a>ACL güvenlik stilini denetle

Bazı NAS sistemleri, erişim denetim listelerini (ACL 'Ler) geleneksel POSIX veya UNIX güvenliği ile birleştiren karma bir güvenlik stili kullanır.

Sisteminiz, güvenlik stilini "ACL" kısaltmasının dahil edilmesi gerekmeden UNIX veya POSIX olarak raporladığında, bu sorun sizi etkilemez.

ACL kullanan sistemlerde, Azure HPC Cache 'in dosya erişimini denetlemek için kullanıcıya özgü ek değerleri izlemesi gerekir. Bu işlem, erişim önbelleği etkinleştirilerek yapılır. Erişim önbelleğini açmak için kullanıcıya yönelik bir denetim yoktur, ancak önbellek sisteminizdeki etkilenen depolama hedefleri için etkinleştirilmesini istemek üzere bir destek bileti açabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede açıklanmayan bir sorununuz varsa, uzman yardımı almak için [bir destek bileti açın](hpc-cache-support-ticket.md) .
