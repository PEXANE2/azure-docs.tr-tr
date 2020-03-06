---
title: Azure Kamu Web, mobil ve API | Microsoft Docs
description: Bu, Azure Kamu için uygulama geliştirmeye yönelik özellikler ve yönergeler karşılaştırması sağlar
services: azure-government
cloud: gov
documentationcenter: ''
author: gsacavdm
manager: pathuff
ms.assetid: a1e173a9-996a-4091-a2e3-6b1e36da9ae1
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 11/22/2019
ms.author: gsacavdm
ms.openlocfilehash: 15c6936b7a8c319c4ddef86eddc47546966782b4
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78396349"
---
# <a name="azure-government-web--mobile"></a>Azure Kamu Web ve Mobil

Bu makalede, Azure Kamu ortamı için Web ve mobil hizmetler 'in yanı sıra, genel sürümden farklı özellik çeşitlemelerini açıklayan ve ortama özgü bazı hususlar özetlenmektedir.

## <a name="azure-cognitive-search"></a>Azure Bilişsel Arama

[Azure bilişsel arama](https://docs.microsoft.com/azure/search/) , Azure Kamu 'da genel kullanıma sunulmuştur. Ortak kamu verileri kullanarak arama işlevselliğinin kendinden yönlendirilme için, [Içerik arama ve zekası](https://documentsearch.azurewebsites.net/home/) Web sitesini ziyaret edin, "ABD Mahkemesi 1 ' i seçin ve tanıtım seçeneklerinden birini belirleyin.

Kamu arama uygulamalarında yaygın olarak benimsenen özellikler, büyük ölçekli metin belgelerinden yapıyı ve bilgileri ayıklamak için yararlı olan bilişsel [yetenekler](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro)içerir.

Basit sorgu söz dizimi, büyük miktarlarda içeriğe göre arama yapmak için sorguları ifade etmek de uygulama geliştiricileri ile ilgilidir. Azure Bilişsel Arama iki sözdizimleri destekler: [basit](https://docs.microsoft.com/azure/search/query-simple-syntax) ve [tam](https://docs.microsoft.com/azure/search/query-lucene-syntax). [Sorgu ifadesi örneklerini](https://docs.microsoft.com/azure/search/search-query-simple-examples) bir yön için gözden geçirebilirsiniz.

### <a name="variations"></a>Farklılıkları
Azure Kamu 'da oluşturulan arama hizmetleri için uç nokta aşağıdaki gibidir:

| Hizmet türü | Azure genel | Azure Kamu |
| ------------ | ------------ | ---------------- |
| Azure Bilişsel Arama hizmeti |\*. search.windows.net |\*. search.windows.us|

Genel buluttaki tüm genel kullanıma açık ve Önizleme özellikleri Azure Kamu 'da da kullanılabilir.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıdaki bilgiler Azure Bilişsel Arama Azure Kamu sınırını tanımlar:

| Düzenlenen/denetlenen veriler izin verildi | Düzenlenen/denetlenen verilere izin verilmiyor |
| ----------------------------------- | --------------------------------------- |
| Azure Bilişsel Arama 'de depolanan ve işlenen tüm veriler, Azure Kamu tarafından düzenlenen verileri içerebilir. | Azure Bilişsel Arama meta verilerinin dışarı aktarma denetimli verileri içermesine izin verilmez. Bu meta veriler, hizmetinizi oluştururken ve sürdürsırada girilen tüm yapılandırma verilerini içerir. Şu alanlara düzenlenmiş/denetlenen verileri girmeyin: abonelik adı, kaynak grupları, hizmet adı, kaynak adları, kaynak etiketleri veya Bilişsel Arama (dizinler, Dizin oluşturucular, veri kaynakları, eş anlamlı haritalar ve becerileri). API 'nin bir parçası olarak gönderilen arama/sorgu dizelerinde REST API PR 'ye gönderilen HTTP üst bilgilerine gizli verileri eklemeyin.|

## <a name="app-services"></a>Uygulama Hizmetleri
### <a name="variations"></a>Farklılıkları
Azure Uygulama Hizmetleri, Azure Kamu 'da genel kullanıma sunulmuştur.

Azure Kamu 'da oluşturulan Azure App Service uygulamaların adresi, genel bulutta oluşturulan uygulamalardan farklıdır:

| Hizmet türü | Azure genel | Azure Kamu |
| --- | --- | --- |
| App Service |\*. azurewebsites.net |\*. azurewebsites.us|
| Hizmet sorumlusu KIMLIĞI| abfa0a7c-a6b6-4736-8310-5855508787cd | 6a02c803-dadfd-4136-b4c3-5a6f318b4714 |

Genel bulutta bulunan bazı App Service özellikleri Azure Kamu 'da henüz kullanılamaz:

- Sorunları tanılama ve çözme
- Dağıtım
    - Dağıtım seçenekleri: yalnızca yerel Git deposu ve dış depo kullanılabilir.
    - Dağıtım Merkezi
- Ayarlar
    - Application Insights
- Geliştirme araçları
    - Performans testi
    - Kaynak Gezgini
    - PHP hata ayıklaması
- & Sorun giderme desteği
    - App Service Danışmanı
- App Service Sertifikaları


### <a name="considerations"></a>Dikkat edilmesi gerekenler
Aşağıdaki bilgiler App Service için Azure Kamu sınırını tanımlar:

| Düzenlenen/denetlenen veriler izin verildi | Düzenlenen/denetlenen verilere izin verilmiyor |
| --- | --- |
| Azure App Service içinde girilen, depolanan ve işlenen veriler, dışarı aktarma denetimli verileri içerebilir. Azure App Service içinde çalışan ikililer. Azure platform bileşenlerine erişim için parolalar ve akıllı kart PIN 'leri gibi statik kimlik doğrulayıcılar. Azure Platformu bileşenlerini yönetmek için kullanılan sertifikaların özel anahtarları. SQL bağlantı dizeleri. Azure hizmetlerinde depolanan sertifikalar, şifreleme anahtarları, ana anahtarlar ve depolama anahtarları gibi diğer güvenlik bilgileri/gizli dizileri. |Meta verilerin dışarı aktarma denetimli veri içermesine izin verilmez. Bu meta veriler, Azure App Service oluştururken ve bakımında girilen tüm yapılandırma verilerini içerir. Şu alanlara düzenlenmiş/denetlenen verileri girmeyin: kaynak grupları, kaynak adları, kaynak etiketleri|

## <a name="api-management"></a>API Management
Bu hizmetle ilgili ayrıntılar ve nasıl kullanılacağı hakkında bilgi edinmek için bkz. [Azure API Management belgeleri](../api-management/index.yml).

### <a name="variations"></a>Farklılıkları

Azure API Management hizmeti, Azure Kamu 'da genel kullanıma sunulmuştur. Azure Kamu için API Management hizmetinde şu anda kullanılamayan özellikler şunlardır:

- Azure AD B2C tümleştirme 
    - Azure AD B2C tümleştirme Azure Kamu 'da kullanılamaz 

Azure Kamu 'da Azure API Management erişmek için URL 'Ler farklıdır:

| Hizmet türü | Azure genel | Azure Kamu |
| --- | --- | --- |
|API Management ağ geçidi| \*. azure-api.net| \*. azure-api.us|
|API Management portalı | \*. portal.azure-api.net |\*. portal.azure-api.us| 
|API Management yönetimi| \*. management.azure-api.net |\*. management.azure-api.us|

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Aşağıdaki bilgiler Azure API Management hizmeti için Azure Kamu sınırını tanımlar:

| Düzenlenen/denetlenen veriler izin verildi | Düzenlenen/denetlenen verilere izin verilmiyor |
| --- | --- |
|Azure API Management hizmetinde depolanan ve işlenen tüm veriler, Azure Kamu tarafından düzenlenen verileri içerebilir.|Azure API Management hizmet meta verilerinin dışarı aktarma denetimli verileri içermesine izin verilmez. Şu alanlara düzenlenmiş/denetlenen verileri girmeyin: API Management hizmet adı, abonelik adı, kaynak grupları, kaynak etiketleri.|

## <a name="next-steps"></a>Sonraki Adımlar
Ek bilgi ve güncelleştirmeler için [Microsoft Azure Kamu bloguna](https://blogs.msdn.microsoft.com/azuregov/) abone olun.

