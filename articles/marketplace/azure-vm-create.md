---
title: Azure Marketi 'nde bir sanal makine teklifi oluşturun.
description: Microsoft ticari marketi 'nde bir sanal makine teklifi oluşturmayı öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 10/20/2020
ms.openlocfilehash: 5964292d7037465456efe22283a5552a2bfc7ad8
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92331269"
---
# <a name="how-to-create-a-virtual-machine-offer-on-azure-marketplace"></a>Azure Marketi 'nde sanal makine teklifi oluşturma

Bu makalede, [Azure Market](https://azuremarketplace.microsoft.com/)Için bir Azure sanal makine teklifinin nasıl oluşturulacağı açıklanır. Bir işletim sistemi, bir sanal sabit disk (VHD) ve en çok 16 veri diski içeren Windows tabanlı ve Linux tabanlı sanal makinelere yöneliktir.

Başlamadan önce [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](partner-center-portal/create-account.md). Hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

## <a name="before-you-begin"></a>Başlamadan önce

Henüz yapmadıysanız, [bir sanal makine teklifi planı](marketplace-virtual-machines.md)' nı gözden geçirin. Sanal makineniz için teknik gereksinimleri açıklayacak ve teklifinizi oluştururken ihtiyaç duyacağınız bilgileri ve varlıkları listelecektir. 

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
2. Sol bölmede, **ticari Market**  >  **genel bakış**' ı seçin.
3. **Genel bakış** sayfasında, **yeni teklif**  >  **Azure sanal makinesi**' ni seçin.

    ![Sol bölme menü seçeneklerini ve "yeni teklif" düğmesini gösteren ekran görüntüsü.](./media/create-vm/new-offer-azure-virtual-machine.png)

> [!NOTE]
> Teklifiniz yayımlandıktan sonra, Iş Ortağı Merkezi 'nde üzerinde yaptığınız herhangi bir düzenleme, yalnızca teklifi yeniden yayımladığınızda Azure Marketi 'nde görünür. Bir teklifi üzerinde değişiklik yaptıktan sonra her zaman yeniden yayımlamayı unutmayın.

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, Azure Marketi teklifinin Web adresindeki müşterilere ve varsa Azure PowerShell ve Azure CLı 'ye yönelik olarak görünür.
- Yalnızca küçük harfleri ve rakamları kullanın. KIMLIK, kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, **Test-teklif-1**girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1` .
- Teklif KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı**girin. Teklif diğer adı, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

- Bu ad Azure Marketi 'nde kullanılmaz. Teklif adından ve müşterilere gösterilen diğer değerlerden farklıdır.

## <a name="enable-a-test-drive-optional"></a>Sınama sürücüsünü etkinleştir (isteğe bağlı)

Test sürücüsü, bu kullanıcılara, bir sabit sayıda saat boyunca önceden yapılandırılmış bir ortama erişim vererek, teklifinizi olası müşterilere sergilemenin harika bir yoludur. Sınama sürücüsü sunumu, daha fazla dönüştürme hızına neden olur ve yüksek oranda nitelikli müşteri adayları üretir. Sınama sürücüleri hakkında daha fazla bilgi için bkz. [test sürücüsü nedir?](partner-center-portal/test-drive.md).

> [!TIP]
> Bir test sürücüsü, ücretsiz deneme sürümünden farklıdır. Bir test sürücüsü, ücretsiz deneme ya da her ikisini birden sağlayabilirsiniz. Bunlar her ikisi de kendi çözümünüzü sabit bir süre için sağlar. Ancak, bir sınama sürücüsü Ayrıca, ürününüzün temel özelliklerine ve avantajlarına yönelik uygulamalı, kendinden kılavuzlu bir tura sahip olan gerçek hayatta bir uygulama senaryosunda gösterilmekte olan avantajlı bir geliştirme içerir.

**Bir sınama sürücüsünü etkinleştirmek için**
1.  **Test sürücüsü**altında, **sınama sürücüsünü etkinleştir** onay kutusunu seçin.
1.  Görüntülenen listeden test sürücü türünü seçin.

## <a name="configure-lead-management"></a>Müşteri adayı yönetimini yapılandırma

Teklifinizi Iş Ortağı Merkezi ile ticari Market 'te yayımladığınızda, Müşteri Ilişkileri yönetimi (CRM) sisteminize bağlayın. Bu, bir kişi, ürününüzü ilgilendiren veya ürünü kullandığı anda müşteri iletişim bilgilerini almanızı sağlar. Bir test sürücüsünü etkinleştirmek istiyorsanız, bir CRM 'ye bağlanmak gerekir (yukarıdaki bölüme bakın). Aksi takdirde, bir CRM 'ye bağlanmak isteğe bağlıdır.

1. **Müşteri liderleri**altında **Bağlan** bağlantısını seçin.
1. **Bağlantı ayrıntıları** iletişim kutusunda listeden bir müşteri adayı hedefi seçin.
1. Görüntülenen alanları doldurun. Ayrıntılı adımlar için aşağıdaki makalelere bakın:

   - [Teklifinizi Azure tablosuna müşteri adayları gönderecek şekilde yapılandırma](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md#configure-your-offer-to-send-leads-to-the-azure-table)
   - [Teklifinizi dynamics 365 müşteri katılımını](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md#configure-your-offer-to-send-leads-to-dynamics-365-customer-engagement) (eski ADıYLA Dynamics CRM Online) göndermek Için uygulamanızı yapılandırın
   - [Teklifinizi, HTTPS uç noktasına müşteri adayları gönderecek şekilde yapılandırma](./partner-center-portal/commercial-marketplace-lead-management-instructions-https.md#configure-your-offer-to-send-leads-to-the-https-endpoint)
   - [Teklifinizi müşteri adaylarını Marketo 'ya gönderecek şekilde yapılandırma](./partner-center-portal/commercial-marketplace-lead-management-instructions-marketo.md#configure-your-offer-to-send-leads-to-marketo)
   - [Teklifinizi Salesforce 'a göndermek için teklifinizi yapılandırma](./partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce.md#configure-your-offer-to-send-leads-to-salesforce)

1. Verdiğiniz yapılandırmayı doğrulamak için **Doğrula** bağlantısını seçin.
1. İletişim kutusunu kapatmak için **Tamam**' ı seçin.

## <a name="resell-through-csps"></a>CSP 'Ler aracılığıyla yeniden satış

[Bulut çözümü sağlayıcısı (CSP)](https://azure.microsoft.com/offers/ms-azr-0145p/) programındaki iş ortakları tarafından kullanılabilir hale getirerek teklifinizin ulaşma durumunu genişletin. Kendi lisansını getir (KLG) planları otomatik olarak programa kabul edilir. KLG olmayan planlarınızı kabul etmek de tercih edebilirsiniz.

Teklifi oluşturmak için **Oluştur** ' u seçin ve devam edin.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal Makine teklifi özelliklerini yapılandırma](azure-vm-create-properties.md)
- [En iyi teklif listeleme deneyimleri](gtm-offer-listing-best-practices.md)