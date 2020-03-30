---
title: Tehditler - Microsoft Tehdit Modelleme Aracı - Azure | Microsoft Dokümanlar
description: Microsoft Tehdit Modelleme Aracı için tehdit kategorisi sayfası, tüm maruz kalan tehditler için kategoriler içeren.
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.subservice: security-develop
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: eb006482b851e9094b82ec3d0753b74c05296994
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68727842"
---
# <a name="microsoft-threat-modeling-tool-threats"></a>Microsoft Tehdit Modelleme Aracı tehditleri

Tehdit Modelleme Aracı, Microsoft Güvenlik Geliştirme Yaşam Döngüsü'nün (SDL) temel bir öğesidir. Yazılım mimarlarının, çözümlenebilmek için nispeten kolay ve uygun maliyetli olduklarında olası güvenlik sorunlarını erkenden tanımlamasına ve azaltmasına olanak tanır. Sonuç olarak, büyük ölçüde geliştirme toplam maliyetini azaltır. Ayrıca, tehdit modelleri oluşturma ve analiz etme konusunda net rehberlik sağlayarak tehdit modellemeyi tüm geliştiriciler için daha kolay hale getirerek aracı güvenlik dışı uzmanları göz önünde bulundurarak tasarladık.

> Bugün başlamak için **[Tehdit Modelleme Aracı'nı](threat-modeling-tool.md)** ziyaret edin!

Tehdit Modelleme Aracı, aşağıdakiler gibi belirli soruları yanıtlamanıza yardımcı olur:

* Bir saldırgan kimlik doğrulama verilerini nasıl değiştirebilir?
* Bir saldırgan kullanıcı profil verilerini okuyabiliyorsa bunun etkisi nedir?
* Kullanıcı profili veritabanına erişim reddedilirse ne olur?

## <a name="stride-model"></a>STRIDE modeli

Bu tür işaretli soruları formüle etmeye daha iyi yardımcı olmak için Microsoft, farklı tehdit türlerini kategorilere ayıran ve genel güvenlik konuşmalarını basitleştiren STRIDE modelini kullanır.

| Kategori | Açıklama |
| -------- | ----------- |
| **Kandırma** | Kullanıcı adı ve parola gibi başka bir kullanıcının kimlik doğrulama bilgilerine yasa dışı olarak erişmeve ardından kullanılmasını içerir |
| **İzinsiz Değişiklik** | Verilerin kötü amaçlı modifikasyoniçerir. Bunlara örnek olarak, veritabanında tutulan kalıcı verilerde yapılan yetkisiz değişiklikler ve Internet gibi açık bir ağ üzerinden iki bilgisayar arasında akarken verilerin değiştirilmesi verilebilir |
| **Tanımadığı** | Diğer tarafların aksini kanıtlaması için herhangi bir yolu olmadan bir eylem gerçekleştirmeyi reddeden kullanıcılarla ilişkili olarak, örneğin, bir kullanıcı yasaklı işlemleri izleme yeteneğine sahip olmayan bir sistemde yasadışı bir işlem gerçekleştirir. Reddetme, bir sistemin reddetme tehditlerine karşı koyabilme yeteneğini ifade eder. Örneğin, bir öğeyi satın alan bir kullanıcının, alındıktan sonra öğeyi imzalaması gerekebilir. Satıcı daha sonra imzalanmış makbuzu kullanıcının paketi aldığının kanıtı olarak kullanabilir |
| **Bilgileri Açıklama** | Bu bilgilere erişimi olmaması gereken kişilere bilgi maruziyeti içerir (örneğin, kullanıcıların erişim izni verilmeyen bir dosyayı okuma yeteneği veya bir davetsiz misafirin iki bilgisayar arasında geçiş sırasında verileri okuyabilme yeteneği |
| **Hizmet Reddi** | Hizmet reddi (DoS) saldırıları, bir Web sunucusunun geçici olarak kullanılamaz veya kullanılamaz hale getirilmesi gibi geçerli kullanıcılara hizmet reddedilir. Sistemin kullanılabilirliğini ve güvenilirliğini artırmak için belirli DoS tehditlerine karşı koruma nız gerekir |
| **Ayrıcalıkların Yükseltilmesi** | Ayrıcalıksız bir kullanıcı ayrıcalıklı erişim elde eder ve bu şekilde tüm sistemi tehlikeye atmak veya yok etmek için yeterli erişime sahiptir. Ayrıcalık tehditlerinin yükselmesi, saldırganın tüm sistem savunmalarına etkin bir şekilde sızdığı ve güvenilir sistemin bir parçası haline geldiği durumları içerir, gerçekten de tehlikeli bir durum |

## <a name="next-steps"></a>Sonraki adımlar

Azure ile bu tehditleri azaltmanın farklı yollarını öğrenmek için **[Tehdit Modelleme Aracı Azaltımlarına](threat-modeling-tool-mitigations.md)** devam edin.