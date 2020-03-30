---
title: Ticari pazar programında Azure Tablo| Azure Marketi
description: Azure Blob için müşteri adayı yönetimini yapılandırma
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 7/30/2019
ms.author: dsindona
ms.openlocfilehash: 062252b007e22fcd2644c8b647fc0ecc2f5938cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285257"
---
# <a name="lead-management-instructions-for-azure-blob"></a>Azure Blob için müşteri adayı yönetimi yönergeleri

>[!Caution]
>Pazar yeri teklifinizdeki müşteri adaylarını işlemek için Azure Blob seçeneği gözden küçümsülse. Şu anda Azure Blob için müşteri yönetimi yapılandırmasıyla yayınlanan bir teklifiniz varsa, artık müşteri müşteri adayları almıyorsunuz. Lütfen müşteri adayı yönetimi yapılandırmanızı diğer müşteri adayı yönetimi seçeneklerinden herhangi biriyle güncelleyin. [Müşteri adayı yönetimi açılış sayfasındaki](./commercial-marketplace-get-customer-leads.md)diğer seçenekler hakkında bilgi edinin."

Müşteri İlişkileri Yönetimi (CRM) sisteminiz Azure Marketi ve AppSource müşteri adaylarını almak için İş Ortağı Merkezi'nde açıkça desteklenmiyorsa, bu müşteri adaylarını işlemek için bir Azure Blob kullanabilirsiniz. Daha sonra verileri dışa aktarmayı ve CRM sisteminize aktarmayı seçebilirsiniz. Bu makaledeki yönergeler, bir Azure Depolama hesabı oluşturma işlemi ve bu hesabın altında bir Azure Blob'u oluşturma nız için size bilgi verecektir. Ayrıca, teklifiniz bir müşteri adayı aldığında e-posta bildirimi göndermek için Microsoft Flow'u kullanarak yeni bir akış oluşturabilirsiniz.


## <a name="how-to-configure-azure-blob"></a>Azure Blob nasıl yapılandırılır?

1. Azure hesabınız yoksa, [ücretsiz bir deneme hesabı oluşturabilirsiniz.](https://azure.microsoft.com/pricing/free-trial/)
1. Azure hesabınız etkin olduktan sonra [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure portalında, aşağıdaki yordamı kullanarak bir depolama hesabı oluşturun.  
    1. Sol menü çubuğunda **+Kaynak Oluştur'u** seçin.  **Yeni** bölme (bıçak) sağda görüntülenir.
    2. **Yeni** bölmede **Depolama'yı** seçin.  **Öne Çıkanlar** listesi sağda görüntülenir.
    3. Hesap oluşturmaya başlamak için **Depolama Hesabı'nı** seçin.  Makaledeki yönergeleri izleyin [Bir depolama hesabı oluşturun.](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)

    ![Azure depolama hesabı oluşturma adımları](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-create.png)

    Depolama hesapları hakkında daha fazla bilgi için [Hızlı Başlangıç öğreticisi'ni](https://docs.microsoft.com/azure/storage/)seçin.  Depolama fiyatlandırması hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/pricing/details/storage/)

4. Depolama hesabınız sağlanana kadar bekleyin, bu işlem genellikle birkaç dakika sürer.  Ardından, **tüm kaynaklarınızı Gör'ü** seçerek veya Azure portalının sol daki gezinme menü çubuğundan **tüm kaynakları** seçerek Azure portalının **Giriş** sayfasından depolama hesabınıza erişin.

    ![Azure depolama hesabınıza erişin](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-access.png)

5. Depolama hesabı bölmenizden **Access tuşlarını** seçin ve anahtar için *Bağlantı dize* değerini kopyalayın. Pazar yeri teklifiniz için müşteri adaylarını almak için yayımlama portalında sağlamanız gereken *Depolama Hesabı Bağlantı String* değeri olduğu için bu değeri kaydedin.

     Bir bağlantı sokması bir örnek:

     ```sql
     DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
     ```

    ![Azure depolama anahtarı](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-keys-2.png)

6. Depolama hesabı sayfanızdan **Blobs'u**seçin.

   ![Azure depolama anahtarı](./media/commercial-marketplace-lead-management-instructions-azure-blob/select-blobs.png)

7. Blobs sayfasına çıktıktan sonra **+ Kapsayıcı** düğmesini seçin.

8. Yeni kapsayıcınız için bir **ad** yazın. Kapsayıcı adı küçük harflerden oluşmalı ve bir harf veya rakamla başlamalıdır; yalnızca harf, rakam ve tire (-) karakterini içerebilir. Kapsayıcı ve blob adları hakkında daha fazla bilgi için, [bkz.](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)

    Pazar yeri teklifiniz için müşteri adaylarını almak için yayımlama portalında sağlamanız gereken *Kapsayıcı Adı* değeri olduğu için bu değeri kaydedin.

9. Kapsayıcıya genel erişim düzeyini Özel olarak ayarlayın **(anonim erişim yok).**

10. Kapsayıcıyı oluşturmak için **Tamam**'ı seçin.

    ![Yeni Konteyner](./media/commercial-marketplace-lead-management-instructions-azure-blob/new-container.png)

## <a name="configure-your-offer-to-send-leads-to-the-azure-blob"></a>Müşteri adaylarını Azure Blob'una gönderecek şekilde teklifinizi yapılandırın

Yayımlama portalında teklifiniz için müşteri adayı yönetimi bilgilerini yapılandırmaya hazır olduğunuzda aşağıdaki adımları izleyin:

1. **Teklifiniz** için Teklif kurulum sayfasına gidin.
2. Müşteri Adayı Yönetimi bölümü altında **Bağlan'ı** seçin.

    ![Teklifi Bağla](./media/commercial-marketplace-lead-management-instructions-azure-blob/connect-offer.png)

3. Bağlantı ayrıntıları açılır penceresinde, Müşteri Adayı Hedef için **Azure Blob'u'nu** seçin.

    ![Ayrıntıyı Bağla](./media/commercial-marketplace-lead-management-instructions-azure-blob/connect-details.png) 

4. Bu yönergeleri izleyerek aldığınız **Kapsayıcı adı** ve Depolama Hesabı **Bağlantısı dizesini** sağlayın.

    * Kapsayıcı adı örneği:`marketplaceleadcontainer`
    * Depolama Hesabı Bağlantısı `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net` ![string örneği: Bağlantı Detayı](./media/commercial-marketplace-lead-management-instructions-azure-blob/connection-details.png) 

5. **Kaydet'i**seçin.

    > [!NOTE]
    > Teklifin geri kalanını yapılandırmayı bitirmeniz ve teklifiçin müşteri adayı alabilmek için yayımlamanız gerekir.


