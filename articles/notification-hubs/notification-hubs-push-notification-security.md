---
title: Bildirim Hub'ları güvenlik modeli
description: Azure Bildirim Hub'larının güvenlik modeli hakkında bilgi edinin.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: ''
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 09/23/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 09/23/2019
ms.openlocfilehash: b871775bc7a6d795e86147ae9cffa27bdd2f3348
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76263770"
---
# <a name="notification-hubs-security"></a>Bildirim Hub'ları güvenliği

## <a name="overview"></a>Genel Bakış

Bu konu, Azure Bildirim Hub'larının güvenlik modelini açıklar.

## <a name="shared-access-signature-security"></a>Paylaşılan Erişim İmzagüvenliği

Bildirim Hub'ları *Paylaşılan Erişim İmzası* (SAS) adı verilen varlık düzeyinde bir güvenlik düzeni uygular. Her kural, [güvenlik taleplerinde](#security-claims)daha sonra açıklandığı gibi bir ad, önemli bir değer (paylaşılan gizli) ve bir dizi hak içerir. 

Bir hub oluşturulurken, otomatik olarak iki kural oluşturulur: biri **Dinleme** haklarına sahip (istemci uygulamasının kullandığı) ve diğeri **tüm** haklara sahip (uygulamanın arka ucunun kullandığı):

- **DefaultListenSharedAccessSignature**: yalnızca **dinleme** izni verir.
- **DefaultFullSharedAccessSignature**: **dinleme,** **yönetme**ve izin **gönder'** i verir. Bu ilke yalnızca uygulama arka uçunuzda kullanılacaktır. İstemci uygulamalarında kullanmayın; yalnızca **Dinle** erişimine sahip bir ilke kullanın. Yeni bir SAS belirteciyle yeni bir özel erişim ilkesi oluşturmak için, bu makalenin ilerleyen saatlerinde [erişim ilkeleri için SAS belirteçlerine](#sas-tokens-for-access-policies) bakın.

Müşteri uygulamalarından kayıt yönetimi gerçekleştirirken, bildirimler yoluyla gönderilen bilgiler hassas değilse (örneğin, hava durumu güncellemeleri), bildirim hub'ına erişmenin yaygın bir yolu, istemci uygulamasına yalnızca Dinle kuralının anahtar değerini vermektir, ve uygulama arka sonuna kuralın anahtar değerini tam erişim vermek için.

Uygulamalar Windows Mağazası istemci uygulamalarına anahtar değerini katıştırmamalıdır; bunun yerine, istemci uygulamasının başlangıçta uygulama arka ucundan geri almasına izin verebin.

**Listen** erişimine sahip anahtar, istemci uygulamasının herhangi bir etiket için kaydolmasını sağlar. Uygulamanızın kayıtları belirli istemcilerle belirli etiketlerle sınırlaması gerekiyorsa (örneğin, etiketler kullanıcı adlarını temsil ettiğinde), uygulama arka uçunuzun kayıtları gerçekleştirmesi gerekir. Daha fazla bilgi için [Kayıt yönetimine](notification-hubs-push-notification-registration-management.md)bakın. Bu şekilde, istemci uygulamasının Bildirim Hub'larına doğrudan erişimi olmayacağını unutmayın.

## <a name="security-claims"></a>Güvenlik talepleri

Diğer varlıklara benzer şekilde, Bildirim Hub işlemlerine üç güvenlik talebi için izin verilir: **Dinle,** **Gönder**ve **Yönet.**

| İste   | Açıklama                                          | İzin verilen işlemler |
| ------- | ---------------------------------------------------- | ------------------ |
| Dinle  | Tek kayıtları Oluşturma/Güncelleme, Okuma ve Silme | Kayıt oluşturma/güncelleme<br><br>Kaydı okuma<br><br>Bir tutamaç için tüm kayıtları okuyun<br><br>Kaydı silme |
| Gönder    | Bildirim Hub'ına ileti gönderme                | İleti gönder |
| Yönetme  | Bildirim Hub'larında (PNS kimlik bilgilerini ve güvenlik anahtarlarını güncellemek dahil) ve etiketlere göre kayıtları okuma |Hub oluşturma/güncelleme/Okuma/Silme<br><br>Kayıtları etikete göre okuma |

Bildirim Hub'ları, doğrudan hub üzerinde yapılandırılan paylaşılan anahtarlarla oluşturulan SAS belirteçlerini kabul eder.

Birden fazla ad alanına bildirim göndermek mümkün değildir. Ad alanları Bildirim Hub'ları için mantıksal kapsayıcılardır ve bildirim göndermede yer almayan lardır.

Ad alanı düzeyindeki işlemler için ad alanı düzeyinde erişim ilkelerini (kimlik bilgilerini) kullanın; örneğin: hub'ları listeleme, hub oluşturma veya silme, vb. Yalnızca hub düzeyindeki erişim ilkeleri bildirim göndermenize izin veriyor.

### <a name="sas-tokens-for-access-policies"></a>Erişim ilkeleri için SAS belirteçleri

Yeni bir güvenlik talebi oluşturmak veya varolan SAS anahtarlarını görüntülemek için aşağıdakileri yapın:

1. Azure Portal’da oturum açın.
2. **Tüm kaynaklar**’ı seçin.
3. Talep oluşturmak veya SAS anahtarını görüntülemek istediğiniz Bildirim Merkezi'nin adını seçin.
4. Sol menüde **Erişim İlkeleri'ni**seçin.
5. Yeni bir güvenlik talebi oluşturmak için **Yeni İlke'yi** seçin. İlkeye bir ad verin ve vermek istediğiniz izinleri seçin. Sonra **Tamam**’ı seçin.
6. Erişim İlkeleri penceresinde tam bağlantı dizesi (yeni SAS anahtarı dahil) görüntülenir. Bu dizeyi daha sonra kullanmak üzere panoya kopyalayabilirsiniz.

SAS anahtarını belirli bir ilkeden ayıklamak için, istediğiniz SAS anahtarını içeren ilkeğin yanındaki **Kopyala** düğmesini seçin. Bu değeri geçici bir konuma yapıştırın ve ardından bağlantı dizesinin SAS anahtar bölümünü kopyalayın. Bu örnek, **mytestnamespace1**adlı bir Bildirim Hub'ları ad alanı ve **ilke 2**adlı bir ilke kullanır. SAS tuşu, **SharedAccessKey**tarafından belirtilen dize sonuna yakın değerdir:

```shell
Endpoint=sb://mytestnamespace1.servicebus.windows.net/;SharedAccessKeyName=policy2;SharedAccessKey=<SAS key value here>
```

![SAS tuşlarını alın](media/notification-hubs-push-notification-security/access1.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Bildirim Hub'larına genel bakış](notification-hubs-push-notification-overview.md)
