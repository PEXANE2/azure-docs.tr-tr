---
title: VMware HCX ağ kesimleri
description: VMware HCX için dört ağ gerekir.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: f01aba13d688dfed573acfc8c4344511fbfd9fa8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91583398"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

VMware HCX için dört ağ gerekir:

- **Yönetim ağı:** Genellikle, bu, vSphere kümesinde kullanılan yönetim ağsıdır.  En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla (dağıtımınıza bağlı olarak daha büyük numaralar gerekebilir).

- **vMotion ağı:** Genellikle, vSphere kümesi üzerinde vMotion için kullanılan ağ aynıdır.  En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla (dağıtımınıza bağlı olarak daha büyük numaralar gerekebilir).  

   VMotion ağı dağıtılmış bir sanal anahtarda veya vSwitch0 üzerinde kullanıma sunulmalıdır. Aksi takdirde, ortamı değiştirin.

   > [!NOTE]
   > Bu ağ yönlendirilmemişse (özel), bu, tamam olur.

- **Yukarı bağlantı ağı:** VMware HCX yukarı ağı için yeni bir ağ oluşturmak ve bağlantı noktası grubu aracılığıyla vSphere kümenize genişletmek istiyorsunuz.  En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla (dağıtımınıza bağlı olarak daha büyük numaralar gerekebilir).  

   > [!NOTE]
   > Önerilen yöntem bir/29 ağ oluşturmaktır, ancak herhangi bir ağ boyutu olur.

- **Çoğaltma ağı:** VMware HCX çoğaltması için yeni bir ağ oluşturmak ve bu ağı bir bağlantı noktası grubu aracılığıyla vSphere kümenize genişletmek istiyorsunuz.  En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla (dağıtımınıza bağlı olarak daha büyük numaralar gerekebilir).

   > [!NOTE]
   > Önerilen yöntem bir/29 ağ oluşturmaktır, ancak herhangi bir ağ boyutu olur.