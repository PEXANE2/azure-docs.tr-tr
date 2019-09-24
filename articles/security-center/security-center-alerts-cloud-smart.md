---
title: Azure Güvenlik Merkezi 'nde bulut Akıllı uyarı bağıntısı (olaylar) | Microsoft Docs
description: Bu konu, Fusion 'un Azure Güvenlik Merkezi 'nde güvenlik olayları oluşturmak için bulut Akıllı uyarı bağıntısını nasıl kullandığını açıklar.
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
ms.openlocfilehash: 4c2f084fe03271b29a12aa6906f5e36612d50674
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71202696"
---
# <a name="cloud-smart-alert-correlation-in-azure-security-center-incidents"></a>Azure Güvenlik Merkezi 'nde bulut Akıllı uyarı bağıntısı (olaylar)

Azure Güvenlik Merkezi, kötü amaçlı etkinlikler hakkında sizi uyarmak için gelişmiş analiz ve tehdit bilgilerini kullanarak karma bulut iş yüklerini sürekli olarak analiz eder.

Tehdit kapsamının kapsamı artıyor. Farklı uyarıları önceliklendirmek ve gerçek bir saldırıyı belirlemek Güvenlik analistlerinin de belirlenmesi gerekir. Güvenlik Merkezi bu uyarı ile analistlere yardımcı olur. Farklı uyarıların ve düşük doğruluk sinyallerinin güvenlik olaylarına sahip olduğu gibi saldırıları tanılamanıza yardımcı olur.

Fusion Analytics, güvenlik merkezi olaylarını güçlendirir ve farklı uyarıların ve bağlamsal sinyallerin birlikte ilişkilendirilene olanak tanıyan teknoloji ve analitik arka bitidir. Fusion, kaynaklar genelinde bir abonelikte bildirilen farklı sinyallere bakar. Fusion, paylaşılan bağlamsal bilgilerle saldırı ilerlemesini veya sinyalleri açığa çıkarmak için birleştirilmiş bir yanıt prosedürü kullanacağınızı belirten desenler bulur.

Fusion Analytics, uyarıları analiz etmek ve yeni saldırı desenlerini keşfetmek üzere AI ile güvenlik etki alanı bilgisini birleştirir. 

Güvenlik Merkezi, uyarıları algılanan amaçla ilişkilendirmek için MITRE saldırı matrisini kullanır, bu da güvenlik etki alanı bilgisini şekillendirmeye yardımcı olur. Ayrıca, güvenlik merkezi bir saldırının her adımında toplanan bilgileri kullanarak bir saldırının adımları gibi görünen etkinliği ayarlayabilir ancak aslında değildir.  

Saldırı genellikle farklı kiracılar genelinde gerçekleştiğinden, güvenlik merkezi her abonelikte bildirilen saldırı dizilerini çözümlemek için AI algoritmalarını birleştirebilir. Bu teknik, saldırı dizilerini, yalnızca birbirleriyle ilişkilendirilmiş arada yerine yaygın olarak karşılaşılan uyarı desenleri olarak tanımlar.

Bir olayın araştırılması sırasında Analistler genellikle tehdit doğası ve bunun nasıl azaltılacağını öğrenmek için ek bağlam gerektirir. Örneğin, bir ağ anomali algılandığında, ağda başka ne olduğunu veya hedeflenen kaynakla ilgili olduğunu anlamadan bile, ileri yapılacak işlemleri anlamak zordur. Yardım için, bir güvenlik olayının yapıtları, ilgili olayları ve bilgileri içermesi olabilir. Güvenlik olayları için kullanılabilen ek bilgiler, algılanan tehdit türüne ve ortamınızın yapılandırmasına bağlı olarak farklılık gösterir. 

![Güvenlik olayı algılanan raporunun ekran görüntüsü](./media/security-center-alerts-cloud-smart/security-incident.png)

Güvenlik olaylarını daha iyi anlamak için bkz. [Azure Güvenlik Merkezi 'nde güvenlik olaylarını işleme](https://docs.microsoft.com/azure/security-center/security-center-incident).

