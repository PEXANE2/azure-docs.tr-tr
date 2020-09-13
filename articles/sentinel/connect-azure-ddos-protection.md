---
title: Azure DDoS koruma verilerini Azure Sentinel 'e bağlama
description: Azure DDoS koruma verilerinin Azure Sentinel 'e nasıl alınacağını öğrenin, bu sayede onu görüntüleyebilir, çözümleyebilir ve araştırmanızı sağlayabilirsiniz.
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: how-to
ms.date: 09/01/2020
ms.author: yelevin
ms.openlocfilehash: e8e44f69965af1987bd5f023644d966b3caf1c77
ms.sourcegitcommit: 59ea8436d7f23bee75e04a84ee6ec24702fb2e61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2020
ms.locfileid: "89505875"
---
# <a name="connect-data-from-azure-ddos-protection"></a>Azure DDoS korumasından verileri bağlama

> [!IMPORTANT]
> Azure Sentinel 'deki Azure DDoS koruma verileri Bağlayıcısı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Dağıtılmış hizmet reddi (DDoS) saldırıları bir uygulamanın kaynaklarını tüketmeye çalışır ve uygulamayı meşru kullanıcılar için kullanılamaz hale getirir. DDoS saldırıları internet üzerinden genel olarak erişilebilen herhangi bir uç noktasını hedefleyebilir. [Azure DDoS koruması](../virtual-network/ddos-protection-overview.md), uygulama tasarımı en iyi uygulamaları ile birlikte, DDoS saldırılarına karşı güçlü bir savunma sağlar. Azure DDoS koruma günlüklerini Azure Sentinel 'e bağlanarak, çalışma kitaplarında günlük verilerini görüntülemenize, özel uyarılar oluşturmak için bu uygulamayı kullanmanıza ve araştırmalarınızı iyileştirebilecek şekilde katabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

- Azure Sentinel çalışma alanında okuma ve yazma izinlerine sahip olmanız gerekir.

- Yapılandırılmış bir [Azure DDoS standart koruma planına](../virtual-network/manage-ddos-protection.md#create-a-ddos-protection-plan)sahip olmanız gerekir.

- [Azure DDoS standart özellikli yapılandırılmış bir sanal ağınız](../virtual-network/manage-ddos-protection.md#enable-ddos-for-a-new-virtual-network)olmalıdır.

## <a name="connect-to-azure-ddos-protection"></a>Azure DDoS korumasına bağlanma
    
1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin.

1. Veri bağlayıcıları galerisinden **Azure DDoS koruması** ' nı seçin ve ardından Önizleme bölmesinde **bağlayıcı sayfasını aç** ' ı seçin.

1. Günlüklerini bağlamak istediğiniz tüm güvenlik duvarları üzerinde **tanılama günlüklerini** etkinleştirin:

    1. **Tanılama ayarlarını aç >** bağlantısını seçin ve listeden BIR **genel IP adresi** kaynağı seçin.

    1. **+ Tanılama ayarı Ekle**' yi seçin.

    1. **Tanılama ayarları** ekranında:
       - **Tanılama ayarı adı** alanına bir ad girin.

       - **Log Analytics gönder** onay kutusunu işaretleyin. Burada iki yeni alan görüntülenir. İlgili **aboneliği** ve **Log Analytics çalışma alanını** (Azure Sentinel 'in bulunduğu yer) seçin.

       - Günlüklerini almak istediğiniz olan kural türlerinin onay kutularını işaretleyin. **Ddosprotectionnotifications**, **DDoSMitigationFlowLogs**ve **DDoSMitigationReports**önerilir.

    1. Ekranın en üstündeki **Kaydet** ' e tıklayın. DDoS korumasını etkinleştirdiğiniz ek güvenlik duvarları (genel IP adresleri) için bu işlemi tekrarlayın.

1. Azure DDoS Koruması uyarıları için Log Analytics ilgili şemayı kullanmak için, **AzureDiagnostics**aratın.

> [!NOTE]
>
> Bu veri bağlayıcısıyla bağlantı durumu göstergeleri (veri bağlayıcıları galerisindeki bir renk şeridi ve veri türü adlarının yanında bulunan bağlantı simgeleri), yalnızca geçen iki haftada bir noktada verilerin alınmış olması halinde *bağlı* (yeşil) olarak görünür. Veri alımı olmadan iki hafta geçtikten sonra bağlayıcının bağlantısı kesilmekte olarak gösterilir. Daha fazla veri geldiğinde *bağlantılı* durum döndürülür.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede Azure DDoS koruma günlüklerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
