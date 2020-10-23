---
title: VMware HCX ağ kesimleri
description: VMware HCX için dört ağ gerekir.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 8137b4383d2a243d53db317db6f5a78b3bc68e67
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92173623"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

VMware HCX için dört ağ gerekir:

- **Yönetim ağı:** Genellikle, bu, vSphere kümesinde kullanılan yönetim ağsıdır. En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla. (Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyaç duyabilirsiniz.)

- **vMotion ağı:** Genellikle, vSphere kümesi üzerinde vMotion için kullanılan ağ aynıdır.  En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla. (Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyaç duyabilirsiniz.)  

   VMotion ağı dağıtılmış bir sanal anahtarda veya vSwitch0 üzerinde kullanıma sunulmalıdır. Aksi takdirde, ortamı değiştirin.

   > [!NOTE]
   > Bu ağ yönlendirilmemişse (özel), bu Tamam ' dır.

- **Yukarı bağlantı ağı:** VMware HCX yukarı ağı için yeni bir ağ oluşturmak ve bağlantı noktası grubu aracılığıyla vSphere kümenize genişletmek istiyorsunuz. En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla. (Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyaç duyabilirsiniz.)  

   > [!NOTE]
   > Önerilen yöntem bir/29 ağ oluşturmaktır, ancak herhangi bir ağ boyutu olur.

- **Çoğaltma ağı:** VMware HCX çoğaltması için yeni bir ağ oluşturmak ve bu ağı bir bağlantı noktası grubu aracılığıyla vSphere kümenize genişletmek istiyorsunuz. En azından, VMware HCX için bu ağ segmentinde iki IP 'yi tanımla. (Dağıtımınıza bağlı olarak daha büyük sayılara ihtiyaç duyabilirsiniz.)

   > [!NOTE]
   > Önerilen yöntem bir/29 ağ oluşturmaktır, ancak herhangi bir ağ boyutu olur.