---
title: Azure abonelikleri için faturaya göre ödeme yapın
description: Azure aboneliklerinin faturaya göre nasıl ödeme yapılacağını açıklar.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/23/2019
ms.author: banders
ms.openlocfilehash: 9e4e05acd88e9b0f0c17d4dd4caf5eb5a883d63d
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70012587"
---
# <a name="pay-for-your-azure-subscription-by-invoice"></a>Azure aboneliğiniz için faturaya göre ödeme yapın

Faturaya göre öde ' ye geçerseniz, fatura tarihinin 30 gün içinde Çek/tel aktarımı ile faturalandırıldığınız anlamına gelir. Azure aboneliğinizi faturayla ödemeye uygun duruma gelmek için, Azure desteğine bir istek gönderin. İsteğiniz onaylandıktan sonra, [Azure Portal](https://portal.azure.com)fatura ödeme (Çek/tel aktarımı) seçeneğine geçebilirsiniz.

> [!IMPORTANT]
> * Fatura ödemesi (Çek/tel aktarımı) yalnızca iş hesaplarında kullanılabilir.
> * Fatura ödemelerinde geçiş yapmadan önce tüm bekleyen ücretleri ödeyin.
> * Şu anda, fatura ödemesi Çin 'de Global Azure için desteklenmez.

## <a name="request-to-pay-by-invoice"></a>Faturaya göre ödeme isteği

1. [Azure Portal](https://portal.azure.com/) oturum açın. **Yardım ve destek** > **Yeni destek isteği ' ni**seçin.

    ![Yardım ve destek bağlantısı](./media/billing-how-to-pay-by-invoice/help-and-support.png)

2. **Sorun türü**olarak **faturalandırma** ' i seçin. *Sorun türü* , destek isteği kategorisidir. Faturaya göre ödeme yapmak istediğiniz aboneliği seçin, bir destek planı seçin ve ardından **İleri**' yi seçin.

3. **Sorun türü**olarak **ödeme** ' yi seçin. *Sorun türü* , destek isteği alt kategorisidir.

4. **Sorun alt türü** olarak **faturaya göre ödeme yap** ' ı seçin

5. **Ayrıntılar** kutusuna aşağıdaki bilgileri girin ve ardından **İleri**' yi seçin.

         New or existing customer:
         If existing, current payment method:
         Order ID (requesting for invoice option):
         Account Admins Live ID (or Org ID) (should be company domain):
         Commerce Account ID:
         Company Name (as registered under VAT or Government Website):
         Company Address (as registered under VAT or Government Website):
         Company Website:
         Country:
         TAX ID/ VAT ID:
         Company Established on (Year):
         Any prior business with Microsoft:
         Contact Name:
         Contact Phone:
         Contact Email:
         Justification on why you prefer Invoice option over credit card:

        For cores increase, provide the following additional information:

         (Old quota) Existing Cores:
         (New quota) Requested cores:
         Specific region & series of Subscription:

    - **Şirket adı** ve **Şirket adresi** , Azure hesabı için verdiğiniz bilgilerle eşleşmelidir. Bilgileri görüntülemek veya güncelleştirmek için bkz. [Azure hesap profili bilgilerinizi değiştirme](billing-how-to-change-azure-account-profile.md).
    - Kredi sınırının onaylanabilmesi için faturalandırma iletişim bilgilerinizi Azure portal ekleyin. İletişim ayrıntıları, şirketin hesaplarının borç veya finans departmanıyla ilişkili olmalıdır. Faturalandırma iletişim bilgilerini güncelleştirmek için [Azure Hesap Merkezi](https://account.azure.com/Profile)adresine gidin.

6. İletişim bilgilerinizi ve tercih edilen iletişim yöntemini seçip **Oluştur**'a tıklayın.

İhtiyaç duyduğunuz kredi miktarı nedeniyle bir kredi denetimi çalıştırdığımızda size bir kredi denetimi uygulaması göndereceğiz.

## <a name="switch-to-invoice-pay-checkwire-transfer"></a>Fatura ödeme geç (Çek/tel aktarımı)

Faturaya göre ödeme yapmayı onayladıktan sonra, Azure portal fatura ödeme (Çek/tel aktarımı) seçeneğine geçebilirsiniz.

Microsoft Online Services Program hesabınız varsa, Azure aboneliğinizi çek/havale yoluyla ödemeye geçirebilirsiniz. Bir Microsoft Müşteri anlaşmasıyla faturalandırma profilinizi denetim/tel aktarımına geçirebilirsiniz. [Hesap türünü nasıl denetleyeceğinizi öğrenin](#check-access-to-a-microsoft-customer-agreement).

### <a name="switch-azure-subscription-to-checkwire-transfer"></a>Azure aboneliğini denetim/tel aktarımına geçir

Azure aboneliğinizi fatura ödemenize (Çek/tel aktarımı) geçirmek için aşağıdaki adımları izleyin. **Fatura ödemenize (Çek/tel aktarımı) geçtiğinizde, kredi kartına geri**dönemezsiniz.

1. [Azure Portal](https://portal.azure.com) hesap yöneticisi olarak oturum açın.
1. **Maliyet yönetimi + faturalandırma**için arama yapın.

    ![Aramayı gösteren ekran görüntüsü](./media/billing-how-to-pay-by-invoice/search.png)

1. Faturalı ödemeye geçirmek istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri**'ni seçin.
1. Komut çubuğunda, **faturaya göre öde** düğmesine tıklayın.

    ![Faturaya göre ödeme düğmesini gösteren ekran görüntüsü](./media/billing-how-to-pay-by-invoice/pay-by-invoice.png)

### <a name="switch-billing-profile-to-checkwire-transfer"></a>Faturalandırma profilini denetim/tel aktarımına geçir

Bir faturalandırma profilini denetim/tel aktarım için değiştirmek üzere aşağıdaki adımları izleyin. Yalnızca Azure 'a kaydolan kişi, bir faturalandırma profilinin varsayılan ödeme yöntemini değiştirebilir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Arama **maliyet Yönetimi + faturalandırma**.
1. Soldaki menüde **faturalandırma profilleri**' ne tıklayın.

    ![menüdeki faturalama profilini gösteren ekran görüntüsü](./media/billing-how-to-pay-by-invoice/billing-profile.png)

1. Bir faturalandırma profili seçin.
1. Soldaki menüden **ödeme yöntemleri**' ni seçin.

   ![Menüdeki ödeme yöntemlerini gösteren ekran görüntüsü](./media/billing-how-to-pay-by-invoice/billing-profile-payment-methods.png)

1. Çek/tel aktarımı ile ödeme yapmayı uygun olduğunuzu belirten mavi başlığa tıklayın.

    ![Denetim/tel 'ya geçiş yapmak için mavi başlık gösteren ekran görüntüsü](./media/billing-how-to-pay-by-invoice/customer-led-switch-to-invoice.png)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesine erişimi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- Gerekirse faturalandırma iletişim bilgilerinizi [Azure Hesap Merkezi](https://account.azure.com/Profile)güncelleştirin.
