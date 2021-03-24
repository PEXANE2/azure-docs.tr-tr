---
title: Azure Resource Manager şablonları için bıcep dili
description: Azure Resource Manager şablonları aracılığıyla Azure 'a altyapı dağıtmak için Bıcep dilini açıklar.
ms.topic: conceptual
ms.date: 03/23/2021
ms.openlocfilehash: 74028c682b48a492c2e8f13bef538d1694370cbd
ms.sourcegitcommit: ac035293291c3d2962cee270b33fca3628432fac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "104955916"
---
# <a name="what-is-bicep-preview"></a>Bicep (Önizleme) nedir?

Bıcep, Azure kaynaklarını bildirimli olarak dağıtmaya yönelik bir dildir. Azure Resource Manager şablonlarınızı geliştirmek için JSON yerine Bıcep kullanabilirsiniz (ARM şablonları). Bıcep, daha kısa bir sözdizimi sağlayarak kod yeniden kullanımı için daha iyi destek ve geliştirilmiş tür güvenliği sunarak yazma deneyimini basitleştirir. Bıcep, etki alanına özgü bir dildir (DSL), bu, belirli bir senaryo veya etki alanı için tasarlanmasıdır. Uygulama yazmak için genel programlama dili olarak tasarlanmamıştır.

Şablon oluşturmak için JSON sözdizimi ayrıntılı olabilir ve karmaşık ifade gerektirebilir. Bıcep, bir JSON şablonunun özelliklerini kaybetmeden bu deneyimi geliştirir. ARM şablonları için JSON üzerinde saydam bir soyutlamadır. Her bicep dosyası standart bir ARM şablonuna derlenir. Bir ARM şablonunda geçerli olan kaynak türleri, API sürümleri ve özellikler bir Bıcep dosyasında geçerlidir. Geçerli sürümde bazı [bilinen sınırlamalar](#known-limitations) vardır.

Bıcep Şu anda önizleme aşamasındadır. Çalışmanın durumunu izlemek için, [Bıcep proje deposuna](https://github.com/Azure/bicep)bakın.

Bıcep hakkında bilgi edinmek için aşağıdaki videoya bakın.

> [!VIDEO https://www.youtube.com/embed/sc1kJfcRQgY]

## <a name="get-started"></a>başlarken

Bıcep 'yi kullanmaya başlamak için [araçları yüklemelisiniz](bicep-install.md).

Araçları yükledikten sonra [Bıcep öğreticisini](./bicep-tutorial-create-first-bicep.md)deneyin. Öğretici serisi, Bıcep 'nin yapısı ve özellikleri boyunca size yol gösterir. Bıcep dosyalarını dağıtır ve bir ARM şablonunu eşdeğer Bıcep dosyasına dönüştürebilirsiniz.

Eşdeğer JSON ve Bıcep dosyalarını yan yana görüntülemek için, [Bıcep PLAYIN](https://aka.ms/bicepdemo)bölümüne bakın.

Bıcep 'ye dönüştürmek istediğiniz mevcut bir ARM şablonunuz varsa, bkz. [JSON ve bıcep arasında ARM şablonlarını dönüştürme](bicep-decompile.md).

## <a name="bicep-improvements"></a>Bıcep geliştirmeleri

Bıcep, denk JSON ile karşılaştırıldığında daha kolay ve daha kısa bir sözdizimi sunmaktadır. `[...]`İfadeler kullanmayın. Bunun yerine, doğrudan işlevleri çağırır ve parametrelerden ve değişkenlerden değerler alırsınız. Dağıtılan her kaynağa bir sembolik ad verirsiniz ve bu, şablonunuzda bu kaynağa başvurmayı kolaylaştırır.

Örneğin, aşağıdaki JSON bir kaynak özelliğinden çıkış değeri döndürüyor.

```json
"outputs": {
  "hostname": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', variables('publicIPAddressName'))).dnsSettings.fqdn]"
    },
}
```

Bıcep içindeki eşdeğer çıkış ifadesinin yazılması daha kolay. Aşağıdaki örnek, şablon içinde tanımlı bir kaynak için simgesel ad olan **Publicıp** adı kullanılarak aynı özelliği döndürür:

```bicep
output hostname string = publicIP.properties.dnsSettings.fqdn
```

Sözdiziminin tam karşılaştırması için bkz. [Şablonlar IÇIN JSON ve Bıcep karşılaştırması](compare-template-syntax.md).

Bıcep, kaynaklar arasındaki bağımlılıkları otomatik olarak yönetir. Bir `dependsOn` kaynağın sembolik adı başka bir kaynak bildiriminde kullanıldığında ayarından kaçınabilirsiniz.

Bıcep ile projenizi birden çok modüle kesebilirsiniz.

Bicep dosyasının yapısı, JSON şablonundan daha esnektir. Dosya içinde herhangi bir yerde parametreler, değişkenler ve çıktılar bildirebilirsiniz. JSON 'da, şablonun ilgili bölümlerinin içindeki tüm parametreleri, değişkenleri ve çıkışları bildirmeniz gerekir.

Bicep için VS Code uzantısı, zengin doğrulama ve IntelliSense sağlar. Örneğin, bir kaynağın özelliklerini almak için uzantının IntelliSense 'i kullanabilirsiniz.

## <a name="known-limitations"></a>Bilinen sınırlamalar

Şu an şu sınırlar mevcuttur:

* Kopyalama döngüleri üzerinde mod veya toplu iş boyutu ayarlanamaz.
* Döngüleri ve koşulları birleştiremez.
* Gibi tek satırlı nesne ve diziler `['a', 'b', 'c']` desteklenmez.

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

Hazırsanız, [JSON dosyalarını Bıcep 'ye dönüştürebilirsiniz](bicep-decompile.md).

## <a name="next-steps"></a>Sonraki adımlar

[Bıcep öğreticisi](./bicep-tutorial-create-first-bicep.md)ile çalışmaya başlayın.
