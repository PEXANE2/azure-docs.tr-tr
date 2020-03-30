---
title: Azure Bilgi Koruması'nı Azure Sentinel'e bağlayın
description: Azure Bilgi Koruması verilerini Azure Sentinel'de nasıl bağlayabilirsiniz öğrenin.
services: sentinel
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: yelevin
ms.openlocfilehash: 396fd7c4289c9d02d451b26f5fb6299acd31e2a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588561"
---
# <a name="connect-data-from-azure-information-protection"></a>Azure Bilgi Koruması'ndan verileri bağlama

> [!IMPORTANT]
> Azure Sentinel'deki Azure Bilgi Koruması veri bağlayıcısı şu anda genel önizlemededir.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Azure Bilgi Koruması veri bağlayıcısını yapılandırarak günlüğe kaydetme bilgilerini [Azure Bilgi Koruması'ndan](https://azure.microsoft.com/services/information-protection/) Azure Sentinel'e aktarabilirsiniz. Azure Bilgi Koruması, ister bulutta ister şirket içinde depolanmış olsun, hassas verilerinizi kontrol etmemenize ve güvenli hale almanıza yardımcı olur.

[Azure Bilgi Koruması için merkezi raporlama,](https://docs.microsoft.com/azure/information-protection/reports-aip) bu hizmetten günlük bilgilerinin Azure Sentinel için seçtiğiniz aynı Log Analytics çalışma alanında depolanması için zaten yapılandırılmışsa, bu veri bağlayıcısının yapılandırmasını atlayabilirsiniz. Azure Bilgi Koruması'ndan günlük bilgileri Azure Sentinel'de zaten kullanılabilir.

Ancak, Azure Bilgi Koruması'ndan günlüğe kaydetme bilgileri, Şu anda Azure Sentinel için seçtiğiniz çalışma alanından farklı bir Log Analytics çalışma alanına gidiyorsa, aşağıdakilerden birini yapın:

- Azure Sentinel'de seçilen çalışma alanını değiştirin.

- Bu veri bağlayıcısını yapılandırarak yapabileceğiniz Azure Bilgi Koruması için çalışma alanını değiştirin.
    
    Çalışma alanını değiştirirseniz, Azure Bilgi Koruması için yeni raporlama verileri artık Azure Sentinel için kullandığınız çalışma alanında depolanır ve geçmiş veriler Azure Sentinel'de kullanılamaz. Ayrıca, önceki çalışma alanı özel sorgular, uyarılar veya REST API'leri için yapılandırıldıysa, bunları Azure Bilgi Koruması için kullanmaya devam etmek istiyorsanız Azure Sentinel çalışma alanı için yeniden yapılandırılmalıdır. Azure Bilgi Koruması kullanan istemciler ve hizmetler için yeniden yapılandırma gerekmez.

## <a name="prerequisites"></a>Ön koşullar

- Kiracınız için aşağıdaki Azure REKLAM yöneticisi rollerinden biri: 
    - Azure Bilgi Koruma yöneticisi
    - Güvenlik yöneticisi
    - Uyumluluk yöneticisi
    - Uyumluluk veri yöneticisi
    - Genel yönetici
    
    > [!NOTE]
    > Kiracınız [birleşik etiketleme platformundaysa](/information-protection/faqs#how-can-i-determine-if-my-tenant-is-on-the-unified-labeling-platform)Azure Bilgi Koruması yöneticisi rolünü kullanamazsınız.
    
    Bu yönetici rolleri yalnızca Azure Bilgi Koruması bağlayıcısını yapılandırmak için gereklidir ve Azure Sentinel Azure Bilgi Koruması'na bağlandığında gerekli değildir.

- Azure Sentinel ve Azure Bilgi Koruması için kullandığınız Log Analytics çalışma alanına okuma ve yazma izinleri.

- Azure Bilgi Koruması Azure portalına eklendi. Bu adımda yardıma ihtiyacınız varsa, [Azure portalına Azure Bilgi Koruması Ekle'ye](https://docs.microsoft.com/azure/information-protection/quickstart-viewpolicy#add-azure-information-protection-to-the-azure-portal)bakın.

## <a name="connect-to-azure-information-protection"></a>Azure Bilgi Koruması'na bağlanın

Azure Bilgi Koruması için Bir Günlük Analizi çalışma alanı yapılandırmadıysanız veya Azure Bilgi Koruması günlüğe kaydetme bilgilerini depolayan çalışma alanını değiştirmeniz gerekiyorsa aşağıdaki yönergeleri kullanın.

1. Azure Sentinel'de **Veri bağlayıcılarını**ve ardından **Azure Bilgi Koruması'nı (Önizleme)** seçin.

2. **Bağlayıcıyı Aç'ı**seçin.

3. Yapılandırma **analitiği (Önizleme)** bıtır'da, Azure Sentinel için şu anda kullanmakta olduğunuz çalışma alanını seçin. Farklı bir çalışma alanı seçerseniz, Azure Bilgi Koruması'ndan gelen raporlama verileri Azure Sentinel tarafından kullanılamaz.

4. Bir çalışma alanı seçtiğinizde, **Tamam'ı** seçin ve bağlayıcı **DURUM** artık **Bağlı**olarak değiştirilmelidir.

5. Azure Bilgi Koruması'ndan gelen raporlama verileri, seçili çalışma alanı içinde **InformationProtectionLogs_CL** tablosunda depolanır. 
    
    Bu raporlama verileri için Azure Monitor'daki ilgili şemayı kullanmak için **InformationProtectionEvents'i**arayın. Bu olay işlevleri hakkında bilgi için Azure Bilgi Koruması belgelerinden olay işlevleri için [Dostça şema başvurusuna](https://docs.microsoft.com/azure/information-protection/reports-aip#friendly-schema-reference-for-event-functions) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Bilgi Koruması'nı Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
