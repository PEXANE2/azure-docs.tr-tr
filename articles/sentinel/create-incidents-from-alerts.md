---
title: Azure Sentinel'deki uyarılardan olay oluşturma | Microsoft Dokümanlar
description: Azure Sentinel'deki uyarılardan nasıl olay oluşturabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: b29b337d7487087bec268528ff26617f7a995235
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77587983"
---
# <a name="automatically-create-incidents-from-microsoft-security-alerts"></a>Microsoft güvenlik uyarılarından otomatik olarak olay oluşturma

Microsoft Bulut Uygulama Güvenliği ve Azure Gelişmiş Tehdit Koruması gibi Azure Sentinel'e bağlı Microsoft güvenlik çözümlerinde tetiklenen uyarılar, Azure Sentinel'de otomatik olarak olay oluşturmaz. Varsayılan olarak, bir Microsoft çözümünü Azure Sentinel'e bağladığınızda, bu hizmette oluşturulan herhangi bir uyarı Azure Sentinel çalışma alanınızdaki Güvenlik Uyarısı tablosunda Ham Veri olarak depolanır. Daha sonra sentinel'e bağlandığınız diğer ham veriler gibi bu verileri kullanabilirsiniz.

Azure Sentinel'i, bağlı bir Microsoft güvenlik çözümünde her uyarı tetikleninher olduğunda, bu makaledeki yönergeleri izleyerek olayları otomatik olarak oluşturacak şekilde kolayca yapılandırabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Güvenlik hizmeti uyarılarından olay oluşturmayı etkinleştirmek için [Microsoft güvenlik çözümlerini bağlamanız](connect-data-sources.md#data-connection-methods) gerekir.

## <a name="using-microsoft-security-incident-creation-analytic-rules"></a>Microsoft Security olay oluşturma analitik kurallarını kullanma

Hangi bağlı Microsoft güvenlik çözümlerinin azure sentinel olaylarını gerçek zamanlı olarak otomatik olarak oluştureceğini seçmek için Azure Sentinel'de bulunan yerleşik kuralları kullanın. Microsoft güvenlik çözümü tarafından oluşturulan uyarılardan hangisinin Azure Sentinel'de olay oluşturması gerektiğini filtrelemek için daha özel seçenekler tanımlamak için kuralları da düzenleme yapabilirsiniz. Örneğin, Azure Sentinel olaylarını yalnızca yüksek öneme yönelik Azure Güvenlik Merkezi uyarılarından otomatik olarak oluşturmayı seçebilirsiniz.

1. Azure Sentinel altındaki Azure portalında **Analytics'i**seçin.

1. Yerleşik analitik kuralların tümünü görmek için **Kural şablonları** sekmesini seçin.

    ![Kural şablonları](media/incidents-from-alerts/rule-templates.png)

1. Kullanmak istediğiniz **Microsoft güvenlik** analizi kuralı şablonunu seçin ve **Oluştur kuralını**tıklatın.

    ![Güvenlik analitiği kuralı](media/incidents-from-alerts/security-analytics-rule.png)

1. Kural ayrıntılarını değiştirebilir ve uyarı şiddetine veya uyarının adında bulunan metne göre olay yaratacak uyarıları filtrelemeyi seçebilirsiniz.  
      
    Örneğin, **Microsoft güvenlik hizmeti** alanında Azure Güvenlik **Merkezi'ni** seçerseniz ve **önem derecesine göre Filtre'de** **Yüksek'i** seçerseniz, yalnızca yüksek öneme karşı Azure Güvenlik Merkezi uyarıları Azure Sentinel'de otomatik olarak olay oluşturur.  

    ![Kural sihirbazı oluşturma](media/incidents-from-alerts/create-rule-wizard.png)

1. Ayrıca, **+Oluştur'u** tıklatıp **Microsoft olay oluşturma kuralını**seçerek farklı Microsoft güvenlik hizmetlerinden gelen uyarıları filtreleyen yeni bir **Microsoft güvenlik** kuralı da oluşturabilirsiniz.

    ![Olay oluşturma kuralı](media/incidents-from-alerts/incident-creation-rule.png)

  **Microsoft güvenlik hizmeti** türüne göre birden fazla Microsoft **Güvenlik** analitik kuralı oluşturabilirsiniz. Her kural filtre olarak kullanıldığından, bu yinelenen olaylar oluşturmaz. Bir uyarı birden fazla **Microsoft Security** analitik kuralıyla eşleşse bile, sadece bir Azure Sentinel olayı oluşturur.

## <a name="enable-incident-generation-automatically-during-connection"></a>Bağlantı sırasında olay oluşturmayı otomatik olarak etkinleştirme
 Bir Microsoft güvenlik çözümü bağladığınızda, Azure Sentinel'de otomatik olarak olay oluşturmak için güvenlik çözümündeki uyarıların otomatik olarak olmasını isteyip istemediğinizi seçebilirsiniz.

1. Bir Microsoft güvenlik çözümü veri kaynağını bağlayın. 

   ![Güvenlik olayları oluşturma](media/incidents-from-alerts/generate-security-incidents.png)

1. **Olayları Oluştur** altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturan varsayılan analitik kuralını etkinleştir'i **seçin.** Daha sonra bu kuralı **Analytics** ve ardından **Etkin kurallar**altında edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Sentinel'e başlamak için Microsoft Azure aboneliğine ihtiyacınız var. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- [Verilerinizi Azure Sentinel'e nasıl ekeceklerinizi](quickstart-onboard.md)ve [verilerinizle ve olası tehditlerde görünürlüğü](quickstart-get-visibility.md)nasıl elde edebilirsiniz öğrenin.
