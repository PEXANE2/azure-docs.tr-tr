---
title: Azure Resource Manager şablonları için bıcep dili
description: Azure Resource Manager şablonları aracılığıyla Azure 'a altyapı dağıtmak için Bıcep dilini açıklar.
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 6a2750dc99e82c9cf8c9b8b97d156d3a9fe30f31
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101747193"
---
# <a name="what-is-bicep-preview"></a>Bicep (Önizleme) nedir?

Bıcep, Azure kaynaklarını bildirimli olarak dağıtmaya yönelik bir dildir. Kısa sözdizimi sağlayarak yazma deneyimini basitleştirir ve modülerlik ve kod yeniden kullanımı için daha iyi destek sunar. Bıcep, etki alanına özgü bir dildir (DSL), bu, belirli bir senaryo veya etki alanı için tasarlanmasıdır. Bıcep, uygulama yazmak için genel programlama dili olarak tasarlanmamıştır.

Bıcep, Azure Resource Manager şablonları (ARM şablonları) üzerinde şeffaf bir soyutlamadır. Her bicep dosyası standart bir ARM şablonuna derlenir. Bir ARM şablonunda geçerli olan kaynak türleri, API sürümleri ve özellikler bir Bıcep dosyasında geçerlidir.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="get-started"></a>başlarken

Bıcep 'yi kullanmaya başlamak için [araçları yüklemelisiniz](https://github.com/Azure/bicep/blob/main/docs/installing.md).

Araçları yükledikten sonra [Bıcep öğreticisini](./bicep-tutorial-create-first-bicep.md)deneyin. Öğretici serisi, Bıcep 'nin yapısı ve özellikleri boyunca size yol gösterir. Bıcep dosyalarını dağıtır ve bir ARM şablonunu eşdeğer Bıcep dosyasına dönüştürebilirsiniz.

Eşdeğer JSON ve Bıcep dosyalarını yan yana görüntülemek için, [Bıcep PLAYIN](https://aka.ms/bicepdemo)bölümüne bakın.

Bıcep 'ye dönüştürmek istediğiniz var olan bir ARM şablonunuz varsa bkz. [JSON 'U Bıcep 'ye](compare-template-syntax.md#decompile-json-to-bicep)dönüştürme.

## <a name="bicep-improvements"></a>Bıcep geliştirmeleri

Bıcep, denk JSON ile karşılaştırıldığında daha kolay ve daha kısa bir sözdizimi sunmaktadır. `[...]`İfadeler kullanmayın. Bunun yerine, doğrudan işlevleri çağırır, parametrelerden ve değişkenlerden değerler alır ve başvuru kaynaklarıdır. Sözdiziminin tam karşılaştırması için bkz. [Şablonlar IÇIN JSON ve Bıcep karşılaştırması](compare-template-syntax.md).

Bıcep, kaynaklar arasındaki bağımlılıkları otomatik olarak yönetir. Bir `dependsOn` kaynağın sembolik adı başka bir kaynak bildiriminde kullanıldığında ayarından kaçınabilirsiniz.

Bıcep ile projenizi birden çok modüle kesebilirsiniz.

Bicep dosyasının yapısı, JSON şablonundan daha esnektir. Dosya içinde herhangi bir yerde parametreler, değişkenler ve çıktılar bildirebilirsiniz. JSON 'da, şablonun ilgili bölümlerinin içindeki tüm parametreleri, değişkenleri ve çıkışları bildirmeniz gerekir.

Bicep için VS Code uzantısı, daha zengin doğrulama ve IntelliSense sağlar. Örneğin, uzantısı bir kaynağın özelliklerini almak için IntelliSense 'e sahiptir.

## <a name="faq"></a>SSS

**Var olan bir dili kullanmak yerine neden yeni bir dil oluşturulsun?**

Bıcep 'yi yeni bir dil yerine var olan ARM şablon diline bir düzeltme olarak düşünebilirsiniz. Sözdizimi değişti, ancak çekirdek işlev ve çalışma zamanı aynı kalır.

Bıcep 'yi geliştirmeye başlamadan önce, var olan bir programlama dilini kullanmayı kabul ediyoruz. Hedef izleyicilerimize, başka bir dille çalışmaya başlamak yerine Bıcep 'nin öğrenmekte olduğunu karartık.

**Önerinizi Terkform veya diğer üçüncü taraf altyapısına kod teklifleri olarak odaklanmadık?**

Farklı kullanıcılar farklı yapılandırma dilleri ve araçları tercih eder. Bu araçların tümünün Azure 'da harika bir deneyim sunduğunuzdan emin olmak istiyoruz. Bıcep, bu çabanın bir parçasıdır.

Terrayform kullanıyorsanız, geçiş yapmak için bir neden yoktur. Microsoft, Azure 'da Tersform 'un en iyi şekilde suntığlarından emin olmanızı taahhüt etmektedir.

ARM şablonlarını seçen müşteriler için Bıcep 'nin yazma deneyimini geliştirdik. Bıcep, kod olarak altyapı benimseyen müşterilerin geçişine de yardımcı olur.

**Bıcep yalnızca Azure için mi?**

Bıcep, tam çözümlerin Azure 'a dağıtılmasıyla ilgili bir DSL. Hedef toplantı, Azure dışındaki bazı API 'lerle çalışmayı gerektirir. Bu senaryolar için genişletilebilirlik noktaları sağlamamız bekleniyor.

**Var olan ARM Şablonlarıma ne olur?**

Her zaman sahip oldukları için tamamen çalışmaya devam ederler. Herhangi bir değişiklik yapmanız gerekmez. Temel alınan ARM şablonu JSON dilini desteklemeye devam edeceğiz. Bıcep dosyaları JSON 'a derlenir ve bu JSON dağıtım için Azure 'a gönderilir.

Hazırsanız, [JSON dosyalarını Bıcep 'ye dönüştürebilirsiniz](compare-template-syntax.md#decompile-json-to-bicep).

## <a name="next-steps"></a>Sonraki adımlar

[Bıcep öğreticisi](./bicep-tutorial-create-first-bicep.md)ile çalışmaya başlayın.
