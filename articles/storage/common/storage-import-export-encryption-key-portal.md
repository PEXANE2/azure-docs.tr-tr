---
title: Alma/Dışa Aktarma hizmeti için müşteri tarafından yönetilen anahtarları yapılandırmak için Azure portalını kullanın
description: Azure İçe Alma/Dışa Aktarma hizmeti için Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarları yapılandırmak için Azure portalını nasıl kullanacağınızı öğrenin. Müşteri tarafından yönetilen anahtarlar, erişim denetimleri oluşturmanıza, döndürmenize, devre dışı bettirmenize ve iptal etmenizi sağlar.
services: storage
author: alkohli
ms.service: storage
ms.topic: how-to
ms.date: 03/12/2020
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: ca1327a547e8550e47ff37e4ba100fcbd2b7a79f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282469"
---
# <a name="use-customer-managed-keys-in-azure-key-vault-for-importexport-service"></a>Alma/Dışa Aktarma hizmeti için Azure Anahtar Kasası'nda müşteri tarafından yönetilen anahtarları kullanma

Azure İçe Aktarma/Verme, sürücüleri şifreleme anahtarı yla kilitlemek için kullanılan BitLocker anahtarlarını korur. Varsayılan olarak, BitLocker anahtarları Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için müşteri tarafından yönetilen anahtarlar da sağlayabilirsiniz.

Müşteri tarafından yönetilen anahtarlar oluşturulmalı ve Azure Anahtar Kasası'nda depolanmalıdır. Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](../../key-vault/key-vault-overview.md)

Bu makalede, [Azure portalında](https://portal.azure.com/)Alma/Dışa Aktarma hizmeti yle müşteri tarafından yönetilen anahtarların nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunlardan emin olun:

1. Aşağıdaki talimatlara göre bir alma veya dışa aktarma işi oluşturdunuz:

    - [Lekeler için bir alma işi oluşturun.](storage-import-export-data-to-blobs.md)
    - [Dosyalar için bir alma işi oluşturun.](storage-import-export-data-to-files.md)
    - [Lekeler için bir dışa aktarma işi oluşturma](storage-import-export-data-from-blobs.md)

2. BitLocker anahtarınızı korumak için kullanabileceğiniz bir anahtar bulunan mevcut bir Azure Anahtar Kasası'na sahipsiniz. Azure portalını kullanarak önemli bir kasa oluşturmayı öğrenmek için [Quickstart: Azure portalını kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın.](../../key-vault/quick-create-portal.md)

    - **Yumuşak silme** ve **temizleme yi yok** varolan Key Vault ayarlanır. Bu özellikler varsayılan olarak etkinleştirilir. Bu özellikleri etkinleştirmek için aşağıdaki makalelerden birinde **yumuşak silmeyi etkinleştirme** ve **Temizleme Korumasını Etkinleştirme** başlıklı bölümlere bakın:

        - [PowerShell ile yumuşak silme nasıl kullanılır.](../../key-vault/key-vault-soft-delete-powershell.md)
        - [CLI ile yumuşak silme nasıl kullanılır.](../../key-vault/key-vault-soft-delete-cli.md)
    - Mevcut anahtar kasası 2048 boyutunda veya daha büyük bir RSA anahtarına sahip olmalıdır. Anahtarlar hakkında daha fazla bilgi için [Azure Anahtar Kasası tuşları, sırlar ve sertifikalar hakkında](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys)Key Vault **tuşlarına** bakın.
    - Anahtar kasası, verilerinizin depolama hesabıyla aynı bölgede olmalıdır.  
    - Varolan bir Azure Anahtar Kasası yoksa, aşağıdaki bölümde açıklandığı gibi satır satırda da oluşturabilirsiniz.

## <a name="enable-keys"></a>Anahtarları etkinleştir

İçe Alma/Dışa Aktarma hizmetiniz için müşteri tarafından yönetilen anahtarı yapılandırmak isteğe bağlıdır. Varsayılan olarak, İçe Alma/Dışa Aktarma hizmeti BitLocker anahtarınızı korumak için Microsoft yönetilen bir anahtar kullanır. Azure portalında müşteri tarafından yönetilen anahtarları etkinleştirmek için aşağıdaki adımları izleyin:

1. Alma işiniz için **Genel Bakış** bıçağına gidin.
2. Sağ bölmede **BitLocker anahtarlarınızın nasıl şifreleneceğini seçin'i**seçin.

    ![Şifreleme seçeneğini seçin](./media/storage-import-export-encryption-key-portal/encryption-key-1.png)

3. **Şifreleme** bıçağında, aygıt BitLocker anahtarını görüntüleyebilir ve kopyalayabilirsiniz. **Şifreleme türü**altında, BitLocker anahtarınızı nasıl korumak istediğinizi seçebilirsiniz. Varsayılan olarak, Microsoft yönetilen bir anahtar kullanılır.

    ![BitLocker tuşunu görüntüle](./media/storage-import-export-encryption-key-portal/encryption-key-2.png)

4. Müşteri yönetilen anahtarını belirtme seçeneğiniz var. Müşteri yönetilen anahtarıseçtikten sonra, **anahtar kasasını ve anahtarı seçin.**

    ![Müşteri yönetilen anahtarını seçin](./media/storage-import-export-encryption-key-portal/encryption-key-3.png)

5. Azure **Key Vault bıçağından Seç tuşuile** abonelik otomatik olarak doldurulur. **Anahtar kasası**için, açılır listeden varolan bir anahtar kasası seçebilirsiniz.

    ![Azure Anahtar Kasası'nı seçin veya oluşturun](./media/storage-import-export-encryption-key-portal/encryption-key-4.png)

6. Ayrıca yeni bir anahtar tonoz oluşturmak için **yeni oluştur'u** da seçebilirsiniz. Create **tuşu tonoz bıçağında**kaynak grubunu ve anahtar kasa adını girin. Diğer tüm varsayılanları kabul edin. **Gözden Geçir + Oluştur'u**seçin.

    ![Yeni Azure Anahtar Kasası Oluşturma](./media/storage-import-export-encryption-key-portal/encryption-key-5.png)

7. Anahtar kasanızla ilişkili bilgileri gözden geçirin ve **Oluştur'u**seçin. Anahtar tonoz oluşturma tamamlamak için birkaç dakika bekleyin.

    ![Azure Anahtar Kasası Oluşturma](./media/storage-import-export-encryption-key-portal/encryption-key-6.png)

8. Azure **Anahtar Kasası'ndan Seç tuşu'nda,** varolan anahtar kasasında bir anahtar seçebilirsiniz.

9. Yeni bir anahtar kasası oluşturduysanız, anahtar oluşturmak için **yeni oluştur'u** seçin. RSA anahtar boyutu 2048 veya daha büyük olabilir.

    ![Azure Anahtar Kasası'nda yeni anahtar oluşturma](./media/storage-import-export-encryption-key-portal/encryption-key-7.png)

    Anahtar kasasını oluşturduğunuzda yumuşak silme ve temizleme koruması etkinleştirilmezse, anahtar kasası yumuşak silme ve temizleme koruması etkin olacak şekilde güncelleştirilir.

10. Anahtarınızın adını sağlayın, diğer varsayılanları kabul edin ve **Oluştur'u**seçin.

    ![Yeni anahtar oluşturma](./media/storage-import-export-encryption-key-portal/encryption-key-8.png)

11. **Sürümü** seçin ve sonra **Seçin'i**seçin. Anahtar kasanızda bir anahtar oluşturulduğu size bildirilir.

    ![Anahtar kasasında oluşturulan yeni anahtar](./media/storage-import-export-encryption-key-portal/encryption-key-9.png)

**Şifreleme** bıçağında, müşteri yönetilen anahtarınız için seçilen anahtar kasasını ve anahtarı görebilirsiniz.

## <a name="disable-keys"></a>Anahtarları devre dışı

Microsoft yönetilen anahtarları yalnızca devre dışı bırakıp, alma/dışa aktarma işinin herhangi bir aşamasında müşteri yönetilen anahtarlarına taşıyabilirsiniz. Ancak, müşteri yönetilen anahtarı nı oluşturduktan sonra devre dışı kalamazsınız.

## <a name="troubleshoot-customer-managed-key-errors"></a>Müşteri tarafından yönetilen anahtar hatalarını giderme

Müşteri yönetilen anahtarınızla ilgili herhangi bir hata alırsanız, sorun giderme için aşağıdaki tabloyu kullanın:

| Hata kodu     |Ayrıntılar     | Kurtarılabilir?    |
|----------------|------------|-----------------|
| CmkErrorAccessİptal | Müşteri yönetilen anahtar uygulandı, ancak anahtar erişimi şu anda iptal edildi. Daha fazla bilgi için, anahtar erişimini nasıl [etkinleştirin' e](https://docs.microsoft.com/rest/api/keyvault/vaults/updateaccesspolicy)bakın.                                                      | Evet, kontrol edin: <ol><li>Anahtar kasası erişim ilkesinde hala MSI'ye sahip.</li><li>Erişim ilkesi, Get, Wrap, Unwrap için izinler sağlar.</li><li>Anahtar kasası güvenlik duvarının arkasındaki bir vNet'teyse, **Microsoft Trusted Services'a İzin Ver'in** etkin olup olmadığını denetleyin.</li></ol>                                                                                            |
| CmkHatası Devre Dışı Bırakma      | Müşteri yönetilen anahtar uygulanır, ancak anahtar devre dışı bırakılır. Daha fazla bilgi için anahtarı nasıl [etkinleştirin' e](https://docs.microsoft.com/rest/api/keyvault/vaults/createorupdate)bakın.                                                                             | Evet, anahtar sürümünü etkinleştirerek     |
| CmkErrorNotFound      | Müşteri yönetilen bir anahtar uygulandı, ancak anahtarı bulamıyor. <br>Anahtar bekletme döneminden sonra silinir ve temizlenirse, anahtarı kurtaramazsınız. Anahtarı yedeklediyseniz, bu sorunu gidermek için anahtarı geri yükleyebilirsiniz. | Hayır, anahtar silindi ve bekletme döneminden sonra da temizlendi. <br>Evet, yalnızca müşteri anahtar yedeklenir ve geri yüklerse.  |
| CmkErrorVaultNotFound | Müşteri yönetilen bir anahtar uygulanır, ancak anahtarla ilişkili anahtar kasasını bulamıyor.<br>Anahtar kasasını sildiyseniz, müşteri yönetilen anahtarını kurtaramazsınız.  Anahtar kasasını farklı bir kiracıya taşıdıysanız, [bkz.](https://docs.microsoft.com/azure/key-vault/key-vault-subscription-move-fix) |   Hayır, eğer müşteri anahtar kasasını sildiyse.<br> Evet, anahtar kasakiracı göçünden geçtiyse, şu: <ol><li>anahtar kasasını eski kiracıya geri taşıyın.</li><li>set Identity = Yok ve sonra geri Identity = SystemAssigned, bu siler ve kimlik yeniden oluşturur</li></ol><br>Not: Kiracı geçiş durumu sınırlı anlayışa dayanır, gerçek davranışı test etmek ve onaylamak gerekir, daha sonra gözden geçirilebilir. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Anahtar Kasası Nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
