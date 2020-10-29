---
title: Yükseltilmiş erişim sırasında yasak eylemler
description: VMware altyapısı, aşağıdaki yasak eylemlerden herhangi birini algıladığında hizmetin kesintisiz kalmasını sağlamak için değişiklikleri geri alır.
titleSuffix: Azure VMware Solution by CloudSimple
ms.date: 10/28/2020
ms.topic: article
ms.service: azure-vmware-cloudsimple
author: divka78
ms.author: dikamath
ms.openlocfilehash: 81a09a9cfe749de56f7306a8d5e1ae5db01295fc
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92915455"
---
# <a name="forbidden-actions-during-elevated-access"></a>Yükseltilmiş erişim sırasında yasak eylemler

Yükseltme zaman aralığı sırasında bazı eylemler yasaktır. VMware altyapısı aşağıdaki yasak eylemlerden herhangi birini algıladığında, VMware altyapısı, hizmetin kesintisiz kalmasını sağlamak için değişiklikleri geri alır.

## <a name="cluster-actions"></a>Küme eylemleri

- Bir kümeyi vCenter 'dan kaldırma.
- Bir kümede vSphere yüksek kullanılabilirliğini (HA) değiştirme.
- VCenter 'dan kümeye ana bilgisayar ekleniyor.
- Kümeden vCenter 'dan bir konak kaldırılıyor.

## <a name="host-actions"></a>Konak eylemleri

- ESXi ana bilgisayarında veri depoları kaldırılıyor.
- VCenter Aracısı konaktan kaldırılıyor.
- Konak yapılandırmasını değiştirme.
- Konak profillerinde herhangi bir değişiklik yapılıyor.
- Bakım moduna bir konak yerleştiriliyor.

## <a name="network-actions"></a>Ağ eylemleri

- Özel bir buluttaki varsayılan dağıtılmış sanal anahtar (DVS) siliniyor.
- Varsayılan DVS 'den bir konak kaldırılıyor.
- Herhangi bir DVS ayarı içeri aktarılıyor.
- Herhangi bir DVS ayarı yeniden yapılandırılıyor.
- DVS 'yi yükseltme.
- Yönetim bağlantı noktası grubu siliniyor.
- Yönetim bağlantı noktası grubu düzenleniyor.

## <a name="roles-and-permissions-actions"></a>Roller ve izinler eylemleri

- Genel bir rol oluşturuluyor.
- Herhangi bir yönetim nesnesi için izinleri değiştirme veya silme.
- Varsayılan rolleri değiştirme veya kaldırma.
- Rolün ayrıcalıklarını bulut sahibi rolünden daha yüksek olacak şekilde artırın.

## <a name="other-actions"></a>Diğer eylemler

- Varsayılan lisanslar kaldırılıyor:
  - vCenter Server
  - ESXi düğümleri
  - NSX-T
  - HCX
- Yönetim kaynak havuzunu değiştirme veya silme.
- Yönetim VM 'Leri kopyalanıyor.


## <a name="next-steps"></a>Sonraki adımlar
[CloudSimple bakım ve güncelleştirmeleri](cloudsimple-maintenance-updates.md) 
