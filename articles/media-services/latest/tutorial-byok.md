---
title: Media Services ile müşteri tarafından yönetilen anahtarlar veya kendi anahtarınızı getir (BYOK)
description: Bu, Media Services depolama hesabı ile müşteri tarafından yönetilen anahtarların kullanılmasına yönelik bir öğreticidir
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: tutorial
ms.date: 10/18/2020
ms.openlocfilehash: c26da9d888bbcdf72c052fa4bd7fcdf443a2d8f7
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92326168"
---
# <a name="tutorial-use-customer-managed-keys-or-bring-your-own-key-byok-with-media-services"></a>Öğretici: müşteri tarafından yönetilen anahtarları kullanın veya Media Services ile kendi anahtarınızı (BYOK) getirin

2020-05-01 API 'siyle, sistem tarafından yönetilen bir kimliği olan bir Media Services hesabıyla müşterinin yönettiği bir RSA anahtarı kullanabilirsiniz.  Bu öğretici, Azure hizmetlerine REST istekleri göndermek için bir Postman koleksiyonu ve ortamı içerir.  Kullanılan hizmetler:

- Postman için uygulama kaydını Azure Active Directory
- Microsoft Graph API
- Azure depolama
- Azure Key Vault
- Media Services

Bu öğreticide, şu şekilde Postman kullanmayı öğreneceksiniz:

> [!div class="checklist"]
> * Azure hizmetleriyle kullanılacak belirteçleri alın.
> * Bir kaynak grubu ve bir depolama hesabı oluşturun.
> * Sistem tarafından yönetilen kimliği olan bir Media Services hesabı oluşturun
> * Depolama hesabıyla kullanılmak üzere bir RSA (müşteri tarafından yönetilen) anahtarı depolamak için bir Key Vault oluşturun.
> * Media Services hesabını, RSA anahtarını depolama hesabıyla kullanacak şekilde güncelleştirin.
> * Postman 'da değişkenleri kullanın.

Azure aboneliğiniz yoksa [ücretsiz bir deneme hesabı oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

- Uygun izinlerle bir hizmet sorumlusu kaydedin.
- [Postman](https://www.postman.com)'yi yükleme.
- Bu öğretici için Postman koleksiyonunu Azure örnekleri ' nde indirin [: Media-Services-müşteri-yönetilen-anahtarlar-bYok](https://github.com/Azure-Samples/media-services-customer-managed-keys-byok)

### <a name="register-a-service-principal-with-the-needed-permissions"></a>Gerekli izinlere sahip bir hizmet sorumlusu kaydetme

[Hizmet sorumlusu oluşturun](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).  Hizmet sorumlusu gizliliğini almak için bkz. [Iki seçenek](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#authentication-two-options) .  Gizli dizi, portalda gizli dizi sayfasından ayrıldığınızda, erişilemez olur.

Hizmet sorumlusu, bu öğreticide görevleri gerçekleştirmek için aşağıdaki izinlere ihtiyaç duyuyor.

![hizmet sorumlusu için gereken izinler](./media/tutorial-byok/service-principal-permissions-1.png)

### <a name="install-postman"></a>Postman yükleme

Azure ile kullanmak için Postman 'ı henüz yüklemediyseniz, [Postman.com](https://www.postman.com/)adresinden edinebilirsiniz.

### <a name="download-and-import-the-collection"></a>Koleksiyonu indirme ve içeri aktarma

Bu öğretici için Postman koleksiyonunu Azure örnekleri ' nde indirin [: Media-Services-müşteri-yönetilen-anahtarlar-bYok](https://github.com/Azure-Samples/media-services-customer-managed-keys-byok)

## <a name="installation-of-collection-and-environment"></a>Koleksiyon ve ortam yüklemesi

1. Postman 'ı çalıştırın.
1. **İçeri aktar**'ı seçin.
1. **Dosyaları karşıya yükle**' yi seçin.
1. Koleksiyonu ve ortam dosyalarını kaydettiğiniz yere gidin.
1. Koleksiyon ve ortam dosyasını seçin.
1. **Aç**’ı seçin.  (Dosyalar bir API olarak içeri aktarılmayacak, ancak koleksiyonlar olarak alınamadığına ilişkin bir uyarı görürsünüz.  Bu çok uygundur.  İstediğiniz şeydir.)
1. Bu koleksiyon artık koleksiyonunuzda BYOK olarak görünür.
1. Ortam değişkenleri artık ortamlarınızda görüntülenir.

### <a name="understand-the-rest-api-requests-in-the-collection"></a>Koleksiyondaki REST API isteklerini anlayın

Koleksiyon aşağıdaki REST API isteklerini sağlar. İsteklerin çoğu, dizideki bir sonraki (veya sonraki) istek için dinamik olarak genel değişkenler oluşturan test betiklerine sahip olduğundan, istekler belirtilen sırada gönderilmesi gerekir. Genel değişkenleri el ile oluşturmak gerekli değildir.

Postman 'da, parantez içinde bulunan bu değişkenleri görürsünüz `{{ }}` .  Örneğin, `{{bearerToken}}`.

1. AAD belirtecini al-test, genel değişken *Yatatoken* 'ı ayarlar
2. Grafik belirtecini al-test, *Graphtoken* genel değişkenini ayarlar
3. Hizmet sorumlusu ayrıntılarını al-test, *Serviceprincipalobjectıd* genel değişkenini ayarlar
4. Depolama hesabı oluşturma-test, *Storageaccountıd* genel değişkenini ayarlar
5. Sistem tarafından yönetilen kimliğe sahip bir Media Services hesabı oluşturun-test, *PrincipalId* genel değişkenini ayarlar
6. Key Vault oluşturma ve hizmet sorumlusuna erişim verme-test, *Keyvaultıd* genel değişkenini ayarlar
7. Key Vault belirteci al-test kümeleri genel değişkeni *Keyvaulttoken*
8. Anahtar kasasında RSA anahtarı oluşturma-test kümeleri genel değişkeni *keyId*
9. Depolama hesabıyla anahtarı kullanmak için Media Services hesabını güncelleştirin-bu istek için bir sınama betiği yok.

## <a name="define-environment-variables"></a>Ortam değişkenlerini tanımlama

1. Ortamlar açılan listesini seçerek indirdiğiniz ortama geçiş yapın.
1. Postman 'da ortam değişkenlerinizi oluşturun. Bunlar, köşeli ayraçlar içinde bulunan değişkenler olarak da kullanılır `{{ }}` .  Örneğin, `{{tenantId}}`.

    * *Tenantıd* = kiracı kimliğiniz
    * *Serviceprincipalıd* = en sevdiğiniz yöntemden oluşturduğunuz HIZMET sorumlusunun kimliği: Portal, CLI ve benzeri.
    * *Servicesprincipalsecret* = hizmet sorumlusu için oluşturulan gizli dizi
    * *abonelik* = abonelik kimliğiniz
    * *Storagename* = depolamaya vermek istediğiniz ad
    * *AccountName* = kullanmak Istediğiniz medya hizmeti hesap adı
    * *Keyvaultname* = kullanmak istediğiniz Key Vault adı
    * *ResourceLocation* = merkezileştirme (veya her zaman kaynaklarınızı nereye koymak istiyorsunuz.  Bu koleksiyon yalnızca merkezileştirme ile test edilmiştir.)
    * *resourceGroup* = kaynak grubu adı

    Aşağıdaki değişkenler Azure kaynaklarıyla çalışmaya yönelik standarttır. bu nedenle, bunları değiştirmenize gerek yoktur.

    * *armResource* = `https://management.core.windows.net`
    * *graphResource* = `https://graph.windows.net/`
    * *keyVaultResource* = `https://vault.azure.net`
    * *armEndpoint* = `management.azure.com`
    * *graphEndpoint* = `graph.windows.net`
    * *aadEndpoint* = `login.microsoftonline.com`
    * *keyVaultDomainSuffix* = `vault.azure.net`

## <a name="send-the-requests"></a>İstekleri Gönder

Ortam değişkenleriniz tanımlandıktan sonra, istekleri yukarıdaki sırada bir kez çalıştırabilir veya koleksiyonu çalıştırmak için Postman 'nın Çalıştırıcısı ' nı kullanabilirsiniz.

## <a name="change-the-key"></a>Anahtarı değiştirme

Anahtar değiştirildiğinde Medya Hizmetleri otomatik olarak algılanır.  Bunu test etmek için, aynı anahtar için başka bir anahtar sürümü oluşturun. Media Services bu anahtarı 15 dakikadan kısa bir süre içinde algılayabilmelidir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz kaynakları kullanmaya devam edemezseniz ve **faturalandırılmaya devam**etmek istemiyorsanız, bunları silin.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılacağını öğrenmek için sonraki makaleye ilerleyin...
> [!div class="nextstepaction"]
> [URL 'ye göre uzak bir dosya kodlayın ve REST ile videoyu akışla](stream-files-tutorial-with-rest.md)
