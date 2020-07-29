---
title: Microsoft Azure Data Box kendinden yönetilen kargo | Verilerdeki Microsoft Docs
description: Azure Data Box cihazlar için kendi kendine yönetilen teslim iş akışını açıklar
services: databox
author: priestlg
ms.service: databox
ms.subservice: disk
ms.topic: how-to
ms.date: 05/20/2020
ms.author: v-grpr
ms.openlocfilehash: 99c7021744467484656c52cf72378c6f553c3913
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84608596"
---
# <a name="use-self-managed-shipping-for-azure-data-box-in-the-azure-portal"></a>Azure portal Azure Data Box için kendi kendine yönetilen dağıtımı kullanın

Bu makalede, bir Azure Data Box cihazının sıralaması, çekilmesi ve kapatılması için kendi kendine yönetilen sevkıyat görevleri açıklanmaktadır. Data Box cihazını Azure portal kullanarak yönetebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

[Azure Data Box sipariş](data-box-deploy-ordered.md)ettiğinizde kendi kendine yönetilen sevkıyat bir seçenek olarak kullanılabilir. Otomatik olarak yönetilen kargo yalnızca aşağıdaki bölgelerde kullanılabilir:

* ABD Kamu
* Batı Avrupa
* Japonya
* Singapur
* Güney Kore

## <a name="use-self-managed-shipping"></a>Otomatik gönderimi kullanma

Data Box bir sıra yerleştirdiğinizde, kendi kendine yönetilen teslim seçeneğini belirleyebilirsiniz.

1. Azure Data Box sırasında, **kişi ayrıntıları**altında **+ Sevkiyat Adresi Ekle**' yi seçin.
   ![Kendi kendine yönetilen kargo](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-1.png)

2. Sevkiyat türü seçerken, **kendi kendine yönetilen sevkiyat** seçeneğini belirleyin. Bu seçenek yalnızca, Önkoşullar bölümünde açıklandığı gibi desteklenen bir bölgedeyse kullanılabilir.

3. Sevkiyat adresinizi girdikten sonra doğrulamanız ve siparişinizi doldurmanız gerekir.
   ![Kendi kendine yönetilen kargo](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-2.png)

4. Cihaz hazırlandıktan ve BT için bir e-posta bildirimi aldıktan sonra, bir toplama zamanlayabilirsiniz.

   Azure Data Box sırada **genel bakış** ' a gidin ve ardından **çekme zamanlaması**' nı seçin.

   ![Data Box cihazını toplama için sıralama](media\data-box-portal-customer-managed-shipping\data-box-portal-schedule-pickup-01.png)

5. **Azure Için zamanlama çekme**konusundaki yönergeleri izleyin.

   Yetkilendirme kodunuzu alabilmeniz için önce, [adbops@microsoft.com](mailto:adbops@microsoft.com) bölgenizin veri merkezinizden cihaz alımı zamanlamak üzere e-posta almalısınız.

   ![Toplamayı zamanlama](media\data-box-portal-customer-managed-shipping\data-box-portal-schedule-pickup-email-01.png)

6. Cihaz toplama 'yı zamanladıktan sonra, cihaz yetkilendirme kodunuzu **Azure Için zamanlama çekme planı** bölmesinde görüntüleyebilirsiniz.

   ![Yetkilendirme kodunuzu görüntüleme](media\data-box-portal-customer-managed-shipping\data-box-portal-auth-01b.png)

   Bu **Yetkilendirme kodunu**bir yere getirin. Güvenlik gereksinimlerine göre, zamanlama alma sırasında, çekme için ulaşan kişinin adının sağlanması gerekir...

   Ayrıca, toplama için veri merkezine kimlerin gönderileceğini de öğreneceksiniz. Siz veya iletişim noktası, veri merkezinde doğrulanacak bir kamu onaylı fotoğraf KIMLIĞI içermelidir.

   Ayrıca, cihazı alan kişinin da **Yetkilendirme koduna**sahip olması gerekir. Yetkilendirme kodu, veri merkezi 'nin çekme zamanında onaylanır.

7. Cihaz veri merkezinden alındıktan sonra siparişiniz otomatik olarak **çekme** durumuna getirilir.

    ![Teslim alındı](media\data-box-portal-customer-managed-shipping\data-box-portal-picked-up-boxed-01.png)

8. Cihaz çekildikten sonra, sitenizdeki Data Box verileri kopyalayın. Veri kopyalama işlemi tamamlandıktan sonra, Data Box göndermeye hazırlanabilirsiniz. Daha fazla bilgi için bkz. [göndermeye hazırlama](data-box-deploy-picked-up.md#prepare-to-ship).

   **Göndermeye hazırlama** adımının kritik bir hata olmadan tamamlaması gerekir, aksi takdirde, gerekli düzeltmeleri yaptıktan sonra bu adımı yeniden çalıştırmanız gerekir. Gönderimi hazırlama işlemi başarıyla tamamlandıktan sonra, cihazın yerel kullanıcı arabirimindeki bırakma için yetkilendirme kodu görüntüleyebilirsiniz.

   > [!NOTE]
   > Yetkilendirme kodunu e-posta üzerinden paylaşmayın. Bu, yalnızca bırakma sırasında veri merkezinde doğrulanmalıdır.

9. Bırakma için bir randevu aldıysanız, siparişin Azure portal **Azure veri merkezi durumunda teslim etmeye hazırlanıyor** olması gerekir. Cihazı döndürmek için **zamanlamayı zamanla** bölümündeki yönergeleri izleyin.

   ![Yetkilendirme kodunuzu görüntüleme](media\data-box-portal-customer-managed-shipping\data-box-portal-received-complete-02b.png)

10. KIMLIĞINIZ ve yetkilendirme kodunuz doğrulandıktan ve cihazı veri merkezinde devre dışı bıraktığınız zaman, sipariş durumunun **alınması**gerekir.

    ![Alınan Tamam](media\data-box-portal-customer-managed-shipping\data-box-portal-received-complete-01.png)

11. Cihaz alındıktan sonra veri kopyalama devam eder. Kopyalama tamamlandığında, sipariş tamamlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Box’ı kullanmaya başlayın](data-box-quickstart-portal.md)
