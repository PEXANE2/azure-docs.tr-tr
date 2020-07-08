---
title: Microsoft Azure Data Box Disk kendinden yönetilen kargo | Verilerdeki Microsoft Docs
description: Azure Data Box Disk cihazlar için kendi kendine yönetilen teslim iş akışını açıklar
services: databox
author: priestlg
ms.service: databox
ms.subservice: disk
ms.topic: how-to
ms.date: 05/20/2020
ms.author: v-grpr
ms.openlocfilehash: 81fc8de7e5a70ebb95e06a8e362cda41f8bfe1aa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84945027"
---
# <a name="use-self-managed-shipping-for-azure-data-box-disk-in-the-azure-portal"></a>Azure portal Azure Data Box Disk için kendi kendine yönetilen dağıtımı kullanın

Bu makalede, Azure Data Box Disk sipariş, çekme ve bırakma işlemleri için kendi kendine yönetilen sevkıyat görevleri açıklanmaktadır. Azure portal kullanarak Data Box Disk yönetebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

[Azure Data Box disk sipariş](data-box-disk-deploy-ordered.md)ettiğinizde kendi kendine yönetilen sevkıyat bir seçenek olarak kullanılabilir. Otomatik olarak yönetilen kargo yalnızca aşağıdaki bölgelerde kullanılabilir:

* ABD Kamu
* Batı Avrupa
* Japonya
* Singapur
* Güney Kore
* Hindistan (Önizleme)

## <a name="use-self-managed-shipping"></a>Otomatik gönderimi kullanma

Data Box Disk bir sıra yerleştirdiğinizde, kendi kendine yönetilen teslim seçeneğini belirleyebilirsiniz.

1. Azure Data Box Disk sırasında, **kişi ayrıntıları**altında **+ Sevkiyat Adresi Ekle**' yi seçin.

   ![Kendi kendine yönetilen kargo](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-1.png)

2. Sevkiyat türü seçerken, **kendi kendine yönetilen sevkiyat** seçeneğini belirleyin. Bu seçenek yalnızca, Önkoşullar bölümünde açıklandığı gibi desteklenen bir bölgedeyse kullanılabilir.

3. Sevkiyat adresinizi girdikten sonra doğrulamanız ve siparişinizi doldurmanız gerekir.

   ![Kendi kendine yönetilen kargo](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-2.png)

4. Cihaz hazırlandıktan ve bir e-posta bildirimi aldıktan sonra bir toplama zamanlayabilirsiniz. Azure Data Box Disk sırada **genel bakış** ' a gidin ve ardından **çekme zamanlaması**' nı seçin.

   ![Data Box cihazını toplama için sıralama](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-user-pickup-01b.png)

5. **Azure Için zamanlama çekme**konusundaki yönergeleri izleyin. Yetkilendirme kodunuzu alabilmeniz için önce, [adbops@microsoft.com](mailto:adbops@microsoft.com) bölgenizin veri merkezinizden cihaz alımı zamanlamak üzere e-posta almalısınız.

   ![Toplamayı zamanlama](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-user-pickup-02c.png)

6. Cihaz toplama 'yı zamanladıktan sonra, yetkilendirme kodunuzu **Azure Için zamanlama çekme**penceresinde görüntüleyebileceksiniz.

   ![Yetkilendirme kodunuzu görüntüleme](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-01b.png)

   Bu **Yetkilendirme kodunu**bir yere getirin.

   Güvenlik gereksinimlerine göre, zamanlama alma sırasında, çekme işlemi için verilecek kişinin adının sağlanması gerekir ve bu, alma işlemi için ulaşacaktır.

   Ayrıca, toplama için veri merkezine kimlerin gönderileceğini de öğreneceksiniz. Siz veya iletişim noktası, veri merkezinde doğrulanacak bir kamu onaylı fotoğraf KIMLIĞI içermelidir.

   Ayrıca, cihazı alan kişinin da **Yetkilendirme koduna**sahip olması gerekir. Yetkilendirme kodu, bir toplama veya bırakma için benzersizdir ve veri merkezinde onaylanır.

7. Cihaz veri merkezinden alındıktan sonra siparişiniz otomatik olarak **çekme** durumuna getirilir.

   ![Teslim alındı](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-ready-disk-01b.png)

8. Cihaz çekildikten sonra, sitenizdeki Data Box Disk verileri kopyalayabilirsiniz. Veri kopyalama işlemi tamamlandıktan sonra, Data Box Disk göndermeye hazırlanabilirsiniz.

   Veri kopyalamayı tamamladıktan sonra, bırakma için bir randevu zamanlamak üzere işlemlere başvurmanız gerekir. Diskleri bırakmak için veri merkezine gelen kişinin ayrıntılarını paylaşmanız gerekir. Ayrıca, veri merkezi 'nin onay sırasında yetkilendirme kodunu doğrulaması gerekir. Bırakma için yetkilendirme kodu, **zamanlama bırakma**bölümünün altında Azure Portal kullanılabilir.

   > [!NOTE]
   > Yetkilendirme kodunu e-posta üzerinden paylaşmayın. Bu, yalnızca bırakma sırasında veri merkezinde doğrulanmalıdır.

9. Bırakma için bir randevu aldıysanız, siparişin artık Azure portal **Azure veri merkezi** durumunda alınması hazır olmalıdır.

   ![Yetkilendirme kodunuzu görüntüleme](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-dropoff-02b.png)

10. KIMLIĞINIZ ve yetkilendirme kodunuz doğrulandıktan ve cihazı veri merkezinde devre dışı bıraktığınız zaman, sipariş durumunun **alınması**gerekir.

    ![Alınan Tamam](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-received-01a.png)

11. Cihaz alındıktan sonra veri kopyalama devam eder. Kopyalama tamamlandığında, sipariş tamamlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Azure portal kullanarak Azure Data Box Disk dağıtma](data-box-disk-quickstart-portal.md)
