---
title: Azure Iletişim Hizmetleri için sohbet istemci kitaplığına genel bakış
titleSuffix: An Azure Communication Services concept document
description: Azure Iletişim Hizmetleri sohbeti istemci kitaplığı hakkında bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 93f90520f9a5f6ec424a7558418abfa4de4699ee
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100364843"
---
# <a name="chat-client-library-overview"></a>Sohbet istemci kitaplığına genel bakış

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Azure Iletişim Hizmetleri sohbet istemci kitaplıkları, uygulamalarınıza zengin ve gerçek zamanlı sohbet eklemek için kullanılabilir.

## <a name="chat-client-library-capabilities"></a>Sohbet istemci kitaplığı özellikleri

Aşağıdaki listede, Iletişim Hizmetleri sohbeti istemci kitaplıklarında Şu anda kullanılabilir olan özellikler kümesi sunulmaktadır.

| Özellik grubu | Özellik                                                                                                          | JS  | Java | .NET | Python |
| ----------------- | ------------------------------------------------------------------------------------------------------------------- | --- | ----- | ---- | -----  |
| Temel yetenekler | 2 veya daha fazla kullanıcı arasında sohbet iş parçacığı oluşturma (en fazla 250 Kullanıcı)                                                       | ✔️   | ✔️  | ✔️    | ✔️   |
|                   | Sohbet iş parçacığının konusunu güncelleştirme                                                                              | ✔️   | ✔️ | ✔️    | ✔️   |
|                   | Sohbet iş parçacığından üye ekleme veya kaldırma                                                                           | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | Sohbet iletisi geçmişinin yeni eklenen üyelerle paylaşıp paylaşamayacağını seçin- *Tümü/hiçbiri/belirli bir zamana kadar* | ✔️   | ✔️   | ✔️    | ✔️  |
|                   | Tüm sohbet üyeleri iş parçacığının listesini al                                                                          | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | Sohbet iş parçacığını silme                                                                                              | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | Kullanıcının sohbet iş parçacığı üyeliklerinin bir listesini alın                                                                  | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | Belirli bir sohbet iş parçacığı için bilgi alın                                                                              | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | Sohbet iş parçacığında ileti gönderme ve alma                                                                            | ✔️   | ✔️   | ✔️    | ✔️  |
|                   | Gönderildikten sonra iletinin içeriğini düzenleme                                                                   | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | İleti silme                                                                                                       | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | Gönderme sırasında bir iletiyi normal veya yüksek önceliğe göre etiketleme                                               | ✔️   | ✔️  | ✔️    | ✔️   |
|                   | Üyeler tarafından okunan iletiler için okundu bilgisi gönderme ve alma <br/> *Sohbet iş parçacığında 20 ' den fazla üye varsa kullanılamaz*    | ✔️   | ✔️  | ✔️    | ✔️   |
|                   | Bir üye sohbet iş parçacığına etkin bir ileti yazarken yazma bildirimleri gönderme ve alma <br/> *Sohbet iş parçacığında 20 ' den fazla üye varsa kullanılamaz*      | ✔️   | ✔️   | ✔️    | ✔️    |
|                   | Sohbet iş parçacığında tüm iletileri al <br/> *Desteklenen Unicode emojıs*                                                  | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | İleti içeriğinin bir parçası olarak emojıs gönder                                                                              | ✔️   | ✔️  | ✔️    | ✔️  |
|Gerçek zamanlı sinyal (özel sinyal paketi tarafından etkin * *)| Bir Kullanıcı bir sohbet iş parçacığında bir üyesi olan yeni bir ileti aldığında bildirim alın                                     | ✔️   | ❌    | ❌  | ❌  |
|                    | Bir kullanıcının üyesi olduğu bir sohbet iş parçacığında başka bir üye tarafından bir ileti düzenlendiğinde bildirim alın                | ✔️   | ❌    | ❌    | ❌  |
|                    | Bir kullanıcının üyesi olduğu bir sohbet iş parçacığında başka bir üye tarafından bir ileti silindiğinde bildirim alın                | ✔️   | ❌    | ❌    | ❌  |
|                    | Başka bir sohbet iş parçacığı üyesi yazıldığında bildirim alın                                                             | ✔️   | ❌    | ❌    | ❌  |
|                    | Sohbet iş parçacığında başka bir üyenin bir iletiyi okumasından (okundu alındısı) bildirim alın                               | ✔️   | ❌    | ❌    | ❌  |
| Ekinlikler             | Sohbet iş parçacıklarında oluşan Kullanıcı etkinliğine abone olmak ve özel bildirim hizmetlerini veya iş mantığını bütünleştirmek için Event Grid kullanın     | ✔️   | ✔️  | ✔️    | ✔️  |
| İzleme        | Gönderilen ileti koşullarına göre kullanımı izleme                                                                               | ✔️   | ✔️  | ✔️    | ✔️  |
|                    | Uygulamanız tarafından yapılan API isteklerinin kalitesini ve durumunu izleyin ve uyarıları Portal aracılığıyla yapılandırın                                                          | ✔️   | ✔️  | ✔️    | ✔️  |
|Ek özellikler | Akıllı Özellikler *dil çevirisi 'ni, bir istemcideki gelen iletinin yaklaşım analizini &, üye kulakça bir ileti oluşturmak üzere konuşmayı metne dönüştürmeye, vb.* sağlamak için sohbet istemci kitaplığı ile birlikte [bilişsel hizmetler API'si](../../../cognitive-services/index.yml) kullanın.                                                                                         | ✔️   | ✔️  | ✔️    | ✔️  |

* * Özel sinyal paketi Web Yuvaları kullanılarak uygulanır. Web Yuvaları desteklenmiyorsa uzun yoklamaya geri dönüş yapılır.

## <a name="javascript-chat-client-library-support-by-os-and-browser"></a>İşletim sistemi ve tarayıcı tarafından JavaScript chat istemci kitaplığı desteği

Aşağıdaki tablo, şu anda kullanılabilir olan desteklenen tarayıcı ve sürümlerin kümesini temsil eder.

|                                  | Windows          | Mac OS          | Ubuntu | Linux  | Android | iOS    | iPad işletim sistemi|
| -------------------------------- | ---------------- | -------------- | ------- | ------ | ------ | ------ | -------|
| **Sohbet istemci kitaplığı** | Firefox *, Chrome*, yeni kenar | Firefox *, Chrome*, Safari * | Mu  | Mu | Mu | Uygulamasını | Uygulamasını |


* Önceki iki sürüme ek olarak en son sürümün desteklendiğini unutmayın.<br/>

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sohbetle çalışmaya başlama](../../quickstarts/chat/get-started.md)

Aşağıdaki belgeler sizin için ilginç olabilir:

- [Sohbet kavramları](../chat/concepts.md) hakkında bilgi edinin
