---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: ac91ef5a56fe234cba47b430b78e74ce81c9d504
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96536965"
---
Azure depolama, Azure Izleyici 'de aşağıdaki işlem ölçümlerini sağlar.

| Metric | Açıklama |
| ------------------- | ----------------- |
| İşlemler | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve hata üreten istekleri içerir. <br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Geçerli Boyutlar: ResponseType, GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions))<br/> Değer örneği: 1024 |
| Giriş | Giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir. <br/><br/> Birim: bayt <br/> Toplama türü: Toplam <br/> Geçerli Boyutlar: GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Çıkış | Çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz. <br/><br/> Birim: bayt <br/> Toplama türü: Toplam <br/> Geçerli Boyutlar: GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Başarılı Sunucugecikmesi | Azure Depolama tarafından gerçekleştirilen başarılı bir isteği işlemek için kullanılan ortalama süre. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez. <br/><br/> Birim: milisaniye <br/> Toplama türü: Ortalama <br/> Geçerli Boyutlar: GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| SuccessE2ELatency | Bir depolama hizmetine yapılan başarılı isteklerin veya belirtilen API işleminin ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. SuccessE2ELatency ve başarılı Serverlatency değerleri arasındaki fark, ağın ve istemcinin neden olduğu gecikme süresi olabilir.<br/><br/> Birim: milisaniye <br/> Toplama türü: Ortalama <br/> Geçerli Boyutlar: GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Kullanılabilirlik | Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, toplam faturalandırılabilir istek değerinin beklenmeyen hata üreten istekler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır. <br/><br/> Birim: yüzde <br/> Toplama türü: Ortalama <br/> Geçerli Boyutlar: GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions)) <br/> Değer örneği: 99,99 |
