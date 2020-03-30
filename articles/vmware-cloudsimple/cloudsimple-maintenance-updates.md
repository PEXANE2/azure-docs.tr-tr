---
title: CloudSimple bakım ve güncellemeleri
titleSuffix: Azure VMware Solution by CloudSimple
description: Zamanlanmış bakım ve güncelleştirmeler için CloudSimple servis işlemini açıklar
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 826fae1123b355a4143118b53ba649f0939acaf7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025036"
---
# <a name="cloudsimple-maintenance-and-updates"></a>CloudSimple bakım ve güncellemeleri

Özel Bulut ortamı, tek bir hata noktası olmayacak şekilde tasarlanmıştır.

* ESXi kümeleri vSphere High Availability (HA) ile yapılandırılır. Kümeler esneklik için en az bir yedek düğüm olacak şekilde boyutlandırılır.
* Yedekli birincil depolama, tek bir hataya karşı koruma sağlamak için en az üç düğüm gerektiren vSAN tarafından sağlanır. vSAN daha büyük kümeler için daha yüksek esneklik sağlayacak şekilde yapılandırılabilir.
* vCenter, PSC ve NSX Manager VM'ler depolama arızalarına karşı korumak için RAID-10 depolama alanı ile yapılandırılır. VM'ler vSphere HA tarafından düğüm/ağ arızalarına karşı korunur.
* ESXi ev sahiplerinin gereksiz fanları ve NIC'leri vardır.
* TOR ve omurga anahtarları esneklik sağlamak için HA çiftleri yapılandırılır.

CloudSimple, çalışma süresi ve kullanılabilirlik için aşağıdaki VM'leri sürekli olarak izler ve kullanılabilirlik SLA'ları sağlar:

* ESXi ev sahipleri
* vCenter
* Psc
* NSX Yöneticisi

CloudSimple ayrıca hatalar için aşağıdakileri sürekli olarak izler:

* Sabit diskler
* Fiziksel NIC bağlantı noktaları
* Sunucular
* Fanlar
* Üs
* Anahtarlar
* Bağlantı noktalarını değiştirin

Bir disk veya düğüm başarısız olursa, etkilenen VMware kümesine otomatik olarak yeni bir düğüm eklenir ve hemen sağlığına geri döner.

CloudSimple, Özel Bulutlar'daki bu VMware öğelerini yedekler, bakımını ve güncelleştirmesini sağlar:

* Esxi
* vCenter Platform Hizmetleri
* Denetleyicisi
* vSAN
* Nsx

## <a name="back-up-and-restore"></a>Yedekleme ve geri yükleme

CloudSimple yedekleme içerir:

* vCenter, PSC ve DVS kurallarının gece artan yedeklemeleri.
* vCenter yerel API'leri uygulama katmanında bileşenleri yedeklemek için.
* VMware yönetim yazılımının güncellenmeden veya yükseltilmeden önce otomatik yedekleme.
* veriler TLS1.2 şifreli bir kanal üzerinden Azure'a aktarılmadan önce kaynaktaki vCenter veri şifrelemesi. Veriler, bölgeler arasında çoğaltıldığı bir Azure blob'unda depolanır.

[Destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)açarak geri yükleme isteyebilirsiniz.

## <a name="maintenance"></a>Bakım

CloudSimple çeşitli planlı bakım türleri yapar.

### <a name="backendinternal-maintenance"></a>Arka uç/dahili bakım

Bu bakım genellikle fiziksel varlıkları yeniden yapılandırmayı veya yazılım düzeltme eki yüklemeyi içerir. Hizmet edilen varlıkların normal tüketimini etkilemez. Gereksiz NIC'ler her fiziksel rafa giderken, normal ağ trafiği ve Özel Bulut işlemleri etkilenmez. Bir performans etkisi ancak kuruluşunuz bakım aralığı sırasında tam yedek bant genişliğini kullanmayı bekliyorsa fark edebilirsiniz.

### <a name="cloudsimple-portal-maintenance"></a>CloudBasit portal bakımı

CloudSimple denetim düzlemi veya altyapısı güncelleştirildiğinde bazı sınırlı hizmet kapalı kalma süresi gerekir. Şu anda, bakım aralıkları ayda bir kez kadar sık olabilir. Frekansın zamanla azalması bekleniyor. CloudSimple portal bakımı için bildirim sağlar ve aralığı mümkün olduğunca kısa tutar. Portal bakım aralığında, aşağıdaki hizmetler herhangi bir etki yaratmadan çalışmaya devam eder:

* VMware yönetim düzlemi ve uygulamaları
* vCenter erişimi
* Tüm ağ ve depolama
* Tüm Azure trafiği

### <a name="vmware-infrastructure-maintenance"></a>VMware altyapı bakımı

Bazen VMware altyapısının yapılandırmasında değişiklik yapmak gerekir.  Şu anda, bu aralıklar her 1-2 ayda bir oluşabilir, ancak sıklığı zaman içinde düşmesi bekleniyor. Bu tür bakımlar genellikle CloudSimple hizmetlerinin normal tüketimini kesintiye uğratmadan yapılabilir. VMware bakım aralığı sırasında, aşağıdaki hizmetler herhangi bir etki yaratmadan çalışmaya devam eder:

* VMware yönetim düzlemi ve uygulamaları
* vCenter erişimi
* Tüm ağ ve depolama
* Tüm Azure trafiği

## <a name="updates-and-upgrades"></a>Güncellemeler ve Yükseltmeler

CloudSimple, Özel Bulut'taki VMware yazılımının (ESXi, vCenter, PSC ve NSX) yaşam döngüsü yönetiminden sorumludur.

Yazılım güncellemeleri şunlardır:

* **Yamalar.** VMware tarafından yayımlanan güvenlik düzeltmeleri veya hata düzeltmeleri.
* **Güncelleştirmeler**. VMware yığın bileşeninin küçük sürüm değişikliği.
* **Yükseltmeleri**. VMware yığın bileşeninin ana sürüm değişikliği.

CloudSimple, VMware'den kullanıma sunulduğu anda kritik bir güvenlik yamanını sınar. CloudSimple, SLA'ya göre güvenlik yamasını bir hafta içinde Özel Bulut ortamlarına sunar.

CloudSimple, VMware yazılım bileşenleriiçin üç ayda bir bakım güncelleştirmesi sağlar. VMware yazılımının yeni bir ana sürümü kullanılabilir olduğunda, CloudSimple yükseltme için uygun bir bakım penceresini koordine etmek için müşterilerle birlikte çalışır.

## <a name="next-steps"></a>Sonraki adımlar

[Veeam kullanarak iş yükü VM'lerini yedekleme](backup-workloads-veeam.md)
