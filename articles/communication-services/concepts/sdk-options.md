---
title: Azure Iletişim Hizmetleri için istemci kitaplıkları ve REST API 'Leri
titleSuffix: An Azure Communication Services concept document
description: Azure Communication Services istemci kitaplıkları ve REST API 'Ler hakkında daha fazla bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/18/2020
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 07f09dbb7fa8d7c88dce3c0af32e3fee21656da7
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101691306"
---
# <a name="client-libraries-and-rest-apis"></a>İstemci kitaplıkları ve REST API’leri

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Azure Iletişim Hizmetleri Özellikleri, kavramsal olarak altı alan halinde düzenlenir. Bazı alanlarda tamamen açık kaynaklı istemci kitaplıkları vardır. Çağıran istemci kitaplığı, özel ağ arabirimlerini kullanır ve şu anda kapalı kaynaklı ve sohbet kitaplığı kapalı kaynaklı bir bağımlılık içeriyor. [Azure Iletişim Hizmetleri GitHub](https://github.com/Azure/communication)deposunda, istemci kitaplıkları için örnekler ve ek teknik ayrıntılar yayımlanır.

## <a name="client-libraries"></a>İstemci kitaplıkları

| Bütünleştirilmiş Kod               | Protokoller             |Açık ve kapalı kaynaklı kaynağı aç| Ad alanları                          | Özellikler                                                      |
| ---------------------- | --------------------- | ---|-------------------------- | --------------------------------------------------------------------------- |
| Azure Resource Manager | REST | Aç            | Azure. ResourceManager. Communication | Iletişim Hizmetleri kaynaklarını sağlama ve yönetme             |
| Common                 | REST | Aç               | Azure. Communication. Common          | Diğer istemci kitaplıkları için temel türleri sağlar |
| Kimlik         | REST | Aç               | Azure. Communication. Identity  | Kullanıcıları ve erişim belirteçlerini yönetme |
| Sohbet                   | Özel sinyalle REST | Kapalı kaynak sinyal paketiyle aç    | Azure. Communication. sohbet            | Uygulamalarınıza gerçek zamanlı metin tabanlı sohbet ekleyin  |
| SMS                    | REST | Aç              | Azure. Communication. SMS             | SMS iletileri gönderme ve alma |
| Events                | Özel aktarım | Kapatıldı |Azure. Communication. çağrılıyor         | Ses, video, ekran paylaşımı ve diğer gerçek zamanlı veri iletişim özelliklerinden yararlanın          |

Azure Resource Manager, yönetim ve SMS istemci kitaplıklarının hizmet tümleştirilmesine odaklandığına ve birçok durumda, bu işlevleri Son Kullanıcı uygulamalarıyla tümleştirirseniz güvenlik sorunları ortaya çıkar. Ortak ve sohbet istemci kitaplıkları hizmet ve istemci uygulamaları için uygundur. Çağıran istemci kitaplığı, istemci uygulamaları için tasarlanmıştır. Hizmet senaryolarına odaklanan bir istemci kitaplığı geliştirmede.

### <a name="languages-and-publishing-locations"></a>Diller ve yayımlama konumları

Ayrı istemci kitaplığı paketleri için yayımlama konumları aşağıda ayrıntılı olarak verilmiştir. 

| Alan           | JavaScript | .NET | Python | Java SE | iOS | Android | Diğer                          |
| -------------- | ---------- | ---- | ------ | ---- | -------------- | -------------- | ------------------------------ |
| Azure Resource Manager | -         | [NuGet](https://www.nuget.org/packages/Azure.ResourceManager.Communication)    |   [Pypı](https://pypi.org/project/azure-mgmt-communication/)    |  -  | -              | -  | [GitHub aracılığıyla git](https://github.com/Azure/azure-sdk-for-go/releases/tag/v46.3.0) |
| Common         | [npm](https://www.npmjs.com/package/@azure/communication-common)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Common/)    | Yok      | [Maven](https://search.maven.org/search?q=a:azure-communication-common)   | [GitHub](https://github.com/Azure/azure-sdk-for-ios/releases/tag/1.0.0-beta.1)            | [Maven](https://search.maven.org/artifact/com.azure.android/azure-communication-common)             | -                              |
| Yönetim | [npm](https://www.npmjs.com/package/@azure/communication-administration)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Administration)    | [Pypı](https://pypi.org/project/azure-communication-administration/)      | [Maven](https://search.maven.org/search?q=a:azure-communication-administration)   | -              | -              | -                            |
| Kimlik | [npm](https://www.npmjs.com/package/@azure/communication-identity)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.identity)    | [Pypı](https://pypi.org/project/azure-communication-identity/)      | [Maven](https://search.maven.org/search?q=a:azure-communication-identity)   | -              | -              | -                            |
| Sohbet           | [npm](https://www.npmjs.com/package/@azure/communication-chat)        | [NuGet](https://www.nuget.org/packages/Azure.Communication.Chat)     | [Pypı](https://pypi.org/project/azure-communication-chat/)     | [Maven](https://search.maven.org/search?q=a:azure-communication-chat)   | [GitHub](https://github.com/Azure/azure-sdk-for-ios/releases)  | [Maven](https://search.maven.org/search?q=a:azure-communication-chat)   | -                              |
| SMS            | [npm](https://www.npmjs.com/package/@azure/communication-sms)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Sms)    | [Pypı](https://pypi.org/project/azure-communication-sms/)       | [Maven](https://search.maven.org/artifact/com.azure/azure-communication-sms)   | -              | -              | -                              |
| Events        | [npm](https://www.npmjs.com/package/@azure/communication-calling)         | -      | -      | -     | [GitHub](https://github.com/Azure/Communication/releases/tag/v1.0.0-beta.2)     | [Maven](https://search.maven.org/artifact/com.azure.android/azure-communication-calling/)            | -                              |
| Başvuru Belgeleri     | [belgeler](https://azure.github.io/azure-sdk-for-js/communication.html)         | [belgeler](https://azure.github.io/azure-sdk-for-net/communication.html)      | -      | [belgeler](http://azure.github.io/azure-sdk-for-java/communication.html)     | [belgeler](/objectivec/communication-services/calling/)      | [belgeler](/java/api/com.azure.communication.calling?view=communication-services-java-android)            | -                              |

## <a name="rest-apis"></a>REST API'leri

İletişim Hizmetleri API 'Leri, [docs.Microsoft.com](/rest/api/azure/)' deki DIĞER Azure REST API 'leri ile birlikte belgelenmiştir. Bu belgede, HTTP iletilerinizi nasıl yapılandıracağınızı ve Postman kullanmaya yönelik yönergeler sunulmaktadır. Bu belge [GitHub](https://github.com/Azure/azure-rest-api-specs)'da Swagger biçiminde de sunulur.

## <a name="additional-support-details"></a>Ek destek ayrıntıları

### <a name="ios-and-android-support-details"></a>iOS ve Android destek ayrıntıları

- İletişim Hizmetleri iOS istemci kitaplıkları hedef iOS sürüm 13 + ve Xcode 11 +.
- Android Java istemci kitaplıkları hedef Android API düzeyi 21 + ve Android Studio 4.0 +

### <a name="net-support-details"></a>.NET destek ayrıntıları

Arama dışında, Iletişim Hizmetleri paketleri .NET Standard 2,0 hedef, aşağıda listelenen platformları destekler.

.NET Framework 4.6.1 aracılığıyla destek
- Windows 10, 8,1, 8 ve 7
- Windows Server 2012 R2, 2012 ve 2008 R2 SP1

.NET Core 2,0 aracılığıyla destek:
- Windows 10 (1607 +), 7 SP1 +, 8,1
- Windows Server 2008 R2 SP1 +
- Max OS X 10.12 +
- Linux çoklu sürüm/dağıtım
- UWP 10.0.16299 (RS3) Eylül 2017
- Unity 2018,1
- Mono 5,4
- Xamarin iOS 10,14
- Xamarin Mac 3,8

## <a name="calling-client-library-timeouts"></a>İstemci kitaplığı zaman aşımlarını çağırma

İstemci kitaplıklarını çağıran Iletişim Hizmetleri için aşağıdaki zaman aşımları geçerlidir:

| Eylem           | Saniye olarak zaman aşımı |
| -------------- | ---------- |
| Yeniden bağlanma/kaldırma Katılımcısı | 120 |
| Bir çağrıdan yeni moditesi ekleme veya kaldırma (videoyu Başlat/Durdur veya ekran paylaşımı) | 40 |
| Çağrı aktarımı işlem zaman aşımı | 60 |
| 1:1 çağrı kurma zaman aşımı | 85 |
| Grup çağrısı kurma zaman aşımı | 85 |
| PSTN çağrısı kurma zaman aşımı | 115 |
| Bir grup çağrısı zaman aşımı için 1:1 çağrısını yükseltin | 115 |


## <a name="api-stability-expectations"></a>API kararlılık beklentileri 

> [!IMPORTANT]
> Bu bölüm, REST API 'Ler ve istemci kitaplıkları için **kararlı** olarak işaretlenmiş yönergeler sağlar. Yayın öncesi, önizleme veya beta olarak işaretlenen API 'Ler, **bildirimde bulunulmadan** değiştirilebilir veya kullanım dışı bırakılmış olabilir. Şu anda Azure Iletişim Hizmetleri **genel önizlemede** ve API 'ler bu şekilde işaretlenir.

Gelecekte Iletişim Hizmetleri istemci kitaplıklarının sürümlerini devre dışı bırakabiliriz ve REST API 'lerimiz ve sunulan istemci kitaplıklarında önemli değişiklikler sunabiliriz. Azure iletişim hizmetleri, hizmet sürümlerinin kullanımdan kaldırılması için *genellikle* iki desteklenebilirlik ilkesi izler:

- Bir Iletişim Hizmetleri arabirimi değişikliği nedeniyle kodu değiştirmeniz gerekmeden önce en az üç yıl önce bilgilendirilirsiniz. Tüm belgelenen REST API 'Leri ve istemci kitaplığı API 'Leri, arabirimlerin kullanımdan çıkarmadan önce en az üç yıllık uyarı ister.
- İstemci kitaplığı derlemelerini en son ikincil sürüme güncelleştirmek zorunda kalmadan önce en az bir yıl bilgilendirilirsiniz. Bu gerekli güncelleştirmeler aynı ana sürümde olduklarından herhangi bir kod değişikliği gerektirmemelidir. Bu özellikle, genellikle güvenlik ve performans güncelleştirmeleri gerektiren gerçek zamanlı bileşenlere sahip olan çağrı ve sohbet kitaplıkları için geçerlidir. Iletişim Hizmetleri istemci kitaplıklarınızı güncel tutmanızı kesinlikle öneririz.

### <a name="api-and-client-library-decommissioning-examples"></a>API ve istemci kitaplığı yetki alma örnekleri

**SMS REST API v24 sürümünü uygulamanıza tümleştirdiniz. Azure Iletişim yayınları v25.**

Bu API 'Ler çalışmayı durdurmadan 3 yıllık uyarı alacaksınız ve v25 ' ye güncelleştirme zorlanacaktır. Bu güncelleştirme için bir kod değişikliği gerekebilir.

**Çağıran istemci kitaplığının v 2.02 sürümünü uygulamanıza tümleştirdiniz. Azure Iletişim yayımları v 2.05.**

V 2.05 sürümünün 12 ay içinde çağıran istemci kitaplığının v 2.05 sürümüne güncelleştirmeniz gerekebilir. Bu, sanal 2.05 v2 ana sürümünde olduğundan ve hiç bir değişiklik olmadığından, bir kod değişikliğine gerek olmadan yapıtın basit bir değişikliği olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için, aşağıdaki istemci kitaplığına genel bakışlara bakın:

- [İstemci kitaplığına genel bakış çağrılıyor](../concepts/voice-video-calling/calling-sdk-features.md)
- [Sohbet istemci kitaplığına genel bakış](../concepts/chat/sdk-features.md)
- [SMS istemci kitaplığına genel bakış](../concepts/telephony-sms/sdk-features.md)

Azure Iletişim Hizmetleri 'ni kullanmaya başlamak için:

- [Azure Iletişim kaynakları oluşturma](../quickstarts/create-communication-resource.md)
- [Kullanıcı erişim belirteçleri](../quickstarts/access-tokens.md) oluştur