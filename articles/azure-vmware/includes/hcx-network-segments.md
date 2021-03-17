---
title: VMware HCX ağ kesimleri
description: VMware HCX için dört ağ gerekir.
ms.topic: include
ms.date: 03/13/2021
ms.openlocfilehash: e9b37c125db82a95c137ede8d642888fba8b6c80
ms.sourcegitcommit: 87a6587e1a0e242c2cfbbc51103e19ec47b49910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103622023"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

Şirket içinde VMware HCX ağ kesimlerini yapılandırmanın çeşitli yolları vardır. Aşağıda, pilot veya küçük bir üretim kullanım durumunu destekleyen basit bir yapılandırma özetlenmektedir.  Yüzlerce veya binlerce iş yükü için tasarlarken, geçişin ihtiyaçlarına bağlı olarak bu yapılandırmanın değiştirilmesi gerekebilir.  

VMware HCX dağıtımı için pilot veya küçük üretim kullanım durumunu desteklemeye hazırlanın, şunları yapın:

- **Yönetim ağı:** VMware HCX 'i şirket içinde dağıttığınızda bir yönetim ağı tanımlamanız gerekir.  Genellikle, şirket içi VMware kümeniz tarafından kullanılan aynı yönetim ağı vardır.  En azından, VMware HCX için bu ağ segmentinde **iki** IP 'yi tanımla. Pilot veya küçük kullanım durumunun ötesinde dağıtımınızın ölçeğine bağlı olarak daha büyük sayılara ihtiyacınız bulunabilir.

> [!NOTE]
   > En fazla 10 hizmet kafesi ve 60 ağ Genişleticilerini (hizmet ağı başına-1) kullanabilmeniz için önerilen yöntem bir/26 ağı oluşturmaktır. Azure VMware Çözüm özel bulutlarını kullanarak her ağ genişletici için **sekiz** ağ uzatabilirsiniz.
   >
   
- **vMotion ağı:** VMware HCX 'i şirket içinde dağıttığınızda bir vMotion ağı tanımlamanız gerekir.  Genellikle, şirket içi VMware kümeniz tarafından vMotion için kullanılan ağ aynıdır.  En azından, VMware HCX için bu ağ segmentinde **iki** IP 'yi tanımla. Pilot veya küçük kullanım durumunun ötesinde dağıtımınızın ölçeğine bağlı olarak daha büyük sayılara ihtiyacınız bulunabilir.

   VMotion ağı dağıtılmış bir sanal anahtarda veya vSwitch0 üzerinde kullanıma sunulmalıdır. Aksi takdirde, ortamı sığacak şekilde değiştirin.

   > [!NOTE]
   > Birçok VMware ortamı, hiçbir sorun ortaya çıkan vMotion için yönlendirilmeyen ağ kesimleri kullanır.

- **Yukarı bağlantı ağı:** VMware HCX 'i şirket içinde dağıttığınızda bir yukarı bağlantı ağı tanımlamanız gerekir. Yukarı bağlantı ağınız olarak tanımlanan yönetim ağını kullanın.
   
- **Çoğaltma ağı:** VMware HCX 'i şirket içinde dağıttığınızda, bir çoğaltma ağı tanımlamanız gerekir. Çoğaltma ağınız olarak tanımlanan yönetim ağını kullanın.  Şirket içi küme Konakları ayrılmış bir VMkernel ağı kullanıyorsa, bu ağ kesiminde **iki** IP adresi ayırın ve çoğaltma ağı Için çoğaltma vmkernel ağını kullanın.
