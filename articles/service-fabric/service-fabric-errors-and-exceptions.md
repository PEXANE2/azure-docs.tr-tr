---
title: Ortak FabricClient özel durumlar atıldı
description: Uygulama ve küme yönetimi işlemleri gerçekleştirirken FabricClient API'leri tarafından atılabilir ortak özel durumlar ve hataları açıklar.
author: oanapl
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: oanapl
ms.openlocfilehash: 9ad3097a490d4728e05ea90652c17c24b79cac2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75457926"
---
# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>FabricClient API'leriyle çalışırken sık karşılaşılan özel durumlar ve hatalar
[FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) API'leri küme ve uygulama yöneticilerinin Bir Hizmet Kumaşı uygulamasında, hizmetinde veya kümede yönetim görevlerini gerçekleştirmesini sağlar. Örneğin, uygulama dağıtımı, yükseltme ve kaldırma, sistem durumu kümesini denetleme veya bir hizmeti sınaması. Uygulama geliştiricileri ve küme yöneticileri, Service Fabric kümesini ve uygulamaları yönetmek için araçlar geliştirmek için FabricClient API'lerini kullanabilir.

FabricClient kullanılarak gerçekleştirilebilecek birçok farklı işlem türü vardır.  Her yöntem, yanlış giriş, çalışma zamanı hataları veya geçici altyapı sorunları nedeniyle hatalar için özel durumlar atabilir.  Belirli bir yöntemle hangi özel durumların atıldığını bulmak için API başvuru belgelerine bakın. Ancak, birçok farklı [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) API'leri tarafından atılabilir bazı istisnalar vardır. Aşağıdaki tabloda FabricClient API'lerinde yaygın olan özel durumlar listelenir.

| Özel durum | Atılan zaman |
| --- |:--- |
| [System.Fabric.FabricObjectKapalı İstisna](https://docs.microsoft.com/dotnet/api/system.fabric.fabricobjectclosedexception) |[FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) nesnesi kapalı durumdadır. Kullandığınız [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) nesnesini atın ve yeni bir [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) nesnesini anında atın. |
| [System.TimeoutException](https://docs.microsoft.com/dotnet/core/api/system.timeoutexception) |Operasyon zaman doldu. [OperationTimedOut,](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) işlemin tamamlanması MaxOperationTimeout'tan daha fazla zaman aldığında döndürülür. |
| [System.Yetkisiz ErişimÖzel Durum](https://docs.microsoft.com/dotnet/core/api/system.unauthorizedaccessexception) |İşlemin erişim denetimi başarısız oldu. E_ACCESSDENIED iade edilir. |
| [System.Fabric.FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) |İşlemicra sırasında çalışma zamanı hatası oluştu. FabricClient yöntemlerinden herhangi biri [FabricException'ı](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception)potansiyel olarak atabilir, [ErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception.ErrorCode) özelliği özel durum nedenini gösterir. [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) numaralandırmasında hata kodları tanımlanır. |
| [System.Fabric.FabricGeçici İstisna](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception) |İşlem, bir tür geçici hata durumu nedeniyle başarısız oldu. Örneğin, çoğaltma sayısı geçici olarak erişilemedığından bir işlem başarısız olabilir. Geçici özel durumlar, yeniden denenebilecek başarısız işlemlere karşılık gelir. |

Bir [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception)döndürülebilir bazı yaygın [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) hataları:

| Hata | Koşul |
| --- |:--- |
| İletişim Hatası |Bir iletişim hatası işlemin başarısız olması, işlemi yeniden denemesi neden oldu. |
| GeçersizCredentialType |Kimlik bilgisi türü geçersizdir. |
| GeçersizX509FindType |X509FindType geçersizdir. |
| GeçersizX509MağazaKonum |X509 mağaza konumu geçersizdir. |
| GeçersizX509StoreName |X509 mağaza adı geçersizdir. |
| GeçersizX509Thumbprint |X509 sertifikası parmak izi dizesi geçersizdir. |
| GeçersizKoruma Düzeyi |Koruma düzeyi geçersizdir. |
| GeçersizX509Mağaza |X509 sertifika deposu açılamaz. |
| GeçersizKonu Adı |Özne adı geçersizdir. |
| GeçersizİzinOrtak NameList |Ortak ad listesi dize biçimi geçersizdir. Virgülden ayrılmış bir liste olmalı. |

