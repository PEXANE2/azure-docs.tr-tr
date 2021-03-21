---
title: Azure Defender 'a yükseltme-Azure Güvenlik Merkezi
description: Bu hızlı başlangıçta ek güvenlik için Güvenlik Merkezi 'nin Azure Defender 'a nasıl yükseltileceği gösterilmektedir.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: quickstart
ms.date: 02/17/2021
ms.author: memildin
ms.openlocfilehash: 2f37a59d5db3883754b602b2b2525e07b57206b7
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102099361"
---
# <a name="quickstart-set-up-azure-security-center"></a>Hızlı başlangıç: Azure Güvenlik Merkezi 'Ni ayarlama

Azure Güvenlik Merkezi, hibrit ve çok bulut iş yükleriniz arasında Birleşik güvenlik yönetimi ve tehdit koruması sağlar. Ücretsiz özellikler yalnızca Azure kaynaklarınız için sınırlı güvenlik sunarak Azure Defender 'ın bu özellikleri şirket içi ve diğer bulutlara genişletmelerini sağlar. Azure Defender, güvenlik açıklarını bulmanıza ve düzeltmenize, kötü amaçlı etkinlikleri engellemek, analiz ve zekası kullanarak tehditleri tespit etmek ve saldırı altında hızla yanıt vermek için erişim ve uygulama denetimleri uygulamanıza yardımcı olur. Azure Defender 'ı ücretsiz olarak deneyebilirsiniz. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Bu hızlı başlangıç bölümünde, Azure Güvenlik Merkezi ve Azure Defender 'ı etkinleştirmek için önerilen tüm adımlarda size yol gösterilir. Tüm hızlı başlangıç adımlarını tamamladıktan sonra şunları yapabilirsiniz:

> [!div class="checklist"]
> * Güvenlik Merkezi, Azure aboneliklerinizde etkinleştirildi
> * Azure Defender, Azure aboneliklerinizde etkinleştirildi
> * Otomatik veri toplama kurulumu
> * Güvenlik uyarıları için ayarlanan e-posta bildirimleri
> * Azure 'a bağlı karma ve çoklu bulut makineleriniz

## <a name="prerequisites"></a>Önkoşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Azure Defender 'ı bir abonelikte etkinleştirmek için abonelik sahibi, abonelik katılımcısı veya güvenlik yöneticisi rolüne sahip olmanız gerekir.

## <a name="enable-security-center-on-your-azure-subscription"></a>Azure aboneliğinizde Güvenlik Merkezi 'ni etkinleştirme

> [!TIP]
> Yönetim grubu içindeki tüm aboneliklerde Güvenlik Merkezi 'ni etkinleştirmek için, bkz. [birden çok Azure aboneliği üzerinde güvenlik merkezini etkinleştirme](onboard-management-group.md).

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.

1. Portalın menüsünde **Güvenlik Merkezi**' ni seçin. 

    Güvenlik Merkezi 'nin genel bakış sayfası açılır.

    :::image type="content" source="./media/security-center-get-started/overview.png" alt-text="Güvenlik Merkezi 'ne Genel Bakış Panosu" lightbox="./media/security-center-get-started/overview.png":::

    **Güvenlik Merkezi - Genel Bakış**, hibrit bulut iş yüklerinizin güvenlik durumuna ilişkin birleştirilmiş bir görünüm sağlayarak, iş yüklerinizin güvenliğini tespit edip değerlendirmenize ve riskleri tanımlayıp en aza indirmenize olanak tanır. Güvenlik Merkezi, ücretsiz olarak, siz veya başka bir abonelik kullanıcısı tarafından daha önce eklendi Azure aboneliklerinizden herhangi birini kullanmanıza izin vermez.

**Abonelikler menü öğesini** seçerek Aboneliklerin listesini görüntüleyebilir ve filtreleyebilirsiniz. Güvenlik Merkezi, seçilen aboneliklerin güvenlik duruşunu yansıtacak şekilde ekranı ayarlar. 

Güvenlik Merkezi 'nin ilk kez başlatılması sırasında geçen dakikalar içinde şu şekilde karşılaşabilirsiniz:

- Bağlı kaynaklarınızın güvenliğini artırmanın yolları için **öneriler** .
- Artık Güvenlik Merkezi tarafından değerlendirilmekte olan kaynaklarınızın envanterini ve bunların güvenlik duruşunu.

Güvenlik Merkezi 'Nden tam olarak yararlanabilmek için hızlı başlangıç bölümünün sonraki adımlarıyla devam edin.



## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta Azure Güvenlik Merkezi 'Ni etkinleştirmiş olursunuz. Sonraki adım, hibrit bulut iş yükleriniz genelinde birleştirilmiş güvenlik yönetimi ve tehdit koruması için Azure Defender 'ı etkinleştirmektir.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure Defender 'ı etkinleştirme](enable-azure-defender.md)