---
title: Azure için rezervasyonlar yazılım maliyetleri | Microsoft Docs
description: Azure ayrılmış VM örneği maliyetlerine hangi yazılım ölçümlerine dahil edilmediğini öğrenin.
services: billing
documentationcenter: ''
author: yashar
manager: yashar
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: banders
ms.openlocfilehash: 52e2e2503303c2a7525a3a6c156f648c097b27dd
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68478620"
---
# <a name="software-costs-not-included-with-azure-reserved-vm-instances"></a>Azure ayrılmış VM örneklerine yazılım maliyetleri dahil değildir

Sanal makine ayrılmış örneği ve SQL ayrılmış kapasite indirimleri, yazılım maliyetlerine değil yalnızca altyapı maliyetlerine uygulanır. Windows VM kullanıyorsanız ve ayrılmış sanal makine örneklerinizin Azure Hibrit Avantajı yoksa, aşağıdaki bölümde listelenen yazılım ölçümleri için ücretlendirilirsiniz. SQL PaaS dağıtımları için, Azure Hibrit Avantajı seçilmezse, IP maliyeti ayrı ölçüm kullanılarak ücretlendirilmeye devam edecektir.

## <a name="windows-software-meters-not-included-in-reservation-cost"></a>Windows yazılım ölçümleri ayırma maliyetine dahil değildir

| Ölçüm kimliği | Kullanım dosyasında MeterName | VM tarafından kullanılır |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Ayırma-Windows SVR patlama (1 çekirdek) | B serisi |
| cac255a2-9f0f-4c62-8bd6-f0fa449c5f76 | Ayırma-Windows SVR patlama (2 çekirdek) | B serisi |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Ayırma-Windows SVR patlama (4 çekirdek) | B serisi |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Ayırma-Windows SVR patlama (8 çekirdek) | B serisi |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Ayırma-Windows SVR (1 çekirdek) | B serisi dışında tümü |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Ayırma-Windows SVR (2 çekirdek) | B serisi dışında tümü |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Ayırma-Windows SVR (4 çekirdek) | B serisi dışında tümü |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Ayırma-Windows SVR (6 çekirdek) | B serisi dışında tümü |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Ayırma-Windows SVR (8 çekirdek) | B serisi dışında tümü |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Ayırma-Windows SVR (12 çekirdek) | B serisi dışında tümü |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Ayırma-Windows SVR (16 çekirdek) | B serisi dışında tümü |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Ayırma-Windows SVR (20 çekirdek) | B serisi dışında tümü |
| 77eb6dd0-88f5-4a16-ab39-05d1742efb25 | Ayırma-Windows SVR (24 çekirdek) | B serisi dışında tümü |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Ayırma-Windows SVR (32 çekirdek) | B serisi dışında tümü |
| f1214b5c-cc16-445f-be6c-a3bb75f8395a | Ayırma-Windows SVR (40 çekirdek) | B serisi dışında tümü |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Ayırma-Windows SVR (64 çekirdek) | B serisi dışında tümü |
| da612742-e7cc-4ca3-9334-0fb7234059cd | Ayırma-Windows SVR (72 çekirdek) | B serisi dışında tümü |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Ayırma-Windows SVR (128 çekirdek) | B serisi dışında tümü |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Ayırma-Windows SVR (256 çekirdek) | B serisi dışında tümü |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Ayırma-Windows SVR (96 çekirdek) | B serisi dışında tümü |

## <a name="cloud-services-software-meters-not-included-in-reservation-cost"></a>Cloud Services yazılım ölçümleri ayırma maliyetine dahil değildir

| Ölçüm kimliği | Kullanım dosyasında MeterName |
| ------- | ------------------------|
|ac9d47ff-ff68-4afc-a145-0c321cf8d0d5|Cloud Services 1 vCPU lisansı|
|e0434559-19ee-4132-9c46-05ad4044f3f7|Cloud Services 2 sanal CPU lisansı|
|6ecc834e-39b3-48b3-8d10-cc5626bacb66|Cloud Services 4 vCPU lisansı|
|13103090-ca72-4825-AB12-7f16c4931d95|Cloud Services 8 vCPU lisansı|
|ecd2bb6e-45a5-49aa-a58b-3947ba21c364|Cloud Services 16 vCPU lisansı|
|de2c7f1d-06dc-4b16-bc8b-c2ec5f4c8aee|Cloud Services 20 vCPU lisansı|
|ca1af837-4b35-47f5-8d14-b1988149c4ca|Cloud Services 32 vCPU lisansı|
|dc72ee45-2ab7-4698-b435-e2cf10d1f9f6|Cloud Services 64 vCPU lisansı|
|7a803026-244c-4659-834c-11e6b2d6b76f|Cloud Services 80 vCPU lisansı|

## <a name="rates-for-azure-meters"></a>Azure ölçümleri için ücretler

Bu ölçümlerden her birinin maliyetini Azure RateCard API 'SI aracılığıyla edinebilirsiniz. Azure ölçüsüne ilişkin ücretleri alma hakkında daha fazla bilgi için bkz. [Azure aboneliğinde kullanılan kaynaklar için fiyat ve meta veri bilgilerini alma](/previous-versions/azure/reference/mt219004(v=azure.100)).

## <a name="next-steps"></a>Sonraki adımlar
Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure için ayırmaları yönetme](billing-manage-reserved-vm-instance.md)
- [Rezervasyon iskontosunun nasıl uygulanacağını anlayın](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).
