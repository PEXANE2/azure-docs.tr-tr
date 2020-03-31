---
title: Azure Veri Gölü Depolama Gen1'de Şifreleme | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1'deki şifreleme, verilerinizi korumanıza, kurumsal güvenlik ilkeleri uygulamanıza ve mevzuata uygunluk gereksinimlerini karşılamanıza yardımcı olur. Bu makale tasarıma genel bir bakış sunarken uygulamanın birkaç teknik yönünü ele almaktadır.
services: data-lake-store
documentationcenter: ''
author: esung22
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: yagupta
ms.openlocfilehash: a009f212bd8baaa353d602dc6090aeeccddd4936
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60878451"
---
# <a name="encryption-of-data-in-azure-data-lake-storage-gen1"></a>Azure Veri Gölü Depolama Gen1'de verilerin şifrelemesi

Azure Veri Gölü Depolama Gen1'deki şifreleme, verilerinizi korumanıza, kurumsal güvenlik ilkeleri uygulamanıza ve mevzuata uygunluk gereksinimlerini karşılamanıza yardımcı olur. Bu makale tasarıma genel bir bakış sunarken uygulamanın birkaç teknik yönünü ele almaktadır.

Data Lake Storage Gen1, hem istirahatte hem de aktarım sırasında verilerin şifresini destekler. Veri istirahati için, Veri Gölü Depolama Gen1 "varsayılan olarak açık" saydam şifrelemeyi destekler. Bu terimlerin biraz daha ayrıntılı olarak anlamı şudur:

* **Varsayılan olarak**: Yeni bir Veri Gölü Depolama Gen1 hesabı oluşturduğunuzda, varsayılan ayar şifreleme sağlar. Bundan sonra, Veri Gölü Depolama Gen1'de depolanan veriler, kalıcı ortamlarda depolamadan önce her zaman şifrelenir. Bu durum tüm veriler için geçerlidir ve bir hesap oluşturulduktan sonra değiştirilemez.
* **Saydam**: Data Lake Storage Gen1, kalıcı olmadan önce verileri otomatik olarak şifreler ve verileri almadan önce şifreyi çözer. Şifreleme, Veri Gölü Depolama Gen1 hesap düzeyinde bir yönetici tarafından yapılandırılır ve yönetilir. Veri erişimi API'lerinde hiçbir değişiklik yapılmaz. Bu nedenle, şifreleme nedeniyle Veri Gölü Depolama Gen1 ile etkileşim edebilen uygulama ve hizmetlerde değişiklik gerekmez.

Aktarımdaki veriler (hareket halindeki veri olarak da bilinir) Veri Gölü Depolama Gen1'de her zaman şifrelenir. Kalıcı medyaya depolama önce veri şifrelemeye ek olarak, aktarımdaki veriler de her zaman HTTPS kullanılarak korunmaktadır. HTTPS, Veri Gölü Depolama Gen1 REST arabirimleri için desteklenen tek protokoldür. Aşağıdaki diyagram, Veri Gölü Depolama Gen1'de verilerin nasıl şifrelenerek şifrelenir hale olduğunu gösterir:

![Veri Gölü Depolama Gen1 veri şifreleme diyagramı](./media/data-lake-store-encryption/fig1.png)


## <a name="set-up-encryption-with-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1 ile şifreleme ayarlama

Veri Gölü Depolama Gen1 için Şifreleme hesap oluşturma sırasında ayarlanır ve her zaman varsayılan olarak etkinleştirilir. Anahtarları kendiniz yönetebilir veya Veri Gölü Depolama Gen1'in sizin için yönetmesine izin verebilirsiniz (bu varsayılan değerdir).

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal)

## <a name="how-encryption-works-in-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'de şifreleme nasıl çalışır?

Aşağıdaki bilgiler ana şifreleme anahtarlarının nasıl yönetilecek olduğunu kapsar ve Veri Gölü Depolama Gen1 için veri şifrelemesinde kullanabileceğiniz üç farklı anahtar türünü açıklar.

### <a name="master-encryption-keys"></a>Ana şifreleme anahtarları

Veri Gölü Depolama Gen1 ana şifreleme anahtarları (MEKs) yönetimi için iki mod sağlar. Şimdilik, ana şifreleme anahtarının en üst düzey anahtar olduğunu varsayabiliriz. Veri Gölü Depolama Gen1'de depolanan tüm verilerin şifresini çözmek için ana şifreleme anahtarına erişim gereklidir.

Ana şifreleme anahtarını yönetmek için kullanılan iki mod şunlardır:

*   Hizmet tarafından yönetilen anahtarlar
*   Müşteri tarafından yönetilen anahtarlar

Her iki modda da ana şifreleme anahtarı Azure Key Vault’ta depolanarak güvenlik altına alınır. Azure üzerinde tam olarak yönetilen, yüksek güvenlikli bir hizmet olan Key Vault, şifreleme anahtarlarını korumak için kullanılabilir. Daha fazla bilgi için bkz. [Key Vault](https://azure.microsoft.com/services/key-vault).

MEK’leri yönetmek için kullanılan iki modun sağladığı özelliklerin kısa bir karşılaştırması aşağıdadır.

|  | Hizmet tarafından yönetilen anahtarlar | Müşteri tarafından yönetilen anahtarlar |
| --- | --- | --- |
|Veriler nasıl depolanır?|Depolanmadan önce her zaman şifrelenir.|Depolanmadan önce her zaman şifrelenir.|
|Ana Şifreleme Anahtarı nerede depolanır?|Key Vault|Key Vault|
|Key Vault dışında açıkta saklanan şifreleme anahtarı var mı? |Hayır|Hayır|
|Key Vault’tan MEK alınabilir mi?|Hayır. MEK Key Vault’ta depolandıktan sonra yalnızca şifreleme ve şifre çözme amacıyla kullanılabilir.|Hayır. MEK Key Vault’ta depolandıktan sonra yalnızca şifreleme ve şifre çözme amacıyla kullanılabilir.|
|Key Vault örneği ve MEK kime aittir?|Veri Gölü Depolama Gen1 hizmeti|Kendi Azure aboneliğiniz kapsamında bulunan Key Vault örneği size aittir. Key Vault’taki MEK, yazılım veya donanım tarafından yönetilebilir.|
|Data Lake Storage Gen1 hizmeti için MEK'e erişimi iptal edebilir misiniz?|Hayır|Evet. Key Vault'taki erişim denetim listelerini yönetebilir ve Veri Gölü Depolama Gen1 hizmetinin hizmet kimliğine erişim denetimi girişlerini kaldırabilirsiniz.|
|MEK’i kalıcı olarak silebilir misiniz?|Hayır|Evet. MEK'i Key Vault'tan silerseniz, Veri Gölü Depolama Gen1 hesabındaki veriler, Data Lake Storage Gen1 hizmeti de dahil olmak üzere hiç kimse tarafından çözülemez. <br><br> MEK’i Key Vault'tan silmeden önce özellikle yedeklediyseniz, MEK geri yüklenebilir ve veriler kurtarılabilir. Ancak, KEY Vault'tan silmeden önce MEK'i yedeklemediyseniz, Veri Gölü Depolama Gen1 hesabındaki veriler bundan sonra hiçbir zaman şifreyi çözemez.|


MEK ve MEK’in içinde bulunduğu Key Vault’un kim tarafından yönetildiği konusundaki bu farklılık dışında, tasarımın geri kalanı her iki mod için aynıdır.

Ana şifreleme anahtarları için modu seçtiğinizde aşağıdakileri unutmamanız gerekir:

*   Bir Veri Gölü Depolama Gen1 hesabı sağlarken müşteri yönetilen anahtarları veya hizmet yönetilen anahtarları kullanıp kullanmamayı seçebilirsiniz.
*   Bir Veri Gölü Depolama Gen1 hesabı sağlandıktan sonra mod değiştirilemez.

### <a name="encryption-and-decryption-of-data"></a>Verilerin şifrelenmesi ve şifresinin çözülmesi

Veri şifreleme tasarımında kullanılan üç tür anahtar vardır. Aşağıdaki tabloda bir özet verilmektedir:

| Anahtar                   | Kısaltma | İlişkili olduğu yer: | Depolama konumu                             | Tür       | Notlar                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Ana Şifreleme Anahtarı | MEK          | Veri Gölü Depolama Gen1 hesabı | Key Vault                              | Asimetrik | Veri Gölü Depolama Gen1 veya siz tarafından yönetilebilir.                                                              |
| Veri Şifreleme Anahtarı   | DEK          | Veri Gölü Depolama Gen1 hesabı | Veri Gölü Depolama Gen1 hizmeti tarafından yönetilen kalıcı depolama | Simetrik  | DEK, MEK ile şifrelenir. Şifrelenmiş DEK, kalıcı medyada depolanır. |
| Blok Şifreleme Anahtarı  | BEK          | Bir veri bloğu | None                                         | Simetrik  | BEK, DEK’ten ve veri bloğundan türetilir.                                                      |

Aşağıdaki diyagram bu kavramları göstermektedir:

![Veri şifrelemesindeki anahtarlar](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-to-be-decrypted"></a>Bir dosyanın şifresinin çözülmesi için kullanılan genel algoritma:
1.  Veri Gölü Depolama Gen1 hesabı için DEK önbelleğe alınmış ve kullanıma hazır olup olmadığını kontrol edin.
    - Bu koşulları karşılamıyorsa, kalıcı depolamadan şifrelenmiş DEK’i okuyun ve şifresinin çözülmesi için Key Vault’a gönderin. Şifresi çözülmüş DEK’i bellekte önbelleğe alın. Artık kullanıma hazırdır.
2.  Dosyadaki her veri bloğu için:
    - Kalıcı depolama alanından şifrelenmiş veri bloğu okunur.
    - DEK’ten ve şifrelenmiş veri bloğundan BEK oluşturulur.
    - Verilerin şifresini çözmek için BEK kullanılır.


#### <a name="pseudo-algorithm-when-a-block-of-data-is-to-be-encrypted"></a>Bir veri bloğu şifrelenecek olduğunda genel algoritma:
1.  Veri Gölü Depolama Gen1 hesabı için DEK önbelleğe alınmış ve kullanıma hazır olup olmadığını kontrol edin.
    - Bu koşulları karşılamıyorsa, kalıcı depolamadan şifrelenmiş DEK’i okuyun ve şifresinin çözülmesi için Key Vault’a gönderin. Şifresi çözülmüş DEK’i bellekte önbelleğe alın. Artık kullanıma hazırdır.
2.  DEK’ten veri bloğu için benzersiz bir BEK oluşturulur.
3.  Veri bloğu, BEK ile AES-256 şifreleme kullanılarak şifrelenir.
4.  Şifrelenmiş veri bloğu kalıcı depolama alanında depolanır.

> [!NOTE] 
> DEK her zaman, kalıcı medyada veya bellekte önbelleğe alınmış olarak MEK tarafından şifrelenmiş olarak depolanır.

## <a name="key-rotation"></a>Anahtar döndürme

Müşteri tarafından yönetilen anahtarları kullanırken MEK’i döndürebilirsiniz. Müşteri tarafından yönetilen anahtarlarla bir Veri Gölü Depolama Gen1 hesabının nasıl ayarlanılabildiğini öğrenmek için [başlarken](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal)bkz.

### <a name="prerequisites"></a>Ön koşullar

Veri Gölü Depolama Gen1 hesabını ayarladığınızda, kendi anahtarlarınızı kullanmayı seçmiş siniz. Bu seçenek, hesap oluşturulduktan sonra değiştirilemez. Aşağıdaki adımlarda, müşteri tarafından yönetilen anahtarlar kullandığınız (Key Vault'tan kendi anahtarlarınızı seçtiğiniz) varsayılır.

Şifreleme için varsayılan seçenekleri kullanırsanız, verileriniz her zaman Data Lake Storage Gen1 tarafından yönetilen anahtarlar kullanılarak şifrelenir. Bu seçenekte, Veri Gölü Depolama Gen1 tarafından yönetildiği için anahtarları döndürme olanağınız yoktur.

### <a name="how-to-rotate-the-mek-in-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'de MEK nasıl döndürülür?

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Veri Gölü Depolama Gen1 hesabınızla ilişkili anahtarlarınızı depolayan Key Vault örneğine göz atın. **Anahtarlar**’ı seçin.

    ![Key Vault ekran görüntüsü](./media/data-lake-store-encryption/keyvault.png)

3. Veri Gölü Depolama Gen1 hesabınızla ilişkili anahtarı seçin ve bu anahtarın yeni bir sürümünü oluşturun. Veri Gölü Depolama Gen1'in şu anda yalnızca anahtarın yeni bir sürümüne anahtar döndürmeyi desteklediğini unutmayın. Farklı bir anahtara döndürmeyi desteklemez.

   ![Yeni Sürümün vurgulandığı Anahtarlar penceresi ekran görüntüsü](./media/data-lake-store-encryption/keynewversion.png)

4. Veri Gölü Depolama Gen1 hesabına göz atın ve **Şifreleme'yi**seçin.

   ![Şifreleme vurgulanmış Veri Gölü Depolama Gen1 hesap penceresinin ekran görüntüsü](./media/data-lake-store-encryption/select-encryption.png)

5. Yeni bir anahtar sürümünün mevcut olduğu bir ileti ile bildirilir. Yeni sürümün anahtarını güncelleştirmek için **Anahtarı Döndür'e** tıklayın.

   ![İleti ve Döndürme Anahtarı vurgulanan Veri Gölü Depolama Gen1 penceresinin ekran görüntüsü](./media/data-lake-store-encryption/rotatekey.png)

Bu işlem iki dakikadan kısa sürer ve anahtar döndürme nedeniyle beklenen kapalı kalma süresi yoktur. İşlem tamamlandıktan sonra anahtarın yeni sürümü kullanılır.

> [!IMPORTANT]
> Anahtar döndürme işlemi tamamlandıktan sonra anahtarın eski sürümü artık verilerinizi şifrelemek için etkin şekilde kullanılmaz.  Ancak verilerinizin yedek kopyalarının etkilendiği nadiren de olsa karşılaşılan beklenmedik hata durumlarında veriler halen eski anahtarı kullanan bir yedeklemeden geri yüklenebilir. Verilerinizin bu tür nadir durumlarda erişilebilir olmasını sağlamak için, şifreleme anahtarınızın önceki sürümünün bir kopyasını saklayın. Olağanüstü durum kurtarma planlamanız için en iyi uygulamalar için [Veri Gölü Depolama Gen1'deki veriler](data-lake-store-disaster-recovery-guidance.md) için Olağanüstü Durum kurtarma kılavuzuna bakın. 