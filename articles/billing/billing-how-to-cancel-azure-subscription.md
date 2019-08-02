---
title: Azure aboneliğinizi iptal edin | Microsoft Docs
description: Ücretsiz deneme aboneliği gibi Azure aboneliğinizin nasıl iptal edileceğini açıklar
author: bandersmsft
manager: amberb
tags: billing
ms.assetid: 3051d6b0-179f-4e3a-bda4-3fee7135eac5
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: banders
ms.openlocfilehash: 7756174f01e3fede17bec3e2ac185e89caddc097
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68666442"
---
# <a name="cancel-your-azure-subscription"></a>Azure aboneliğinizi iptal edin

Artık abonelik gerekmiyorsa, Azure portal Azure aboneliğinizi iptal edebilirsiniz. 

Aboneliğinizi iptal etmeden önce:
* Verilerinizi yedekleyin. Örneğin, verileri Azure Storage veya SQL 'de depoluyorsanız bir kopyasını indirin. Bir sanal makineniz varsa, bir görüntüsünü yerel olarak kaydedin.
* Hizmetlerinizi kapatın. [Yönetim portalındaki kaynaklar sayfasına](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources)gidin ve çalışan tüm sanal makineleri, uygulamaları veya diğer hizmetleri **durdurun** .
* Verilerinizi geçirmeyi düşünün. Bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md).
* Tüm kaynakları ve tüm kaynak gruplarını silin. Bir aboneliği iptal etmeden önce bunları silme işlemi gereklidir. Her kaynak grubunun ayrı ayrı silinmesi gerekir. Kaynak grubu silme sırasında, kaynak grubu adını yazarak silme işlemini onaylamanız gerekir.
* İçindeki `AssignableScopes`bu aboneliğe başvuran özel rolleriniz varsa, aboneliği kaldırmak için bu özel rolleri güncelleştirmeniz gerekir. Bir aboneliği iptal ettikten sonra özel bir rolü güncelleştirmeye çalışırsanız bir hata alabilirsiniz. Daha fazla bilgi için bkz. [Azure kaynakları için](../role-based-access-control/custom-roles.md)özel roller ve özel rollerle Ilgili [sorunları giderme](../role-based-access-control/troubleshooting.md#problems-with-custom-roles) .

Ücretli bir Azure destek planını iptal ederseniz abonelik dönemi geri kalanı için faturalandırılırsınız. Daha fazla bilgi için bkz. [Azure destek planları](https://azure.microsoft.com/support/plans/).

## <a name="cancel-subscription-in-the-azure-portal"></a>Azure portal aboneliği iptal et

1. [Azure Portal abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)aboneliğinizi seçin.
2. İptal etmek istediğiniz aboneliği seçin.
3. **Genel bakış**' ı seçin ve **aboneliği iptal et**' i seçin.
    ![Iptal düğmesini gösteren ekran görüntüsü](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)
3. İstemleri izleyin ve iptali tamamlayın.


## <a name="who-can-cancel-a-subscription"></a>Bir aboneliği kimlerin iptal edebilir?

Aşağıdaki tabloda, bir aboneliği iptal etmek için gereken izinler açıklanmaktadır.

|Abonelik türü     |Kimlerin iptal edebilen  |
|---------|---------|
|Azure Web sitesi aracılığıyla Azure 'a kaydolduğunuzda oluşturulan abonelikler. Örneğin, [ücretsiz bir Azure hesabı](https://azure.microsoft.com/offers/ms-azr-0044p/)için kaydolduğunuzda, [Kullandıkça Öde tarifesine](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [Visual Studio abonesi](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)olarak hesaba kaydolabilirsiniz. |  Abonelik için hesap yöneticisi, sahipler ve katkıda bulunanlar  |
|[Microsoft kurumsal anlaşma](https://azure.microsoft.com/pricing/enterprise-agreement/) ve [Kurumsal geliştirme/test](https://azure.microsoft.com/offers/ms-azr-0148p/)     |  Abonelik için hesap sahibi, sahipler ve katkıda bulunanlar       |
|[DevTest Için](https://azure.microsoft.com/offers/ms-azr-0148g/) [Azure planı](https://azure.microsoft.com/offers/ms-azr-0017g/) ve Azure planı     |  Abonelik için sahipler ve katkıda bulunanlar      |


## <a name="what-happens-after-i-cancel-my-subscription"></a>Aboneliğimi iptal etmem durumunda ne olur?

İptal ettikten sonra faturalandırma hemen durdurulur. Ancak, İptalin portalda görünmesi 10 dakikaya kadar sürebilir. Bir fatura döneminin ortasında iptal ederseniz, son faturayı dönem bittikten sonra normal fatura tarihi üzerinden göndereceğiz.

İptal ettikten sonra hizmetleriniz devre dışı bırakılır. Diğer bir deyişle, sanal makineleriniz de serbest bırakılır, geçici IP adresleri serbest bırakılır ve depolama alanı salt okunurdur.

Verilerinize erişmeniz veya fikrinizi değiştirmeniz gerekebilmeniz halinde verilerinizi kalıcı olarak silmeden önce 90 gün bekleyeceğiz. Verileri saklamak için ücret ödemezsiniz. Daha fazla bilgi edinmek için bkz. [Microsoft Güven Merkezi-verilerinizi yönetme](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

## <a name="reactivate-subscription"></a>Aboneliği yeniden etkinleştirme

Aboneliğinizi Kullandıkça Öde tarifesine yanlışlıkla iptal ederseniz, [Hesap merkezinde yeniden etkinleştirebilirsiniz](billing-subscription-become-disable.md).

Aboneliğiniz, Kullandıkça Öde tarifelerine sahip bir abonelik değilse, aboneliğinizi yeniden etkinleştirmek için 90 gün içinde destek ekibiyle iletişime geçin.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).
