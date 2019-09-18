---
title: Ödeme hesabınız için ek bir Azure aboneliği oluşturma
description: Azure portalında nasıl yeni bir Azure aboneliği ekleneceğini öğrenin.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 558410d980d261780f7287d1e27ed704b356fc2b
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "67490923"
---
# <a name="create-an-additional-azure-subscription-for-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi için ek Azure aboneliği oluşturma

Geliştirme, test ve güvenlik için ayrı ortamlar ayarlamak veya uyumluluk nedeniyle verileri yalıtmak için ödeme hesabınıza ilişkin ek abonelikler oluşturun.

Bu makale, Microsoft Müşteri Sözleşmesi’ne ilişkin ödeme hesabı için geçerlidir. [Microsoft Müşteri Sözleşmesi’ne erişiminiz olup olmadığını denetleyin](#check-access). Diğer ödeme hesabı türleri için abonelikler oluşturmak istiyorsanız bkz. [Azure portalında ek abonelik oluşturma](billing-create-subscription.md).

Abonelik oluşturmak için **fatura bölümü sahibi**, **fatura bölümü katkıda bulunanı** veya **Azure aboneliği oluşturucusu** olmanız gerekir. Daha fazla bilgi için bkz. [Abonelik faturalama rolleri ve görevleri](billing-understand-mca-roles.md#subscription-billing-roles-and-tasks). Diğer kullanıcılara ödeme hesabınız için Azure abonelikleri oluşturma izni sağlamak için bkz. [Diğer kullanıcılara Azure abonelikleri oluşturma izni sağlama](#give-others-permission).

## <a name="create-a-subscription"></a>Abonelik oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Abonelikler** araması yapın.

   ![Portalda abonelikler aramasını gösteren ekran görüntüsü](./media/billing-mca-create-subscription/billing-search-subscriptions.png)

3. **Ekle**’yi seçin

4. Birden çok ödeme hesabına erişiminiz varsa, Müşteri sözleşmeniz için ödeme hesabını seçin.

   ![Abonelik oluştur sayfasını gösteren ekran görüntüsü](./media/billing-mca-create-subscription/billing-mca-create-azure-subscription.png)

5. Bir faturalama profili seçin. Aboneliğinizin ücretleri, seçili faturalama profiline faturalanır. Yalnızca bir faturalama profiline erişiminiz varsa seçim gri olur.

6. Bir fatura bölümü seçin. Aboneliğinizin ücretleri, faturalama profilinin faturasının bu bölümüne faturalanır. Yalnızca bir fatura bölümüne erişiminiz varsa seçim gri olur.

7. Abonelik için bir plan seçin. Geliştirme veya test iş yükleri için bu aboneliği kullanmayı planlıyorsanız **DevTest için Microsoft Azure Planı**’nı seçin; aksi takdirde **Microsoft Azure Planı**‘nı kullanın. Yalnızca bir plana erişiminiz varsa seçim gri olur.

8. Abonelik için bir ad girin. Bu ad, Azure portalında aboneliği kolayca belirlemenize yardımcı olur.

9. **Oluştur**’u seçin.

## <a name="give-others-permission"></a>Diğer kullanıcılara izin verme

Azure aboneliği oluşturma izni vermek için bir fatura bölümünde Azure aboneliği oluşturucusu olarak kullanıcıları ekleyin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda abonelikler aramasını gösteren ekran görüntüsü](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Fatura bölümüne gidin. Erişiminize bağlı olarak, bir faturalama hesabı veya faturalama profili seçmeniz gerekebilir. Ödeme hesabından veya faturalama profilinden **Fatura bölümleri**’ni ve sonra listeden bir fatura bölümünü seçin. Kullanıcılar tarafından oluşturulacak abonelikler bu fatura bölümüne faturalandırılır.
   
   ![Fatura bölümlerinin seçilmesini gösteren ekran görüntüsü](./media/billing-mca-create-subscription/mca-select-invoice-sections.png)        

4. Soldan **Erişim Yönetimi (IAM)** öğesini seçin.

5. Sayfanın üst kısmında **Ekle**'yi seçin.

6. Rol için **Azure aboneliği oluşturucusu**’nu seçin.

7. Erişim izni vermek istediğiniz kullanıcının e-posta adresini girin.

8. **Kaydet**’i seçin.

## <a name="check-access"></a>Erişimi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Yerleşik rolleri kullanarak diğer kullanıcılara Azure kaynakları oluşturma izni verme](../role-based-access-control/built-in-roles.md#built-in-role-descriptions)
- [Windows sanal makinesi oluşturma](../virtual-machines/windows/quick-create-portal.md)
- [Linux sanal makinesi oluşturma](../virtual-machines/linux/quick-create-portal.md)
- [Kaynak organizasyonu ve yönetimi için yönetim grupları oluşturma](../governance/management-groups/create.md?toc=/azure/billing/TOC.json)
