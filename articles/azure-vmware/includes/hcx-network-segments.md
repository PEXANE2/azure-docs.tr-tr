---
title: VMware HCX ağ kesimleri
description: VMware HCX için dört ağ gerekir.
ms.topic: include
ms.date: 03/13/2021
ms.openlocfilehash: 1f0fcc1ddeeb26702a297035e6ae2a2f73dd71d6
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462376"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

VMware HCX için dört ağ gerekir:

- **Yönetim ağı:** Genellikle, Azure VMware Çözüm kümesi tarafından kullanılan aynı yönetim ağı vardır. En azından, VMware HCX için bu ağ segmentinde **iki** IP 'yi tanımla. Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyacınız bulunabilir.

   > [!NOTE]
   > En fazla 10 hizmet kafesi ve 60 ağ Genişleticilerini (hizmet ağı başına-1) kullanabilmeniz için önerilen yöntem bir/26 ağı oluşturmaktır. Azure VMware Çözüm özel bulutlarını kullanarak her ağ genişletici için **sekiz** ağ uzatabilirsiniz.
   >
   
- **vMotion ağı:** Genellikle, Azure VMware Çözüm kümesinde vMotion için kullanılan ağ aynıdır.  En azından, VMware HCX için bu ağ segmentinde **iki** IP 'yi tanımla. Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyacınız bulunabilir.  

   VMotion ağı dağıtılmış bir sanal anahtarda veya vSwitch0 üzerinde kullanıma sunulmalıdır. Aksi takdirde, ortamı değiştirin.

   > [!NOTE]
   > Bu ağ özel (yönlendirilmeyen) olabilir.

- **Yukarı bağlantı ağı:** VMware HCX yukarı ağı için yeni bir ağ oluşturmak ve bağlantı noktası grubu aracılığıyla vSphere kümenize genişletmek istiyorsunuz. En azından, VMware HCX için bu ağ segmentinde **iki** IP 'yi tanımla. Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyacınız bulunabilir.  

   > [!NOTE]
   > En fazla 10 hizmet kafesi ve 60 ağ Genişleticilerini (hizmet ağı başına-1) kullanabilmeniz için önerilen yöntem bir/26 ağı oluşturmaktır. Azure VMware Çözüm özel bulutlarını kullanarak her ağ genişletici için **sekiz** ağ uzatabilirsiniz.
   >
   
- **Çoğaltma ağı:** Bu isteğe bağlıdır. VMware HCX çoğaltması için yeni bir ağ oluşturmak ve bu ağı bir bağlantı noktası grubu aracılığıyla vSphere kümenize genişletmek istiyorsunuz. En azından, VMware HCX için bu ağ segmentinde **iki** IP 'yi tanımla. Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyacınız bulunabilir.

   > [!NOTE]
   > Bu yapılandırma yalnızca şirket içi küme ana bilgisayarları adanmış bir çoğaltma VMkernel ağı kullanması durumunda mümkündür.  Şirket içi kümenizin tanımlı bir adanmış çoğaltma VMkernel ağı yoksa, bu ağı oluşturmaya gerek yoktur.
