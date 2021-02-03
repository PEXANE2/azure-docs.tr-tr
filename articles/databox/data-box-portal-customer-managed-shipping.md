---
title: Microsoft Azure Data Box kendinden yönetilen kargo | Verilerdeki Microsoft Docs
description: Azure Data Box cihazlar için kendi kendine yönetilen teslim iş akışını açıklar
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: how-to
ms.date: 02/02/2021
ms.author: alkohli
ms.openlocfilehash: 07529b18191c71776a9a36edbfa4cfd8ded5af4f
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99524558"
---
# <a name="use-self-managed-shipping-for-azure-data-box-in-the-azure-portal"></a>Azure portal Azure Data Box için kendi kendine yönetilen dağıtımı kullanın

Bu makalede, bir Azure Data Box cihazının sıralaması, çekilmesi ve kapatılması için kendi kendine yönetilen sevkıyat görevleri açıklanmaktadır. Data Box cihazını Azure portal kullanarak yönetebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

[Azure Data Box sipariş](data-box-deploy-ordered.md)ettiğinizde kendi kendine yönetilen sevkıyat bir seçenek olarak kullanılabilir. Otomatik olarak yönetilen kargo yalnızca aşağıdaki bölgelerde kullanılabilir:

* ABD Kamu
* Birleşik Krallık
* Batı Avrupa
* Japonya
* Singapur
* Güney Kore
* Hindistan
* Güney Afrika
* Avustralya

## <a name="use-self-managed-shipping"></a>Otomatik gönderimi kullanma

Data Box bir sıra yerleştirdiğinizde, kendi kendine yönetilen teslimat seçeneğini belirleyebilirsiniz.

1. Azure Data Box sırasında, **kişi ayrıntıları** altında **+ Sevkiyat Adresi Ekle**' yi seçin.
 
   ![Kendi kendine yönetilen kargo, sevkiyat adresi ekleme](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-1.png)

2. Bir sevkiyat türü seçerken, **kendi kendine yönetilen sevkiyat** seçeneğini belirleyin. Bu seçenek yalnızca, Önkoşullar bölümünde açıklandığı gibi desteklenen bir bölgedeyse kullanılabilir.

3. Sevkiyat adresinizi sağlamadıktan sonra doğrulamanız ve siparişinizi doldurmanız gerekir.

   ![Otomatik olarak yönetilen sevkıyat, adresi doğrulama ve ekleme](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-2.png)

4. Cihaz hazırlandıktan ve BT için bir e-posta bildirimi aldıktan sonra, bir toplama zamanlayabilirsiniz.

   Azure Data Box sırada **genel bakış** ' a gidin ve ardından **çekme zamanlaması**' nı seçin.

   ![Data Box sırası, zamanlama alma seçeneği](media\data-box-portal-customer-managed-shipping\data-box-portal-schedule-pickup-01.png)

5. **Azure Için zamanlama çekme** konusundaki yönergeleri izleyin.

   Yetkilendirme kodunuzu alabilmeniz için önce, [adbops@microsoft.com](mailto:adbops@microsoft.com) bölgenizin veri merkezinizden cihaz alımı zamanlamak üzere e-posta almalısınız.

   ![Azure yönergeleri için çekme zamanlaması](media\data-box-portal-customer-managed-shipping\data-box-portal-schedule-pickup-email-01.png)

6. Cihaz piara hesabınızı zamanladıktan sonra, cihaz yetkilendirme kodunuzu **Azure Için zamanlama alma** bölmesinde görüntüleyebilirsiniz.

   ![Cihaz yetkilendirme kodunuzu görüntüleme](media\data-box-portal-customer-managed-shipping\data-box-portal-auth-01b.png)

   Bu **Yetkilendirme kodunu** bir yere getirin. Her güvenlik gereksinimi için, planlama sırasında çekme sırasında, çekme için ulaşan kişinin adını sunmak gerekir.

   Ayrıca, toplama için veri merkezine kimlerin gönderileceğini de öğreneceksiniz. Siz veya iletişim noktası, veri merkezinde doğrulanacak bir kamu onaylı fotoğraf KIMLIĞI içermelidir.

   Ayrıca, cihazı alan kişinin da **Yetkilendirme koduna** sahip olması gerekir. Yetkilendirme kodu, veri merkezi 'nin çekme zamanında onaylanır.

7. Cihaz veri merkezinden alındıktan sonra siparişiniz otomatik olarak **çekme** durumuna getirilir.

    ![Çekme durumunda bir sipariş](media\data-box-portal-customer-managed-shipping\data-box-portal-picked-up-boxed-01.png)

8. Cihaz çekildikten sonra, sitenizdeki Data Box verileri kopyalayın. Veri kopyalama işlemi tamamlandıktan sonra, Data Box göndermeye hazırlanabilirsiniz. Daha fazla bilgi için bkz. [göndermeye hazırlama](data-box-deploy-picked-up.md#prepare-to-ship).

   **Göndermeye hazırlama** adımının herhangi bir kritik hata olmadan tamamlaması gerekir. Aksi takdirde, gerekli düzeltmeleri yaptıktan sonra bu adımı yeniden çalıştırmanız gerekir. **Göndermeye hazırlama** adımı başarıyla tamamlandıktan sonra, cihaz yerel kullanıcı arabirimindeki bırakma için yetkilendirme kodunu görüntüleyebilirsiniz.

   > [!NOTE]
   > Yetkilendirme kodunu e-posta üzerinden paylaşmayın. Bu, yalnızca bırakma sırasında veri merkezinde doğrulanmalıdır.

9. Bırakma için bir randevu aldıysanız, siparişin Azure portal **Azure veri merkezi durumunda almaya hazırız** gerekir. Cihazı döndürmek için **zamanlamayı zamanla** bölümündeki yönergeleri izleyin.

   ![Cihaz bırakma yönergeleri](media\data-box-portal-customer-managed-shipping\data-box-portal-received-complete-02b.png)

10. KIMLIĞINIZ ve yetkilendirme kodunuz doğrulandıktan ve cihazı veri merkezinde devre dışı bıraktığınız zaman, sipariş durumu **alınmalıdır**.

    ![Alındı durumuna sahip bir sıra](media\data-box-portal-customer-managed-shipping\data-box-portal-received-complete-01.png)

11. Cihaz alındıktan sonra veri kopyalama devam eder. Kopyalama tamamlandığında, sipariş tamamlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Box’ı kullanmaya başlayın](data-box-quickstart-portal.md)
