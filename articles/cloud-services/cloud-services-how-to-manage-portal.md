---
title: Yaygın bulut hizmeti yönetimi görevleri | Microsoft Dokümanlar
description: Azure portalında Bulut Hizmetlerini nasıl yönetirize edin. Bu örnekler Azure portalını kullanır.
services: cloud-services
documentationcenter: ''
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 07/05/2017
ms.author: tagore
ms.openlocfilehash: 80481bc11933b0404079221f23b5054024f00acb
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811357"
---
# <a name="manage-cloud-services-in-the-azure-portal"></a>Azure portalında Bulut Hizmetlerini Yönetme
Azure portalının **Bulut Hizmetleri** alanında şunları yapabilirsiniz:

* Bir hizmet rolünü veya dağıtımı güncelleştirin.
* Aşamalı bir dağıtımı üretime teşvik edin.
* Kaynak bağımlılıklarını görebilmeniz ve kaynakları birlikte ölçeklendirebilmeniz için kaynakları bulut hizmetinize bağlayın.
* Bulut hizmetini veya dağıtımı silin.

Bulut hizmetinizi nasıl ölçeklendirecekhakkında daha fazla bilgi için, [portaldaki bir bulut hizmeti için otomatik ölçeklendirmeyi yapılandır' a](cloud-services-how-to-scale-portal.md)bakın.

## <a name="update-a-cloud-service-role-or-deployment"></a>Bulut hizmeti rolünü veya dağıtımı güncelleştirme
Bulut hizmetiniz için uygulama kodunu güncelleştirmeniz gerekiyorsa, bulut hizmeti bıçağında **Güncelleştir'i** kullanın. Tek bir rolü veya tüm rolleri güncelleştirebilirsiniz. Güncellemek için yeni bir hizmet paketi veya hizmet yapılandırma dosyası yükleyebilirsiniz.

1. Azure [portalında,][Azure portal]güncelleştirmek istediğiniz bulut hizmetini seçin. Bu adım, bulut hizmeti örnek bıçak açılır.

2. Bıçakta **Güncelleştir'i**seçin.

    ![Güncelleme düğmesi](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Dağıtımı yeni bir hizmet paketi dosyası (.cspkg) ve hizmet yapılandırma dosyası (.cscfg) ile güncelleştirin.

    ![GüncelleştirmeDağıtım](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. İsteğe bağlı olarak, depolama hesabını ve dağıtım etiketini güncelleştirin.

5. Herhangi bir rol yalnızca bir rol örneği varsa, yükseltmenin devam etmesini sağlamak için bir veya daha fazla rol tek bir örnek onay kutusu **içerse bile Dağıt'ı** seçin.

    Azure, her rolün en az iki rol örneği (sanal makineler) varsa, bulut hizmeti güncelleştirmesi sırasında yalnızca yüzde 99,95 hizmet kullanılabilirliğini garanti edebilir. İki rol örneğiyle, bir sanal makine istemci isteklerini işlerken diğeri güncelleştirilir.

6. Paketin yüklenmesi bittikten sonra güncelleştirmeyi uygulamak için **Başlat dağıtım** onay kutusunu seçin.

7. Hizmeti güncelleştirmeye başlamak için **Tamam'ı** seçin.

## <a name="swap-deployments-to-promote-a-staged-deployment-to-production"></a>Aşamalı bir dağıtımı üretime teşvik etmek için dağıtımları değiştirme
Bulut hizmetinin yeni bir sürümü dağıtmaya karar verdiğinde, yeni sürümünüzü bulut hizmeti hazırlama ortamınızda sahneleyip test edin. İki dağıtımın ele alındığı URL'leri değiştirmek ve yeni bir sürümü üretime tanıtmak için **Takas'ı** kullanın.

Bulut **Hizmetleri** sayfasından veya panodaki dağıtımları değiştirebilirsiniz.

1. Azure [portalında,][Azure portal]güncelleştirmek istediğiniz bulut hizmetini seçin. Bu adım, bulut hizmeti örnek bıçak açılır.

2. Bıçakta **Takas'ı**seçin.

    ![Bulut Hizmetleri Takas düğmesi](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Aşağıdaki onay istemi açılır:

    ![Bulut Hizmetleri Takas](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Dağıtım bilgilerini doğruladıktan sonra, dağıtımları değiştirmek için **Tamam'ı** seçin.

    Dağıtım değişimi hızlı bir şekilde gerçekleşir, çünkü değişen tek şey dağıtımlar için sanal IP adresleri (VIP'ler) olur.

    İşlem maliyetlerinden tasarruf etmek için, üretim dağıtımınızın beklendiği gibi çalıştığını doğruladıktan sonra hazırlama dağıtımını silebilirsiniz.

### <a name="common-questions-about-swapping-deployments"></a>Dağıtımları değiştirme yle ilgili sık sorulan sorular

**Dağıtımları değiştirmenin ön koşulları nelerdir?**

Başarılı bir dağıtım değişimi için iki temel ön koşul vardır:

- Üretim yuvanız için statik bir IP adresi kullanmak istiyorsanız, evreleme yuvanız için de bir tane rezerve etmelisiniz. Aksi takdirde, takas başarısız olur.

- Değiş tokuşu gerçekleştiremeden önce rollerinizin tüm örnekleri çalışıyor olmalıdır. Azure portalının **Genel Bakış** bıçağında örneklerinizin durumunu kontrol edebilirsiniz. Alternatif olarak, Windows PowerShell'de [AzureRol Al](/powershell/module/servicemanagement/azure/get-azurerole?view=azuresmps-3.7.0) komutunu kullanabilirsiniz.

Konuk işletim sistemi güncelleştirmelerinin ve hizmet iyileştirme işlemlerinin dağıtım takaslarının başarısız lığa da neden olabileceğini unutmayın. Daha fazla bilgi için [bulut hizmeti dağıtım sorunlarını giderme sorununa](cloud-services-troubleshoot-deployment-problems.md)bakın.

**Bir takas, başvurum için kapalı kalma süresine neden olur mu? Bununla nasıl başa çıkmalıyım?**

Önceki bölümde açıklandığı gibi, azure yük bakiyesinde yalnızca bir yapılandırma değişikliği olduğundan dağıtım değişimi genellikle hızlıdır. Bazı durumlarda, 10 veya daha fazla saniye sürebilir ve geçici bağlantı hatalarına neden olabilir. Müşterilerinizüzerindeki etkiyi sınırlamak [için, istemci yeniden deneme mantığını](../best-practices-retry-general.md)uygulamayı düşünün.

## <a name="delete-deployments-and-a-cloud-service"></a>Dağıtımları ve bulut hizmetini silme
Bir bulut hizmetini silmeden önce, varolan her dağıtımı silmeniz gerekir.

İşlem maliyetlerinden tasarruf etmek için, üretim dağıtımınızın beklendiği gibi çalıştığını doğruladıktan sonra hazırlama dağıtımını silebilirsiniz. Durdurulan dağıtılmış rol örnekleri için işlem maliyetleri için faturalandırılırsınız.

Bir dağıtımı veya bulut hizmetinizi silmek için aşağıdaki yordamı kullanın.

1. Azure [portalında,][Azure portal]silmek istediğiniz bulut hizmetini seçin. Bu adım, bulut hizmeti örnek bıçak açılır.

2. Bıçakta **Sil'i**seçin.

    ![Bulut Hizmetleri Sil düğmesi](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Bulut hizmetinin tamamını silmek için **Bulut hizmetini ve dağıtımları** onay kutusunu seçin. Veya **Üretim dağıtımı** veya Hazırlama **dağıtım** onay kutusunu seçebilirsiniz.

    ![Bulut Hizmetleri Silme](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Alttaki **Sil'i** seçin.

5. Bulut hizmetini silmek için **bulut hizmetini sil'i**seçin. Daha sonra onay isteminde **Evet'i**seçin.

> [!NOTE]
> Bir bulut hizmeti silindiğinde ve ayrıntılı izleme yapılandırıldığı zaman, verileri depolama hesabınızdan el ile silmeniz gerekir. Ölçüm tablolarını nerede bulacağınız hakkında bilgi için bulut [hizmeti denetimine giriş bilgisine](cloud-services-how-to-monitor.md)bakın.


## <a name="find-more-information-about-failed-deployments"></a>Başarısız dağıtımlar hakkında daha fazla bilgi edinin
**Genel Bakış** bıçağının üst kısmında bir durum çubuğu vardır. Çubuğu seçtiğinizde, yeni bir bıçak açılır ve herhangi bir hata bilgisi görüntüler. Dağıtımherhangi bir hata içermiyorsa, bilgi bıçağı boştur.

![Bulut Hizmetlerine Genel Bakış](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Sonraki adımlar
* [Bulut hizmetinizin genel yapılandırması.](cloud-services-how-to-configure-portal.md)
* [Bulut hizmetini](cloud-services-how-to-create-deploy-portal.md)nasıl dağıtılayın öğrenin.
* Özel bir [etki alanı adı](cloud-services-custom-domain-name-portal.md)yapılandırın.
* [TLS/SSL sertifikalarını](cloud-services-configure-ssl-certificate-portal.md)yapılandırın.



