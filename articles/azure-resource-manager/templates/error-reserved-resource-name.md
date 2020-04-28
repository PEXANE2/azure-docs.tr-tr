---
title: Ayrılmış kaynak adı hataları
description: Ayrılmış bir sözcük içeren bir kaynak adı sağlarken hataların nasıl çözümleneceğini açıklar.
ms.topic: troubleshooting
ms.date: 11/08/2017
ms.openlocfilehash: e76f4bf9bfee7de6e7523d69acf1388d2dd80e93
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75477635"
---
# <a name="resolve-reserved-resource-name-errors"></a>Ayrılmış kaynak adı hatalarını çözümle

Bu makalede, adında ayrılmış bir sözcük içeren bir kaynağı dağıttığınızda karşılaştığınız hata açıklanır.

## <a name="symptom"></a>Belirti

Ortak bir uç nokta aracılığıyla kullanılabilen bir kaynak dağıtıldığında, şu hatayı alabilirsiniz:

```
Code=ReservedResourceName;
Message=The resource name <resource-name> or a part of the name is a trademarked or reserved word.
```

## <a name="cause"></a>Nedeni

Ortak uç noktası olan kaynaklar, adında ayrılmış kelimeleri veya ticari markaları kullanamaz.

Aşağıdaki sözcükler ayrılmıştır:

* ERİŞİM
* MAVISI
* CıLARı
* BIZSPARK
* BıZ
* CORTANA
* DIRECTX
* OLMALı
* CRM
* EXCEL
* DEĞIŞIMI
* FOREFRONT
* ĞA
* HOLOLENS
* HYPERV
* KINECT
* Kurumsal
* MSDN
* O365
* OFFICE
* OFFICE365
* KLASÖR
* 'TA
* OFFICE
* PROGRAMLARıNDA
* SHAREPOINT
* TOPLANTıSıNA
* \
* VISUALSTUDIO

Aşağıdaki sözcükler, adında tam bir sözcük veya alt dize olarak kullanılamaz:

* LOGıN
* MICROSOFT
* PENCERELERIN
* YENILIKLER

## <a name="solution"></a>Çözüm

Ayrılmış sözcüklerden birini kullanmayan bir ad sağlayın.