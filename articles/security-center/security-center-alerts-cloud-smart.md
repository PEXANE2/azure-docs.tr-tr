---
title: Azure Güvenlik Merkezi 'nde bulut Akıllı uyarı bağıntısı (olaylar) | Microsoft Docs
description: Bu konu, Fusion 'un Azure Güvenlik Merkezi 'nde güvenlik olayları oluşturmak için bulut Akıllı uyarı bağıntısını nasıl kullandığını açıklar.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: e9d5a771-bfbe-458c-9a9b-a10ece895ec1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 7ba2cf14d9fac100f44a1ef23997b27ba062bee0
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295855"
---
# <a name="cloud-smart-alert-correlation-in-azure-security-center-incidents"></a>Azure Güvenlik Merkezi 'nde bulut Akıllı uyarı bağıntısı (olaylar)

Güvenlik Merkezi, kötü amaçlı etkinlikler hakkında sizi uyarmak için gelişmiş analiz ve tehdit bilgilerini kullanarak karma bulut iş yüklerini sürekli olarak analiz eder.

Tehdit kapsamının kapsamı büyüdükçe ve güvenliği aşılmış bir şekilde artmayı algılama gereksinimi arttıkça, Güvenlik analistlerinin farklı uyarıları değerlendirmesi ve gerçek bir saldırının belirlenmesi zor olabilir. Güvenlik Merkezi, farklı uyarıların ve düşük aslına uygunluk sinyallerinin güvenlik olaylarına karşı ilişkilendirilmesi ve uyarı azıg ile analistleri tanılarken analistlere yardımcı olur.

Fusion, güvenlik merkezi olaylarını güçlendirir ve farklı uyarıların ve bağlamsal sinyallerin birlikte ilişkilendirilene olanak tanıyan teknoloji ve analitik arka bitidir. Fusion, kaynaklar genelinde bir abonelikte bildirilen farklı sinyallere bakarak ve Birleşik bir yanıt yordamının ne olması gerektiğini belirten paylaşılan bağlamsal bilgilerle saldırı ilerlemesini veya sinyalleri gösteren yaygın desenleri bularak işe yarar. Bunlar için alınmıştır.

Fusion Analytics, uyarıları analiz etmek ve yeni saldırı desenlerini keşfetmek için AI ile güvenlik etki alanı bilgisini birleştirir. 

Güvenlik Merkezi, uyarıları algılanan amaçla ilişkilendirmek için MITRE saldırı matrisini kullanır, bu da güvenlik etki alanı bilgisini şekillendirmeye yardımcı olur. Ayrıca, güvenlik merkezi bir saldırının her adımı için toplanan bilgileri kullanarak bir saldırının adımları gibi görünen, ancak olmayan etkinlikleri ayarlayabilir.  

Saldırı genellikle farklı kiracılar genelinde gerçekleşdiğinden, Güvenlik Merkezi, her bir abonelikte bildirilen saldırı dizilerini çözümlemek için her bir abonelikte bildirilen saldırı dizilerini analiz etmek üzere AI algoritmalarını birleştirebilir farklı.

Bir olayın araştırılması sırasında Analistler genellikle tehdit doğası ve bunun nasıl azaltılacağını öğrenmek için ek bağlam gerektirir. Örneğin, ağ anomali algılandığında, ağda başka ne olduğunu veya hedeflenen kaynakla ilgili olduğunu anlamak zorunda kalmadan bile, hangi eylemlerin yapılacağını anlamak zordur. Yardım için, bir güvenlik olayının yapıtları, ilgili olayları ve bilgileri içermesi olabilir. Güvenlik olayları için kullanılabilen ek bilgiler, algılanan tehdit türüne ve ortamınızın yapılandırmasına bağlı olarak farklılık gösterir. 

![Güvenlik olayı ayrıntıları](./media/security-center-alerts-cloud-smart/security-incident.png)

Güvenlik olaylarını daha iyi anlamak için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik olaylarını işleme](https://docs.microsoft.com/azure/security-center/security-center-incident).

