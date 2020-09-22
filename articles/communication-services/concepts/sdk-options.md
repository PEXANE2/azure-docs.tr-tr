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
ms.openlocfilehash: f39aa76e4bd2ce3d298e555f56b09d0218ef0862
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941164"
---
# <a name="client-libraries-and-rest-apis"></a>İstemci kitaplıkları ve REST API 'Leri

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Azure Iletişim Hizmetleri Özellikleri, kavramsal olarak altı alan halinde düzenlenir. Bazı alanlarda tamamen açık kaynaklı istemci kitaplıkları vardır. Çağıran istemci kitaplığı, özel ağ arabirimlerini kullanır ve şu anda kapalı kaynaklı ve sohbet kitaplığı kapalı kaynaklı bir bağımlılık içeriyor. Tüm SDK 'lara ve örneklere bağlantılar [Azure Communication Services GitHub](https://github.com/Azure/communication)deposunda saklanır.

## <a name="client-libraries"></a>İstemci kitaplıkları

| Bütünleştirilmiş Kod               | Protokoller             |Açık ve kapalı kaynaklı kaynağı aç| Ad alanları                          | Özellikler                                                      |
| ---------------------- | --------------------- | ---|-------------------------- | --------------------------------------------------------------------------- |
| Azure Resource Manager | REST | Aç            | Azure. ResourceManager. Communication | Iletişim Hizmetleri kaynaklarını sağlama ve yönetme             |
| Common                 | REST | Aç               | Azure. Communication. Common          | Diğer istemci kitaplıkları için temel türleri sağlar |
| Yönetim         | REST |                | Azure. Communication. Administration  | Kullanıcıları, erişim belirteçlerini ve telefon numaralarını yönetme, standartlara uyumlu bir yönetim ve açma sunucusu ayırma |
| Sohbet                   | Özel sinyalle REST | Kapalı kaynak sinyal paketiyle aç    | Azure. Communication. sohbet            | Uygulamalarınıza gerçek zamanlı metin tabanlı sohbet ekleyin  |
| SMS                    | REST | Aç              | Azure. Communication. SMS             | SMS iletileri gönderme ve alma |
| Events                | Özel aktarım | Kapalı |Azure. Communication. çağrılıyor         | Ses, video, ekran paylaşımı ve diğer gerçek zamanlı veri iletişim özelliklerinden yararlanın          |

### <a name="client-library-language-support"></a>İstemci kitaplığı dil desteği

Ayrı istemci kitaplığı paketleri için kullanılabilirlik Kılavuzu ve zaman çizelgeleri aşağıda ayrıntılı olarak verilmiştir. [Azure yol haritası](https://azure.microsoft.com/updates/) , yaklaşan özelliklerle ilgili ek bilgiler sağlar.

| Alan           | JavaScript | .NET | Python | Java | Swift veya obj-C | Java (Android) | Diğer                          |
| -------------- | ---------- | ---- | ------ | ---- | -------------- | -------------- | ------------------------------ |
| Azure Resource Manager | ✔️         | ✔️    | ✔️      | -    | -              | *Henüz desteklenmiyor*  | GIT ve Azure CLı *henüz desteklenmiyor* |
| Common         | ✔️         | ✔️    | -      | ✔️   | ✔️            | ✔️             | -                              |
| Yönetim | ✔️         | ✔️    | ✔️      | ✔️   | -              | -              | CLI                            |
| Sohbet           | ✔️         | ✔️    | ✔️      | ✔️   | *Henüz desteklenmiyor*  | *Henüz desteklenmiyor*  | -                              |
| SMS            | ✔️         | ✔️    | ✔️      | ✔️   | -              | -              | -                              |
| Events        | ✔️         | -      | -      | -     | (Obj-C) ✔️     | ✔️            | -                              |

### <a name="client-library-public-repository-support"></a>İstemci kitaplığı ortak depo desteği

İletişim Hizmetleri, yerleşik kitaplıkları çeşitli genel depolarda yayımlar.

| Dil       | İçin iyileştirildi...                       | Paketleme |
| -------------- | ------------------------------------ | --------- |
| .NET           | Platformlar arası                       | NuGet     |
| Python         | Windows & Linux sunucuları              | Pypı      |
| Java (J2EE)    | Windows veya Linux sunucularındaki JVM      | Maven     |
| Java (Android) | Android istemci uygulamaları          | Maven     |
| JavaScript     | Tarayıcı istemci uygulamaları ve düğümü | NPM       |

## <a name="rest-apis"></a>REST API'leri

İletişim Hizmetleri API 'Leri, [docs.Microsoft.com](https://docs.microsoft.com/rest/api/azure/)' deki DIĞER Azure REST API 'leri ile birlikte belgelenmiştir. Bu belgede, HTTP iletilerinizi nasıl yapılandıracağınızı ve Postman kullanmaya yönelik yönergeler sunulmaktadır. Bu belge [GitHub](https://github.com/Azure/azure-rest-api-specs)'da Swagger biçiminde de sunulur.

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

## <a name="api-stability-expectations"></a>API kararlılık beklentileri 

> [!IMPORTANT]
> Bu bölüm, REST API 'Ler ve istemci kitaplıkları için **kararlı**olarak işaretlenmiş yönergeler sağlar. Yayın öncesi, önizleme veya beta olarak işaretlenen API 'Ler, **bildirimde bulunulmadan**değiştirilebilir veya kullanım dışı bırakılmış olabilir. Şu anda Azure Iletişim Hizmetleri **genel önizlemede**ve API 'ler bu şekilde işaretlenir.

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
