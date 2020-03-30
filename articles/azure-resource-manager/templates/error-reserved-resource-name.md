---
title: Ayrılmış kaynak adı hataları
description: Ayrılmış bir sözcük içeren bir kaynak adı sağlarken hataların nasıl çözüleceğini açıklar.
ms.topic: troubleshooting
ms.date: 11/08/2017
ms.openlocfilehash: e76f4bf9bfee7de6e7523d69acf1388d2dd80e93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75477635"
---
# <a name="resolve-reserved-resource-name-errors"></a>Ayrılmış kaynak adı hatalarını çözümle

Bu makalede, adına ayrılmış bir sözcük içeren bir kaynak dağıtılırken karşılaştığınız hata açıklanmaktadır.

## <a name="symptom"></a>Belirti

Ortak bir bitiş noktası üzerinden kullanılabilen bir kaynak dağıtırken, aşağıdaki hatayı alabilirsiniz:

```
Code=ReservedResourceName;
Message=The resource name <resource-name> or a part of the name is a trademarked or reserved word.
```

## <a name="cause"></a>Nedeni

Genel bitiş noktası olan kaynaklar, adiçinde ayrılmış sözcükleri veya ticari markaları kullanamaz.

Aşağıdaki sözcükler ayrılmıştır:

* ERİŞİM
* Azure
* Bing
* BIZSPARK
* Biztalk
* CORTANA
* Directx
* Dotnet
* Dynamics
* EXCEL
* Exchange
* Forefront
* Groove
* HOLOLENS
* HYPERV
* Kinect
* Lync
* MSDN
* O365
* Office
* OFİs 365
* ONEDRIVE
* Onenote
* Outlook
* Powerpoint
* Sharepoint
* Skype
* Visio
* VISUALSTUDIO

Aşağıdaki sözcükler, addaki tüm sözcük veya alt dize olarak kullanılamaz:

* Oturum açma
* Microsoft
* Windows
* Xbox

## <a name="solution"></a>Çözüm

Ayrılmış sözcüklerden birini kullanmayan bir ad sağlayın.