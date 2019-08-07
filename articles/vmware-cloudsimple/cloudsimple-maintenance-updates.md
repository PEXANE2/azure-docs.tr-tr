---
title: CloudSimple-CloudSimple bakımı ve güncelleştirmeleri tarafından Azure VMware çözümü
description: Zamanlanmış bakım ve güncelleştirmeler için CloudSimple hizmeti işlemini açıklar
author: sharaths-cs
ms.author: dikamath
ms.date: 04/30/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5d6eeecbecc89995c25e687cc6808ed3b0c5dc5c
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816210"
---
# <a name="cloudsimple-maintenance-and-updates"></a>CloudSimple bakım ve güncelleştirmeleri

Özel bulut ortamı, tek başarısızlık noktası olmayacak şekilde tasarlanmıştır:

* ESXi kümeleri, vSphere yüksek kullanılabilirliği ile yapılandırılır. Kümeler dayanıklılık için en az bir yedek düğüme sahip olacak şekilde boyutlandırılır.
* Gereksiz birincil depolama, tek bir hataya karşı koruma sağlamak için en az üç düğüm gerektiren vSAN tarafından sağlanır. vSAN, daha büyük kümeler için daha yüksek dayanıklılık sağlamak üzere yapılandırılabilir.
* vCenter, PSC ve NSX Yöneticisi VM 'Leri, depolama hatasına karşı korunmak için RAID-10 depolama ilkesiyle yapılandırılır. Sanal makineler vSphere HA tarafından düğüm/ağ hatalarıyla korunmaktadır.
* ESXi konaklarının gereksiz fanları ve NIC 'Leri vardır.
* TOR ve sırt geçişleri, dayanıklılık sağlamak için HA çiftlerine göre yapılandırılmıştır.

CloudSimple, çalışma süresi ve kullanılabilirlik için aşağıdaki VM 'Leri sürekli izler ve kullanılabilirlik SLA 'Ları sağlar:

* ESXi Konakları
* vCenter
* PSC
* NSX Yöneticisi

CloudSimple Ayrıca hatalara karşı sürekli olarak aşağıdaki işlemleri izler:

* Sabit diskler
* Fiziksel NIC bağlantı noktaları
* Sunucular
* Larına
* Açılma
* Anahtarlar
* Bağlantı noktalarını Değiştir

Bir disk veya düğüm başarısız olursa, etkilenen VMware kümesine hemen sistem durumuna geri getirmek için yeni bir düğüm otomatik olarak eklenir.

CloudSimple yedekler, bu VMware öğelerini özel bulutlarda tutar ve güncelleştirir:

* ESXi
* vCenter platform hizmetleri
* Denetleyici
* vSAN
* NSX

## <a name="back-up-and-restore"></a>Yedekleme ve geri yükleme

CloudSimple yedeklemesi şunları içerir:

* VCenter, PSC ve DVS kurallarının gece artımlı yedeklemeleri.
* Uygulama katmanında bileşenleri yedeklemek için vCenter Native API 'lerinin kullanımı.
* VMware yönetim yazılımının herhangi bir güncelleştirmesinden veya yükseltilmesiyle önce otomatik yedekleme.
* Bir TLS 1.2 tarafından Azure 'a şifreli bir kanal üzerinden veri aktarımından önce, vCenter tarafından kaynak üzerinde veri şifreleme. Veriler, bölgeler arasında çoğaltılan bir Azure Blob 'unda depolanır.

Bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)açarak geri yükleme isteğinde bulabilirsiniz.

## <a name="maintenance"></a>Bakım

CloudSimple, çeşitli planlı bakım türlerini yapar.

### <a name="backendinternal-maintenance"></a>Arka uç/Iç bakım

Bu bakım genellikle fiziksel varlıkları yeniden yapılandırma veya yazılım düzeltme eklerini yükleme içerir. Hizmet verilen varlıkların normal tüketimini etkilemez. Her fiziksel rafa giden yedekli NIC 'lerle, normal ağ trafiği ve özel bulut işlemleri etkilenmez. Yalnızca kuruluşunuz, bakım aralığı sırasında tam yedekli bant genişliğini kullanmayı bekliyorsa, bir performans etkisi fark edebilirsiniz.

### <a name="cloudsimple-portal-maintenance"></a>CloudSimple Portal Bakımı

CloudSimple denetim düzlemi veya altyapısı güncelleniyorsa, bazı sınırlı hizmet kapalı kalma süresi gereklidir. Şu anda, bakım aralıkları ayda bir kez daha sık olabilir. Zamanın zaman içinde reddedilecek sıklık beklenmektedir. CloudSimple, Portal bakımı için bildirim sağlar ve aralığı mümkün olduğunca kısa tutar. Bir portal bakım aralığı sırasında, aşağıdaki hizmetler herhangi bir etki olmadan çalışmaya devam eder:

* VMware yönetim düzlemi ve uygulamaları
* vCenter erişimi
* Tüm ağ ve depolama
* Tüm Azure trafiği

### <a name="vmware-infrastructure-maintenance"></a>VMware altyapı Bakımı

Bazen VMware altyapısının yapılandırmasında değişiklik yapmak gerekir.  Şu anda bu aralıklar her 1-2 ayda bir gerçekleşebilir, ancak zamanın zaman içinde reddetmesi beklenir. Bu tür bir bakım genellikle CloudSimple hizmetlerinin normal tüketimi kesintiye uğramadan yapılabilir. Bir VMware bakım aralığı sırasında, aşağıdaki hizmetler herhangi bir etki olmadan çalışmaya devam eder:

* VMware yönetim düzlemi ve uygulamaları
* vCenter erişimi
* Tüm ağ ve depolama
* Tüm Azure trafiği

## <a name="updates-and-upgrades"></a>Güncelleştirmeler ve yükseltmeler

CloudSimple, özel bulutta VMware yazılımının (ESXi, vCenter, PSC ve NSX) yaşam döngüsü yönetiminden sorumludur.

Yazılım güncelleştirmeleri şunları içerir:

* **Düzeltme ekleri**. VMware tarafından yayınlanan güvenlik düzeltme ekleri veya hata düzeltmeleri.
* **Güncelleştirmeler**. VMware Stack bileşeninin küçük sürüm değişikliği.
* **Yükseltmeleri**. VMware Stack bileşeninin ana sürüm değişikliği.

CloudSimple, VMware 'den kullanılabilir hale geldiğinde kritik bir güvenlik düzeltme ekini test eder. SLA 'Sı başına, CloudSimple, güvenlik düzeltme ekini bir hafta içinde özel bulut ortamlarına kaydeder.

CloudSimple, VMware yazılım bileşenlerine üç aylık bakım güncelleştirmeleri sağlar. VMware yazılımının yeni bir ana sürümü kullanılabilir olduğunda, CloudSimple, yükseltme için uygun bir bakım penceresini koordine etmek üzere müşterilerle birlikte çalışmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

[Veead kullanarak iş yükü VM 'Lerini yedekleyin](https://docs.azure.cloudsimple.com/backup-workloads-veeam/).