---
title: Azure Defender 'a yükseltme-Azure Güvenlik Merkezi
description: Bu hızlı başlangıçta ek güvenlik için Güvenlik Merkezi 'nin Azure Defender 'a nasıl yükseltileceği gösterilmektedir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: b583e3d86ba193a92080f2da9cc0b16d07c5a993
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91447169"
---
# <a name="quickstart-setting-up-azure-security-center"></a>Hızlı başlangıç: Azure Güvenlik Merkezi 'Ni ayarlama

Azure Güvenlik Merkezi, hibrit bulut iş yüklerinizde birleşik güvenlik yönetimi ve tehdit koruması sağlar. Ücretsiz özellikler yalnızca Azure kaynaklarınız için sınırlı güvenlik sunarak Azure Defender 'ın bu özellikleri şirket içi ve diğer bulutlara genişletmelerini sağlar. Azure Defender, güvenlik açıklarını bulmanıza ve düzeltmenize, kötü amaçlı etkinlikleri engellemek, analiz ve zekası kullanarak tehditleri tespit etmek ve saldırı altında hızla yanıt vermek için erişim ve uygulama denetimleri uygulamanıza yardımcı olur. Azure Defender 'ı ücretsiz olarak deneyebilirsiniz. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Bu makalede, güvenlik açıklarını ve tehditleri izlemek için, ek güvenlik için Azure Defender 'a yükseltirsiniz ve makinelerinize Log Analytics aracısını yüklersiniz.

## <a name="prerequisites"></a>Önkoşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Azure Defender 'ı bir abonelikte etkinleştirmek için abonelik sahibi, abonelik katılımcısı veya güvenlik yöneticisi rolüne sahip olmanız gerekir.


## <a name="open-security-center-for-the-first-time"></a>Güvenlik Merkezi 'ni ilk kez açın

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.

1. Portalın menüsünde **Güvenlik Merkezi**' ni seçin. 

    Güvenlik Merkezi 'nin genel bakış sayfası açılır.

    :::image type="content" source="./media/security-center-get-started/overview.png" alt-text="Güvenlik Merkezi 'ne Genel Bakış Panosu" lightbox="./media/security-center-get-started/overview.png":::

**Güvenlik Merkezi - Genel Bakış**, hibrit bulut iş yüklerinizin güvenlik durumuna ilişkin birleştirilmiş bir görünüm sağlayarak, iş yüklerinizin güvenliğini tespit edip değerlendirmenize ve riskleri tanımlayıp en aza indirmenize olanak tanır. Güvenlik Merkezi, ücretsiz olarak, siz veya başka bir abonelik kullanıcısı tarafından daha önce eklendi Azure aboneliklerinizden herhangi birini kullanmanıza izin vermez.

**Abonelikler menü öğesini** seçerek Aboneliklerin listesini görüntüleyebilir ve filtreleyebilirsiniz. Güvenlik Merkezi, seçilen aboneliklerin güvenlik duruşunu yansıtacak şekilde ekranı ayarlar. 

Güvenlik Merkezi’ni başlattıktan sonraki ilk birkaç dakika içinde şunları görebilirsiniz:

- Bağlı kaynaklarınızın güvenliğini artırmanın yolları için **öneriler** .
- Artık Güvenlik Merkezi tarafından değerlendirilmekte olan kaynaklarınızın envanterini ve bunların güvenlik duruşunu.

Güvenlik Merkezi 'Nden tam olarak yararlanabilmek için, Azure Defender 'ı etkinleştirmek ve Log Analytics aracısını yüklemek üzere aşağıdaki adımları gerçekleştirmeniz gerekir.


## <a name="enable-azure-defender"></a>Azure Defender 'ı etkinleştir

Güvenlik Merkezi hızlı başlangıç ve öğreticiler amacına yönelik olarak Azure Defender 'ı etkinleştirmeniz gerekir. Ücretsiz 30 günlük deneme sürümü mevcuttur. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/). 

1. Güvenlik Merkezi 'nin kenar çubuğundan **Başlarken**' i seçin.

    :::image type="content" source="./media/security-center-get-started/get-started-upgrade-tab.png" alt-text="Güvenlik Merkezi 'ne Genel Bakış Panosu"::: 

    **Yükseltme** sekmesi, ekleme için uygun olan abonelikleri ve çalışma alanlarını listeler.

1. **Azure Defender 'ı etkinleştirmek için çalışma alanlarını seçin** listesinde, Yükseltilecek çalışma alanlarını seçin.
   - Deneme yanılma uygun olmayan abonelikler ve çalışma alanları ' nı seçerseniz, bir sonraki adım bunları yükseltir ve ücretler başlatılır.
   - Ücretsiz deneme için uygun olan bir çalışma alanı seçerseniz, bir sonraki adım bir denemeye başlar.
1. Azure Defender 'ı etkinleştirmek için **Yükselt** ' i seçin.

## <a name="enable-automatic-data-collection"></a>Otomatik veri toplamayı etkinleştir
Güvenlik Merkezi, güvenlik açıklarını ve tehditleri izlemek için makinelerinizden veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırma ve olay günlüklerini okuyan ve analiz için verileri çalışma alanınıza kopyalayan Log Analytics Aracı kullanılarak toplanır. Varsayılan olarak Güvenlik Merkezi sizin için yeni bir çalışma alanı oluşturur.

Otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi, desteklenen tüm makinelere ve oluşturulan tüm yeni makinelere Log Analytics aracısını yükleme. Otomatik sağlama önemle önerilir.

Log Analytics aracısının otomatik sağlamasını etkinleştirmek için:

1. Güvenlik Merkezi 'nin menüsünde **fiyatlandırma & ayarları**' nı seçin.
1. Uygun aboneliği seçin.
1. **Veri toplama** sayfasında, **otomatik sağlamayı** **Açık**olarak ayarlayın.
1. **Kaydet**’i seçin.

    :::image type="content" source="./media/security-center-enable-data-collection/enable-automatic-provisioning.png" alt-text="Güvenlik Merkezi 'ne Genel Bakış Panosu":::

>[!TIP]
> Bir çalışma alanının sağlanması gerekiyorsa, aracı yüklemesi 25 dakikaya kadar sürebilir.

Makinelerinize dağıtılan Aracı, Güvenlik Merkezi, sistem güncelleştirme durumu, işletim sistemi güvenlik yapılandırması, uç nokta koruması ve ek güvenlik uyarıları oluşturma ile ilgili ek öneriler sağlayabilir.

>[!NOTE]
> Otomatik sağlamanın **kapalı** olarak ayarlanması, aracısının zaten sağlandığı Azure vm 'lerinden Log Analytics aracısını kaldırmaz. Otomatik sağlamanın devre dışı bırakılması, kaynaklarınızın güvenliğinin izlenmesini kısıtlar.



## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta Azure Defender 'ı etkinleştirmiş ve karma bulut iş yükleriniz genelinde Birleşik güvenlik yönetimi ve tehdit koruması için Log Analytics Aracısı sağlamış olursunuz. Güvenlik Merkezi’ni kullanma hakkında daha fazla bilgi edinmek için şirket içi ortamda ve diğer bulutlarda bulunan Windows bilgisayarları eklemeye ilişkin hızlı başlangıca geçin.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure dışı makineler ekleme](quickstart-onboard-machines.md)

Bulut harcamalarınızı iyileştirmek ve kaydetmek istiyor musunuz?

> [!div class="nextstepaction"]
> [Maliyet yönetimi ile maliyetleri çözümlemeye başlayın](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/get-started.png
[5]: ./media/security-center-get-started/pricing.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
[9]: ./media/security-center-get-started/select-subscription.png
