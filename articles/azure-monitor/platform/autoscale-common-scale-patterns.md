---
title: Ortak otomatik ölçeklendirme desenlerine genel bakış
description: Azure'da kaynağınızı otomatik ölçeklendirmek için ortak desenlerden bazılarını öğrenin.
ms.topic: conceptual
ms.date: 05/07/2017
ms.subservice: autoscale
ms.openlocfilehash: a77cf1704c20abb77d432eab16569071208f6da8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75396102"
---
# <a name="overview-of-common-autoscale-patterns"></a>Ortak otomatik ölçeklendirme desenlerine genel bakış
Bu makalede, Azure'da kaynağınızı ölçeklendirmek için ortak desenlerden bazıları açıklanmaktadır.

Azure Monitör otomatik ölçeklendirmesi yalnızca [Sanal Makine Ölçeği Kümeleri,](https://azure.microsoft.com/services/virtual-machine-scale-sets/) [Bulut Hizmetleri,](https://azure.microsoft.com/services/cloud-services/) [Uygulama Hizmeti - Web Uygulamaları](https://azure.microsoft.com/services/app-service/web/)ve [API Yönetimi hizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts)için geçerlidir.

## <a name="lets-get-started"></a>Başlayabilesin

Bu makalede, otomatik ölçek aşina olduğunu varsayar. [Kaynağınızı ölçeklendirmek için buradan][1]başlayabilirsiniz. Aşağıdaki ortak ölçek desenleri bazılarıdır.

## <a name="scale-based-on-cpu"></a>CPU'ya dayalı ölçek

Bir web uygulamanız (/VMSS/bulut hizmeti rolü) ve

- CPU'ya göre ölçeklendirmek/ölçeklendirmek istiyorsunuz.
- Ayrıca, en az sayıda örnek olduğundan emin olmak istersiniz.
- Ayrıca, ölçeklendirebileceğiniz örnek sayısı için maksimum bir sınır belirlediğinizi de sağlamak istersiniz.

![CPU'ya dayalı ölçek][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a>Hafta içi vs hafta sonları farklı ölçeklendirin

Bir web uygulamanız (/VMSS/bulut hizmeti rolü) ve

- Varsayılan olarak 3 örnek istiyorsunuz (hafta içi)
- Hafta sonları trafik beklemiyorsunuz ve bu nedenle hafta sonları 1 örneğine küçültmek istiyorsunuz.

![Hafta içi vs hafta sonları farklı ölçeklendirin][3]

## <a name="scale-differently-during-holidays"></a>Tatillerde farklı ölçeklendirin

Bir web uygulamanız (/VMSS/bulut hizmeti rolü) ve

- Varsayılan olarak CPU kullanımına göre ölçeklendirmek/küçültmek istiyorsunuz
- Ancak, tatil sezonunda (veya işletmeniz için önemli olan belirli günlerde) varsayılanları geçersiz kılmak ve emrinizde daha fazla kapasiteye sahip olmak istersiniz.

![Tatillerde farklı ölçeklendirin][4]

## <a name="scale-based-on-custom-metric"></a>Özel metrik e göre ölçeklendirme

Bir web ön ucunuz ve arka uçla iletişim kuran bir API katmanınız vardır.

- API katmanını ön uçtaki özel olaylara göre ölçeklendirmek istiyorsunuz (örnek: Ödeme işleminizi alışveriş sepetindeki öğe sayısına göre ölçeklendirmek istiyorsunuz)

![Özel metrik e göre ölçeklendirme][5]

<!--Reference-->
[1]: ./autoscale-get-started.md
[2]: ./media/autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/autoscale-common-scale-patterns/custom-metric-scale.png

