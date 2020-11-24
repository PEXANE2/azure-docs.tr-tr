---
title: VMware HCX ağ kesimleri
description: VMware HCX için dört ağ gerekir.
ms.topic: include
ms.date: 11/23/2020
ms.openlocfilehash: 48894c532c97b70cde1473fb8b81f406ded70343
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95529743"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

VMware HCX için dört ağ gerekir:

- **Yönetim ağı:** Genellikle, bu, vSphere kümesinde kullanılan yönetim ağsıdır. En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla. Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyacınız bulunabilir.

   > [!NOTE]
   > Önerdiğimiz Yöntem bir/26 ağı oluşturuyor. Bir/26 ağda, en fazla 10 hizmet kafesi ve 60 ağ Extender (hizmet ağı başına-1) kullanabilirsiniz. Azure VMware Çözüm özel bulutlarını kullanarak her ağ genişletici için sekiz ağ uzatabilirsiniz.
   >
   
- **vMotion ağı:** Genellikle, vSphere kümesi üzerinde vMotion için kullanılan ağ aynıdır.  En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla. Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyacınız bulunabilir.  

   VMotion ağı dağıtılmış bir sanal anahtarda veya vSwitch0 üzerinde kullanıma sunulmalıdır. Aksi takdirde, ortamı değiştirin.

   > [!NOTE]
   > Bu ağ özel (yönlendirilmeyen) olabilir.

- **Yukarı bağlantı ağı:** VMware HCX yukarı ağı için yeni bir ağ oluşturmak ve bağlantı noktası grubu aracılığıyla vSphere kümenize genişletmek istiyorsunuz. En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla. Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyacınız bulunabilir.  

   > [!NOTE]
   > Önerdiğimiz Yöntem bir/26 ağı oluşturuyor. Bir/26 ağda, en fazla 10 hizmet kafesi ve 60 ağ Extender (hizmet ağı başına-1) kullanabilirsiniz. Azure VMware Çözüm özel bulutlarını kullanarak her ağ genişletici için sekiz ağ uzatabilirsiniz.
   >
   
- **Çoğaltma ağı:** Bu isteğe bağlıdır. VMware HCX çoğaltması için yeni bir ağ oluşturmak ve bu ağı bir bağlantı noktası grubu aracılığıyla vSphere kümenize genişletmek istiyorsunuz. En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla. Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyacınız bulunabilir.

   > [!NOTE]
   > Bu yapılandırma yalnızca şirket içi küme ana bilgisayarları adanmış bir çoğaltma VMkernel ağı kullanması durumunda mümkündür.  Şirket içi kümenizin tanımlı bir adanmış çoğaltma VMkernel ağı yoksa, bu ağı oluşturmaya gerek yoktur.
