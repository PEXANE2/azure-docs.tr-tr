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
ms.openlocfilehash: 705bd926c2ac6f414464254969b5c511c88891f0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104656116"
---
# <a name="chat-client-library-overview"></a>Sohbet istemci kitaplığına genel bakış  

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]    

Azure Iletişim Hizmetleri sohbet istemci kitaplıkları, uygulamalarınıza zengin ve gerçek zamanlı sohbet eklemek için kullanılabilir.
    
## <a name="chat-client-library-capabilities"></a>Sohbet istemci kitaplığı özellikleri 

Aşağıdaki listede, Iletişim Hizmetleri sohbeti istemci kitaplıklarında Şu anda kullanılabilir olan özellikler kümesi sunulmaktadır.  

| Özellik grubu | Özellik | JavaScript  | Java | .NET | Python | iOS | Android |
|-----------------|-------------------|---|-----|----|-----|----|----|
| Temel yetenekler | 2 veya daha fazla kullanıcı arasında sohbet iş parçacığı oluşturma (en fazla 250 Kullanıcı)                                                       | ✔️   | ✔️  | ✔️    | ✔️   |  ✔️    | ✔️   |    
|                   | Sohbet iş parçacığının konusunu güncelleştirme                                                                              | ✔️   | ✔️ | ✔️    | ✔️   |  ✔️    | ✔️   |   
|                   | Sohbet iş parçacığından katılımcı ekleme veya kaldırma                                                                           | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  
|                   | Sohbet iletisi geçmişini eklenmekte olan katılımcı ile paylaşıp paylaşmeyeceğinizi seçin                                   | ✔️   | ✔️   | ✔️    | ✔️  |  ✔️    | ✔️   | 
|                   | Sohbet iş parçacığında katılımcıların bir listesini alın                                                                          | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   | 
|                   | Sohbet iş parçacığını silme                                                                                              | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |    
|                   | Bir iletişim kullanıcısı verildiğinde, kullanıcının bir parçası olduğu sohbet iş parçacıklarının listesini alın                                           | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |   
|                   | Belirli bir sohbet iş parçacığı için bilgi alın                                                                              | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   |   
|                   | Sohbet iş parçacığında ileti gönderme ve alma                                                                            | ✔️   | ✔️   | ✔️    | ✔️  |  ✔️    | ✔️   |   
|                   | Gönderilen iletinin içeriğini düzenleme                                                                                | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   |   
|                   | İleti silme                                                                                                       | ✔️   | ✔️  | ✔️ | ✔️ |  ✔️    | ✔️   |   
|                   | Sohbetteki diğer katılımcılar tarafından okunan iletiler için okundu bilgileri <br/> *Sohbet iş parçacığında 20 ' den fazla katılımcı varsa kullanılamaz*    | ✔️   | ✔️  | ✔️    | ✔️   |  ✔️    | ✔️   |   
|                   | Katılımcılar bir sohbet iş parçacığına etkin bir şekilde ileti yazarken bildirim alın <br/> *Sohbet iş parçacığında 20 ' den fazla üye varsa kullanılamaz*      | ✔️   | ✔️   | ✔️    | ✔️    |  ✔️    | ✔️   | 
|                   | Sohbet iş parçacığında tüm iletileri al <br/>                                                                         | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  
|                   | İleti içeriğinin bir parçası olarak Unicode emojıs gönder                                                                            | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |    
|Gerçek zamanlı sinyal (özel sinyal paketi tarafından etkin * *)|  Gelen iletiler ve sohbet uygulamanızdaki diğer işlemlere yönelik gerçek zamanlı güncelleştirmeler almak için abone olun. Gerçek zamanlı sinyalte desteklenen güncelleştirmelerin listesini görmek için bkz. [sohbet kavramları](concepts.md#real-time-signaling)                                     | ✔️   | ❌    | ❌  | ❌  | ❌  | ❌  |    
| Event Grid desteği             | Azure Event Grid tümleştirme kullanın ve iletişim hizmetinizi, sohbet etkinliğine göre iş mantığını veya özel bir anında iletme bildirimi hizmetini yürütmeye yönelik olarak yapılandırın   | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  
| İzleme        | Panolar derlemek, sohbet uygulamanızın sistem durumunu izlemek ve tüm bunları anlamak için uyarıları ayarlamak üzere Azure portal yayılan API isteği ölçümlerini kullanın      | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |    
|                   | Iletişim Hizmetleri kaynağınızı, izleme ve tanılama amaçlarıyla sohbet operasyonel Günlükler alacak şekilde yapılandırın          | ✔️   | ✔️  | ✔️    | ✔️  |  ✔️    | ✔️   |  


* * Özel sinyal paketi Web Yuvaları kullanılarak uygulanır. Web Yuvaları desteklenmiyorsa uzun yoklamaya geri dönüş yapılır.  

## <a name="javascript-chat-client-library-support-by-os-and-browser"></a>İşletim sistemi ve tarayıcı tarafından JavaScript chat istemci kitaplığı desteği 

Aşağıdaki tablo, şu anda kullanılabilir olan desteklenen tarayıcı ve sürümlerin kümesini temsil eder.
    
|                                  | Windows          | Mac OS          | Ubuntu | Linux  | Android | iOS    | iPad işletim sistemi|
|--------------------------------|----------------|--------------|-------|------|------|------|-------|
| **Sohbet istemci kitaplığı** | Firefox *, Chrome*, yeni kenar | Firefox *, Chrome*, Safari * | Mu  | Mu | Mu | Uygulamasını | Uygulamasını |

* Önceki iki sürüme ek olarak en son sürümün desteklendiğini unutmayın.<br/>   

## <a name="next-steps"></a>Sonraki adımlar   

> [!div class="nextstepaction"] 
> [Sohbetle çalışmaya başlama](../../quickstarts/chat/get-started.md)    

Aşağıdaki belgeler sizin için ilginç olabilir:  
- [Sohbet kavramları](../chat/concepts.md) hakkında bilgi edinin