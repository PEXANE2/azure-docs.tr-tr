---
title: Azure Güvenlik Merkezi olayları - uyarıların akıllı korelasyonları
description: Bu konu, birleşimin Azure Güvenlik Merkezi'nde güvenlik olayları oluşturmak için bulut akıllı uyarı korelasyonuna nasıl görebildiğini açıklar.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e9d5a771-bfbe-458c-9a9b-a10ece895ec1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/02/2019
ms.author: memildin
ms.openlocfilehash: b26f0bab073ce248ca23bb8a815fa3e293ddba51
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73686481"
---
# <a name="cloud-smart-alert-correlation-in-azure-security-center-incidents"></a>Azure Güvenlik Merkezi'nde bulut akıllı uyarı korelasyon (olaylar)

Azure Güvenlik Merkezi, kötü amaçlı etkinlikler hakkında sizi uyarmak için gelişmiş analitik ve tehdit istihbaratLarını kullanarak karma bulut iş yüklerini sürekli olarak analiz eder.

Tehdit kapsamının genişliği artıyor. En ufak bir uzlaşmayı bile tespit etme ihtiyacı önemlidir ve güvenlik analistlerinin farklı uyarıları triyajlayıp gerçek bir saldırıyı tanımlamaları zor olabilir. Güvenlik Merkezi analistlerbu uyarı yorgunluğu ile başa çıkmak yardımcı olur. Farklı uyarıları ve düşük sadakat sinyallerini güvenlik olaylarına ilişkilendirerek saldırıların meydana geldiği anda tanılamalarına yardımcı olur.

Fusion analitiği, Güvenlik Merkezi olaylarını birbirine uyaran ve farklı uyarıları ve bağlamsal sinyalleri bir araya getiren teknoloji ve analitik arka uçtür. Fusion, kaynaklar arasında bir abonelikte bildirilen farklı sinyallere bakar. Füzyon, saldırı ilerlemesini veya sinyalleri paylaşılan bağlamsal bilgilerle ortaya çıkararak, bunlar için birleşik bir yanıt yordamı kullanmanız gerektiğini belirten desenler bulur.

Füzyon analitiği, uyarıları analiz etmek için güvenlik etki alanı bilgilerini AI ile birleştirir ve ortaya çıkan yeni saldırı modellerini keşfeder. 

Güvenlik Merkezi, uyarıları algılanan amaçlarıyla ilişkilendirmek için MITRE Attack Matrix'i kullanır ve güvenlik etki alanı bilgilerini resmileştirmeye yardımcı olur. Buna ek olarak, bir saldırının her adımı için toplanan bilgileri kullanarak, Güvenlik Merkezi bir saldırının adımları gibi görünen, ancak gerçekte olmayan etkinliği eleyebilir.

Saldırılar genellikle farklı kiracılar arasında meydana geldiğinden, Güvenlik Merkezi her abonelikte bildirilen saldırı dizilerini çözümlemek için AI algoritmalarını birleştirebilir. Bu teknik, saldırı dizilerini tesadüfen birbiriyle ilişkilendirilmek yerine yaygın uyarı desenleri olarak tanımlar.

Bir olayla ilgili bir soruşturma sırasında, analistler tehdidin niteliği ve nasıl hafifletilir hakkında bir karara varmak için genellikle ekstra içeriğe ihtiyaç duyarlar. Örneğin, ağda başka neler olduğunu anlamadan veya hedeflenen kaynakla ilgili olarak bir ağ anomalisi algılansa bile, bundan sonra hangi eylemlerin gerçekleşsüreceğini anlamak zordur. Yardımcı olmak için, bir güvenlik olayı yapıları, ilgili olayları ve bilgileri içerebilir. Güvenlik olayları için kullanılabilir ek bilgiler, algılanan tehdidin türüne ve ortamınızın yapılandırmasına bağlı olarak değişir. 

![Güvenlik olayı algılanan raporun ekran görüntüsü](./media/security-center-alerts-cloud-smart/security-incident.png)

Güvenlik olaylarını daha iyi anlamak [için Azure Güvenlik Merkezi'ndeki güvenlik olaylarını nasıl ele aleyeceğiniz](https://docs.microsoft.com/azure/security-center/security-center-incident)e bakın.

