---
title: Azure Veri Kataloğu oluşturma
description: Bu hızlı başlangıç, Azure portalını kullanarak Bir Azure Veri Kataloğu'nun nasıl oluşturulurolduğunu açıklar.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: quickstart
ms.date: 08/01/2019
ms.openlocfilehash: 854760ea0158d356cc5449c9f1fdab6161588640
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "68976871"
---
# <a name="quickstart-create-an-azure-data-catalog"></a>Hızlı Başlangıç: Azure Veri Kataloğu Oluşturma

Azure Veri Kataloğu kurumsal veri varlıkları için bir kayıt sistemi ve bulma sistemi olarak görev yapan tam yönetilen bir bulut hizmetidir. Ayrıntılı bir genel bakış için bkz. [Azure Veri Kataloğu nedir](overview.md).

Bu hızlı başlangıç, bir Azure Veri Kataloğu oluşturmaya başlamanıza yardımcı olur.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için şunları yapmanız gerekir:

* Microsoft [Azure](https://azure.microsoft.com/) aboneliği.
* Kendi Azure Etkin [Dizin kiracınız](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)olması gerekir.

Veri Kataloğu'nu ayarlamak için bir Azure aboneliğinin sahibi veya ortağı olmalısınız.

## <a name="create-a-data-catalog"></a>Veri kataloğu oluşturma

Bir kuruluş (Azure Active Directory etki alanı) için yalnızca bir tane veri kataloğu hazırlayabilirsiniz. Bu nedenle, bu Azure Etkin Dizin etki alanına ait bir Azure aboneliğinin sahibi veya ortağı zaten bir katalog oluşturmuşsa, birden çok Azure aboneliğiniz olsa bile yeniden katalog oluşturamazsınız. Azure Active Directory etki alanınızda bir kullanıcı tarafından veri kataloğu oluşturulup oluşturulmadığını test etmek için [Azure Veri Kataloğu giriş sayfasına](http://azuredatacatalog.com) gidin ve kataloğu görüp görmediğinizi doğrulayın. Sizin için katalog zaten oluşturulmuşsa aşağıdaki yordamı atlayın ve sonraki bölümüne gidin.

1. [Azure portalına](https://portal.azure.com) > gidin**Kaynak oluşturun** ve **Veri Kataloğu'nu**seçin.

    ![Azure Veri Kataloğu oluşturma düğmesi](media/data-catalog-get-started/data-catalog-create.png)

2. Veri kataloğu için bir **ad,** kullanmak istediğiniz **abonelik,** kataloğun **konumunu** ve **fiyatlandırma katmanını**belirtin. Ardından **Oluştur**’u seçin.

3. [Azure Veri Kataloğu giriş sayfasına](http://azuredatacatalog.com) gidin ve **Verileri Yayımla**’ya tıklayın.

   ![Azure Veri Kataloğu--Verileri Yayımla düğmesi](media/data-catalog-get-started/data-catalog-publish-data.png)

   Ayrıca **Başlat'ı**seçerek [Veri Kataloğu hizmet sayfasından Veri Kataloğu](https://azure.microsoft.com/services/data-catalog) ana sayfasına da ulaşabilirsiniz.

   ![Azure Veri Kataloğu--pazarlama giriş sayfası](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)

4. **Ayarlar** sayfasına gidin.

    ![Azure Veri Kataloğu--veri kataloğu hazırlama](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)

5. **Fiyatlandırmayı** genişletin ve Azure Veri Kataloğu **sürümünüzü** (Ücretsiz veya Standart) doğrulayın.

    ![Azure Veri Kataloğu--sürüm seçme](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)

6. Fiyatlandırma katmanınız olarak *Standart* sürümü seçerseniz, **Güvenlik Gruplarını** genişletebilir ve Active Directory güvenlik gruplarının Veri Kataloğu'na erişmesini ve faturalandırmanın otomatik olarak ayarlanmasını onaylamasını sağlayabilirsiniz.

    ![Azure Veri Kataloğu Güvenlik Grupları](media/data-catalog-get-started/data-catalog-standard-security-groups.png)

7. **Katalog Kullanıcıları** seçeneğini genişletin ve **Ekle**’ye tıklayarak veri kataloğu için kullanıcı ekleyin. Bu gruba otomatik olarak eklenirsiniz.

    ![Azure Veri Kataloğu--kullanıcılar](media/data-catalog-get-started/data-catalog-add-catalog-user.png)

8. Fiyatlandırma katmanınız olarak *Standart* sürümü seçerseniz, **Sözlük Yöneticileri'ni** genişletebilir ve sözlük yöneticisi kullanıcıları eklemek için **Ekle'yi** tıklatabilirsiniz. Bu gruba otomatik olarak eklenirsiniz.

    ![Azure Veri Kataloğu Sözlük Yöneticileri](media/data-catalog-get-started/data-catalog-standard-glossary-admin.png)

9. **Katalog Yöneticileri** seçeneğini genişletin ve **Ekle**’ye tıklayarak veri kataloğu için başka yöneticiler ekleyin. Bu gruba otomatik olarak eklenirsiniz.

    ![Azure Veri Kataloğu--yöneticiler](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)

10. **Portal Başlığını** genişletin ve portal başlığında görüntülenecek ek metin ekleyin.

    ![Azure Veri Kataloğu-Portal Başlığı](media/data-catalog-get-started/data-catalog-portal-title.png)

11. **Ayarlar** sayfasını tamamladıktan sonra **Yayımla** sayfasına gidin.

    ![Azure Veri Kataloğu--oluşturuldu](media/data-catalog-get-started/data-catalog-created.png)

## <a name="find-a-data-catalog-in-the-azure-portal"></a>Azure portalında veri kataloğu bulma

1. Web tarayıcısının ayrı bir sekmesinde veya ayrı bir web tarayıcısı penceresinde [Azure portalına](https://portal.azure.com) gidin ve önceki adımda veri kataloğunu oluşturmak için kullandığınız hesabın aynısıyla oturum açın.

2. **Tüm hizmetleri** seçin ve ardından **Veri Kataloğu'nu**tıklatın.

    ![Azure Veri Kataloğu--Azure'a göz atın](media/data-catalog-get-started/data-catalog-browse-azure-portal.png)

    Oluşturduğunuz veri kataloğunu görürsünüz.

    ![Azure Veri Kataloğu--kataloğu listede görüntüleme](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)

3. Oluşturduğunuz kataloğa tıklayın. Portalda **Veri Kataloğu** dikey penceresini görürsünüz.

   ![Azure Veri Kataloğu--portaldaki dikey pencere](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)

4. Veri kataloğunun özelliklerini görüntüleyebilir ve güncelleştirebilirsiniz. Örneğin, **Fiyatlandırma katmanı**’na tıklayın ve sürümü değiştirin.

    ![Azure Veri Kataloğu--fiyatlandırma katmanı](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, kuruluşunuz için bir Azure Veri Kataloğu oluşturmayı öğrendiniz. Artık veri kaynaklarını veri kataloğunuza kaydedebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Veri Kataloğu'nda veri kaynaklarını kaydetme](data-catalog-how-to-register.md)
