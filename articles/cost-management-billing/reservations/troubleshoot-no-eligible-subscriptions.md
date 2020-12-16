---
title: Azure portaldaki Koşullara uygun abonelik yok hata iletisiyle ilgili sorunları giderme
description: Bu makale, Azure portalda rezervasyon satın almaya çalıştığınızda karşınıza çıkan Koşullara uygun abonelik yok hata iletisiyle ilgili sorunları gidermenize yardımcı olur.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: reservations
ms.author: banders
ms.reviewer: yashar
ms.topic: troubleshooting
ms.date: 12/15/2020
ms.openlocfilehash: ad85bd278b5dff1532f218acc0b8e88515d96070
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97561214"
---
# <a name="troubleshoot-no-eligible-subscriptions"></a>Koşullara uygun abonelik yok hata iletisiyle ilgili sorunları giderme

Bu makale, Azure portalda rezervasyon satın almaya çalıştığınızda karşınıza çıkan *Koşullara uygun abonelik yok* hata iletisiyle ilgili sorunları gidermenize yardımcı olur.

## <a name="symptoms"></a>Belirtiler

1. [Azure portalda](https://portal.azure.com) oturum açın ve **Rezervasyonlar**’a gidin.
1. **Ekle** seçeneğini belirleyin ve sonra bir hizmet seçin.
1. Aşağıdaki hata iletisini alırsınız:
   ```
    No eligible subscriptions
    
    You do not have any eligible subscriptions to purchase reservations. To purchase a reservation, you should have owner or reservation purchaser permission on at least one subscription of the following type: Pay-as-you-go, CSP, Microsoft Enterprise or Microsoft Customer Agreement.
    ```
1. **Satın almak istediğiniz ürünü seçin** alanında **Faturalama aboneliği** listesini genişleterek belirli bir aboneliğin ayrılmış örnek satın alma koşullarını sağlamama nedenini öğrenin. Aşağıdaki görüntüde, rezervasyon satın alamama nedeni örnekleri gösterilmektedir.  
    :::image type="content" source="./media/troubleshoot-no-eligible-subscriptions/select-product-to-purchase.png" alt-text="Rezervasyonun satın alınamama nedeninin gösterildiği örnek" lightbox="./media/troubleshoot-no-eligible-subscriptions/select-product-to-purchase.png" :::

## <a name="cause"></a>Nedeni

Azure ayrılmış örneği satın almak için aşağıdaki gereksinimleri karşılayan en az bir aboneliğe sahip olmanız gerekir:

- Abonelik desteklenen bir teklif türü olmalıdır. Desteklenen teklif türleri şunlardır: Kullandıkça öde, Bulut Çözümü Sağlayıcısı (CSP), Microsoft Azure Kurumsal veya Microsoft Müşteri Sözleşmesi.
- Abonelikte sahip veya rezervasyon satın almacı olmalısınız.

Gereksinimleri karşılayan bir aboneliğiniz yoksa `No eligible subscriptions` hatasını alırsınız.

### <a name="cause-1"></a>1\. Neden

Abonelik desteklenen bir teklif türü olmalıdır. Desteklenen teklif türleri şunlardır: Kullandıkça öde, CSP, Microsoft Azure Kurumsal veya Microsoft Müşteri Sözleşmesi. Aboneliğinizin türü desteklenen bir tür değil. Rezervasyonları desteklemeyen bir teklif türüne sahip abonelik seçtiğinizde aşağıdaki hatayı alırsınız.

```
Subscription not eligible for purchase

This subscription is not eligible for reservation benefit an cannot be used to purchase a reservation.
```

:::image type="content" source="./media/troubleshoot-no-eligible-subscriptions/subscription-not-eligible.png" alt-text="Abonelik satın alma koşullarına uygun değil hata iletisinin gösterildiği örnek" :::

### <a name="cause-2"></a>2\. Neden

Abonelikte sahip veya rezervasyon satın almacı olmalısınız. Yeterli izinleriniz yoksa aşağıdaki hatayı görürsünüz.

```
You do not have owner or reservation purchaser access on the subscription

You can only purchase reservations using subscriptions on which you have owner or reservation purchaser access.
```

## <a name="solution"></a>Çözüm

- Geçerli teklifiniz rezervasyonları desteklemiyorsa yeni bir Azure aboneliği oluşturmanız gerekir.
- Mevcut bir rezervasyona erişiminiz yoksa geçerli sahibinden erişim alabilirsiniz.

### <a name="solution-1"></a>1\. Çözüm

Rezervasyon satın almak için, rezervasyonları destekleyen yeni bir Azure aboneliği oluşturmanız gerekir.

- Azure Ücretsiz denemesine sahipseniz [aboneliğinizi yükseltebilirsiniz](../manage/upgrade-azure-subscription.md).
- [Kullandıkça öde fiyatlarına](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/) sahip yeni bir Azure aboneliği oluşturabilirsiniz.
- [Microsoft Müşteri Sözleşmesi](https://azure.microsoft.com/pricing/purchase-options/microsoft-customer-agreement/)’ne kaydolun ve yeni bir abonelik oluşturun.
- [CSP](https://www.microsoft.com/solution-providers/home) ile yeni bir abonelik satın alın ve yeni bir abonelik oluşturun.

### <a name="solution-2"></a>2\. Çözüm

Rezervasyona sahip erişimi almak için şunlardan birine erişim almanız gerekir:

- Rezervasyonun satın alındığı rezervasyon siparişi
- Rezervasyonun kendisi

Geçerli rezervasyon siparişinin sahibi veya rezervasyon sahibi aşağıdaki adımları kullanarak size temsilci erişimi verebilir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Tüm Hizmetler** > **Rezervasyon**'u seçerek erişim sahibi olduğunuz rezervasyonları listeleyin.
1. Diğer kullanıcılara erişim vermek istediğiniz rezervasyonu seçin.
1. **Erişim denetimi (IAM)** öğesini seçin.
1. **Rol ataması ekle** > **Rol** > **Sahip**'i seçin. Sınırlı erişim vermek istiyorsanız farklı bir rol seçebilirsiniz.
1. Sahip olarak eklemek istediğiniz kullanıcının e-posta adresini yazın.
1. Kullanıcıyı ve ardından **Kaydet**'i seçin.

Daha fazla bilgi için bkz. [Rezervasyonu yönetebilecek kullanıcıları ekleme veya değiştirme](manage-reserved-vm-instance.md#who-can-manage-a-reservation-by-default).

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyon sahibinin size erişim vermesine ihtiyaç duyuyorsanız [Rezervasyonu yönetebilecek kullanıcıları ekleme veya değiştirme](manage-reserved-vm-instance.md#who-can-manage-a-reservation-by-default) başlıklı makaleyi gözden geçirin.
