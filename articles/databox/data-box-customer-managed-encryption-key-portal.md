---
title: Azure Data Box için müşteri tarafından yönetilen anahtarları yapılandırmak üzere Azure portal kullanın
description: Azure Data Box için Azure Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırmak üzere Azure portal nasıl kullanacağınızı öğrenin. Müşteri tarafından yönetilen anahtarlar, erişim denetimleri oluşturmanıza, döndürmenize, devre dışı bırakmanızı ve iptal edebilmesini sağlar.
services: databox
author: alkohli
ms.service: databox
ms.topic: how-to
ms.date: 05/07/2020
ms.author: alkohli
ms.subservice: pod
ms.openlocfilehash: 60e621b34250b036888b233b084ba1ddff939048
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87087807"
---
# <a name="use-customer-managed-keys-in-azure-key-vault-for-azure-data-box"></a>Azure Key Vault Azure Data Box için müşteri tarafından yönetilen anahtarları kullanın

Azure Data Box, cihazı bir şifreleme anahtarı aracılığıyla kilitlemek için kullanılan cihaz kilit açma anahtarını (cihaz parolası olarak da bilinir) korur. Varsayılan olarak, Data Box sıra için cihaz kilit açma anahtarı, Microsoft tarafından yönetilen bir anahtarla şifrelenir. Cihaz kilit açma anahtarı üzerinde ek denetim için, müşteri tarafından yönetilen bir anahtar da sağlayabilirsiniz. 

Müşteri tarafından yönetilen anahtarların bir Azure Key Vault oluşturulması ve depolanması gerekir. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](../key-vault/general/overview.md).

Bu makalede, müşteri tarafından yönetilen anahtarların [Azure portal](https://portal.azure.com/)Azure Data Box ile nasıl kullanılacağı gösterilmektedir. Bu makale hem Azure Data Box cihazları hem de Azure Data Box Heavy cihazları için geçerlidir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunlardan emin olun:

1. [Öğretici: order Azure Data Box](data-box-deploy-ordered.md)yönergelerine göre Azure Data Box bir sıra oluşturdunuz.

2. Aygıtın kilit açma anahtarınızı korumak için kullanabileceğiniz bir anahtara sahip mevcut bir Azure Key Vault var. Azure portal kullanarak bir Anahtar Kasası oluşturmayı öğrenmek için bkz. [hızlı başlangıç: Azure Portal kullanarak Azure Key Vault gizli dizi ayarlama ve alma](../key-vault/secrets/quick-create-portal.md).

    - **Geçici silme** ve **Temizleme** , var olan anahtar kasasında ayarlanır. Bu özellikler varsayılan olarak etkinleştirilmemiştir. Bu özellikleri etkinleştirmek için, aşağıdaki makalelerden birinde **geçici silme özelliğini etkinleştirme** ve **Temizleme korumasını etkinleştirme** başlıklı bölümlere bakın:

        - [PowerShell ile geçici silme nasıl kullanılır](../key-vault/general/soft-delete-powershell.md).
        - [CLI ile geçici silme nasıl kullanılır](../key-vault/general/soft-delete-cli.md).
    - Mevcut Anahtar Kasası 2048 boyutunda bir RSA anahtarına veya daha fazlasına sahip olmalıdır. Anahtarlar hakkında daha fazla bilgi için bkz. [Azure Key Vault anahtarları hakkında](../key-vault/keys/about-keys.md).
    - Anahtar Kasası, verileriniz için kullanılan depolama hesaplarıyla aynı bölgede olmalıdır. Birden çok depolama hesabı Azure Data Box kaynağınız ile bağlantılı olabilir.
    - Mevcut bir Anahtar Kasası yoksa, aşağıdaki bölümde açıklandığı gibi satır içi de oluşturabilirsiniz.

## <a name="enable-keys"></a>Anahtarları etkinleştir

Azure Data Box için müşteri tarafından yönetilen anahtarı yapılandırmak isteğe bağlıdır. Varsayılan olarak, Data Box BitLocker anahtarınızı korumak için Microsoft tarafından yönetilen bir anahtar kullanır. Azure portal müşterinin yönettiği bir anahtarı etkinleştirmek için şu adımları izleyin:

1. Data Box siparişiniz için **genel bakış** dikey penceresine gidin.

    ![Data Box Order 'ın genel bakış dikey penceresi](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-1.png)

2. **Ayarlar > şifreleme**' ye gidin. **Şifreleme türü**altında, cihaz kilit açma anahtarınızı nasıl korumak istediğinizi seçebilirsiniz. Varsayılan olarak, cihazın kilit açma parolasının korunması için Microsoft tarafından yönetilen anahtar kullanılır. 

    ![Şifreleme seçeneğini belirleyin](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-2.png)

3. Şifreleme türünü **müşteri tarafından yönetilen anahtar**olarak seçin. Müşteri tarafından yönetilen anahtarı seçtikten sonra **bir anahtar kasası ve anahtar seçin**.

    ![Müşteri tarafından yönetilen anahtar seçin](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-3.png)

5. **Azure Key Vault anahtarı seç** dikey penceresinde, abonelik otomatik olarak doldurulur. **Anahtar Kasası**için, açılan listeden var olan bir anahtar kasasını seçebilirsiniz.

    ![Yeni Azure Key Vault oluştur](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-31.png)

    Yeni bir Anahtar Kasası oluşturmak için **Yeni oluştur** ' u de seçebilirsiniz. **Anahtar Kasası oluştur dikey**penceresinde, kaynak grubunu ve Anahtar Kasası adını girin. **Geçici silme** ve **Temizleme korumasının** etkinleştirildiğinden emin olun. Diğer tüm varsayılanları kabul edin. **Gözden geçir + oluştur**' u seçin.

    ![Yeni Azure Key Vault oluştur](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-4.png)

7. Anahtar kasanız ile ilişkili bilgileri gözden geçirin ve **Oluştur**' u seçin. Anahtar Kasası oluşturma işleminin tamamlanabilmesi için birkaç dakika bekleyin.

    ![Azure Key Vault oluştur](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-5.png)

8. **Azure Key Vault anahtarı seç**' de, varolan anahtar kasasında bir anahtar seçebilirsiniz.

    ![Azure Key Vault yeni anahtar oluştur](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-6.png)

9. Yeni bir anahtar oluşturmak isterseniz, anahtar oluşturmak için **Yeni oluştur** ' u seçin. RSA anahtar boyutu 2048 veya daha büyük olabilir.

    ![Azure Key Vault yeni anahtar oluştur](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-61.png)

10. Anahtarınız için ad belirtin, diğer varsayılanları kabul edin ve **Oluştur**' u seçin. 

    ![Yeni anahtar oluştur](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-7.png)


11. Anahtar Kasanızda bir anahtarın oluşturulduğunu size bildirirsiniz. **Sürümü** seçin ve ardından **Seç**' i seçin.

    ![Anahtar Kasası 'nda yeni anahtar oluşturuldu](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-8.png)

12. **Şifreleme türü** bölmesinde, anahtar kasasını ve müşterinizin yönettiği anahtar için seçilen anahtarı görebilirsiniz.

    ![Müşteri tarafından yönetilen anahtar için anahtar ve Anahtar Kasası](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-9.png)

13. Anahtarı kaydedin. 

    ![Müşteri tarafından yönetilen anahtarı Kaydet](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-10.png)

    Anahtar URL 'SI, **şifreleme türü**altında görüntülenir.

    ![Müşteri tarafından yönetilen anahtar URL 'SI](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-11.png)

> [!IMPORTANT]
> Microsoft yönetilen anahtarını devre dışı bırakabilir ve Data Box sırasının herhangi bir aşamasında müşteri tarafından yönetilen anahtara geçebilirsiniz. Ancak, müşteri tarafından yönetilen anahtarı oluşturduktan sonra, Microsoft tarafından yönetilen anahtara geri dönemezsiniz.

## <a name="troubleshoot-errors"></a>Sorun giderme hataları

Müşteri tarafından yönetilen anahtarınızla ilgili herhangi bir hata alırsanız, sorunlarını gidermek için aşağıdaki tabloyu kullanın.

| Hata kodu| Hata ayrıntıları| Gider?|
|-------------|--------------|---------|
| SsemUserErrorEncryptionKeyDisabled| Müşteri tarafından yönetilen anahtar devre dışı bırakıldığından geçiş anahtarı getirilemedi.| Evet, anahtar sürümünü etkinleştirerek.|
| Ssemusererrorencryptionkeyerermedi| Müşteri tarafından yönetilen anahtarın süresi geçtiğinden geçiş anahtarı getirilemedi.| Evet, anahtar sürümünü etkinleştirerek.|
| Ssemusererrorkeyayrıntılar NotFound| Müşteri tarafından yönetilen anahtar bulunamadığı için geçiş anahtarı getirilemedi.| Anahtar kasasını sildiyseniz, müşteri tarafından yönetilen anahtarı kurtaramazsınız.  Anahtar kasasını farklı bir kiracıya geçirdiyseniz, [abonelik taşıdıktan sonra Anahtar Kasası KIRACı kimliğini değiştirme](../key-vault/general/move-subscription.md)konusuna bakın. Anahtar kasasını sildiyseniz:<ol><li>Evet, temizleme koruma süresi içinde ise, [anahtar kasasını kurtarma](../key-vault/general/soft-delete-powershell.md#recovering-a-key-vault)bölümündeki adımları kullanarak.</li><li>Temizleme koruma süresinin ötesinde, hayır.</li></ol><br>Diğer bir deyişle, Anahtar Kasası bir kiracı geçişi gerçekleştirmişse, aşağıdaki adımlardan biri kullanılarak kurtarılabilir: <ol><li>Anahtar kasasını eski kiracıya geri döndürür.</li><li>`Identity = None`Değerini ayarlayın ve ardından değerine geri ayarlayın `Identity = SystemAssigned` . Bu, yeni kimlik oluşturulduktan sonra kimliği siler ve yeniden oluşturur. `Get` `Wrap` `Unwrap` Anahtar kasasının erişim ilkesindeki yeni kimlik için, ve izinlerini etkinleştirin.</li></ol> |
| SsemUserErrorKeyVaultBadRequestException| Müşteri tarafından yönetilen anahtar erişimi iptal edildiğinden geçiş anahtarı getirilemedi.| Evet, aşağıdakileri denetle: <ol><li>Anahtar Kasası hala MSI 'ye erişim ilkesinde sahiptir.</li><li>Erişim ilkesi, almak, kaydırmak, sarmadan kaldırmak için izinler sağlar.</li><li>Anahtar Kasası güvenlik duvarının arkasındaki bir vNet 'daysanız, **Microsoft güvenilir Hizmetleri 'Ne Izin ver** ' in etkinleştirilip etkinleştirilmediğini denetleyin.</li></ol>|
| Ssemusererrorkeyvaultayrıntılar NotFound| Müşteri tarafından yönetilen anahtar için ilişkili anahtar kasası bulunamadığı için geçiş anahtarı getirilemedi. | Anahtar kasasını sildiyseniz, müşteri tarafından yönetilen anahtarı kurtaramazsınız.  Anahtar kasasını farklı bir kiracıya geçirdiyseniz, [abonelik taşıdıktan sonra Anahtar Kasası KIRACı kimliğini değiştirme](../key-vault/general/move-subscription.md)konusuna bakın. Anahtar kasasını sildiyseniz:<ol><li>Evet, temizleme koruma süresi içinde ise, [anahtar kasasını kurtarma](../key-vault/general/soft-delete-powershell.md#recovering-a-key-vault)bölümündeki adımları kullanarak.</li><li>Temizleme koruma süresinin ötesinde, hayır.</li></ol><br>Diğer bir deyişle, Anahtar Kasası bir kiracı geçişi gerçekleştirmişse, aşağıdaki adımlardan biri kullanılarak kurtarılabilir: <ol><li>Anahtar kasasını eski kiracıya geri döndürür.</li><li>`Identity = None`Değerini ayarlayın ve ardından değerine geri ayarlayın `Identity = SystemAssigned` . Bu, yeni kimlik oluşturulduktan sonra kimliği siler ve yeniden oluşturur. `Get` `Wrap` `Unwrap` Anahtar kasasının erişim ilkesindeki yeni kimlik için, ve izinlerini etkinleştirin.</li></ol> |
| SsemUserErrorSystemAssignedIdentityAbsent  | Müşteri tarafından yönetilen anahtar bulunamadığı için geçiş anahtarı getirilemedi.| Evet, aşağıdakileri denetle: <ol><li>Anahtar Kasası hala MSI 'ye erişim ilkesinde sahiptir.</li><li>Kimlik, atanan sistem türünde.</li><li>Anahtar kasasının erişim ilkesindeki kimlik için Get, Wrap ve Unwrap izinlerini etkinleştirin.</li></ol>|
| Genel hata  | Geçiş anahtarı getirilemedi.| Bu genel bir hatadır. Hatayı gidermek ve sonraki adımları öğrenmek için Microsoft Desteği başvurun.|


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault nedir](../key-vault/general/overview.md)?