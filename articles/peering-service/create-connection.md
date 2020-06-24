---
title: 'Azure eşleme hizmeti: oluşturma '
description: Bu öğreticide Azure eşleme hizmetini ve bir ön eki kaydetmeyi öğrenin.
services: peering-service
author: derekolo
ms.service: peering-service
ms.topic: tutorial
ms.date: 05/2/2020
ms.author: derekol
Customer intent: With Azure Peering service enhancing the customer connectivity to Microsoft cloud services .
ms.openlocfilehash: f04ae0d2e541be0a7984e944298fa8d7b47fd126
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "84872704"
---
# <a name="tutorial-create-a-peering-service-connection"></a>Öğretici: eşleme hizmeti bağlantısı oluşturma

Bu öğreticide, bir eşleme hizmeti kaynağı oluşturma ve eşleme hizmeti bağlantısının nasıl yapılandırılacağı gösterilir. 

1. Bir eşleme hizmeti bağlantısı kaydetmek için **kaynak**  >  **eşleme hizmeti**oluştur ' u seçin.

 
    ![Eşleme Hizmeti'ni kaydetme](./media/peering-service-portal/peering-servicecreate.png)

2. **Eşleme hizmeti bağlantısı oluşturma** sayfasındaki **temel bilgiler** sekmesinde aşağıdaki ayrıntıları girin.
 
3. Abonelikle ilişkili aboneliği ve kaynak grubunu seçin.

    ![Eşleme hizmeti temel sekmesini Kaydet](./media/peering-service-portal/peering-servicebasics.png)

4. Eşleme hizmeti örneğinin kaydedilmesi gereken bir **ad** girin.

5. Şimdi, sayfanın altındaki **Sonraki: yapılandırma** düğmesini seçin. **Yapılandırma** sayfası görüntülenir.
## <a name="configure-the-peering-service-connection"></a>Eşleme hizmeti bağlantısını yapılandırma

1. **Yapılandırma** sayfasında, eşleme hizmeti **konumu** açılan listesinden aynı ' ı seçerek eşleme hizmetinin etkinleştirilmesi gereken konumu seçin.

1. Eşleme hizmeti **sağlayıcısı** açılır listesinden bir sağlayıcı adı seçerek eşleme hizmetinin alınması gereken hizmet sağlayıcısını seçin.
 
1. Ön **ekler** bölümünün alt kısmındaki **Yeni ön ek oluştur** ' u seçin ve metin kutuları görüntülenir. Şimdi önek kaynağının adını ve hizmet sağlayıcısıyla ilişkili önekleri girin.

1. **Önek anahtarı** ' nı seçin ve SAĞLAYıCıNıZ (ISS veya ixp) tarafından size verilen önek anahtarını ekleyin. Bu anahtar, MS 'nin IP ön ekine ayrılan ön eki ve sağlayıcıyı doğrulamasını sağlar.

    ![Eşleme hizmeti yapılandırma sekmesini Kaydet](./media/peering-service-portal/peering-serviceconfiguration.png)

1. Sayfanın sol alt kısmındaki **gözden geçir + oluştur** düğmesini seçin. **Gözden geçir + oluştur** sayfası görünür ve Azure yapılandırmanızı doğrular.

 1. Gösterildiği gibi **doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

> ![Eşleme hizmeti yapılandırma sekmesini Kaydet](./media/peering-service-portal/peering-service-prefix.png)

1. Bir eşleme hizmeti bağlantısını kaydettikten sonra, dahil edilen öneklere ek doğrulama gerçekleştirilir. Doğrulama durumunu kaynak adının **önekler** bölümünde gözden geçirebilirsiniz. Doğrulama başarısız olursa, aşağıdaki hata iletilerinden biri görüntülenir:

   - Geçersiz eşleme hizmeti ön eki, ön ek geçerli biçim olmalıdır, yalnızca IPv4 öneki desteklenir.
   - Eşleme hizmet sağlayıcısından önek alınmadı.
   - Ön ek duyurusu geçerli bir BGP topluluğuna sahip değil, lütfen eşleme hizmeti sağlayıcısına başvurun.
   - Yedekleme yolu bulunamadı, lütfen eşleme hizmeti sağlayıcısına başvurun.
   - Yol olarak daha uzun süre alınan önek, lütfen eşleme hizmeti sağlayıcısına başvurun.
   - Bu önek, yolda olduğu gibi özel olarak alındı, lütfen eşleme hizmeti sağlayıcısına başvurun.

### <a name="add-or-remove-a-prefix"></a>Ön ek ekleme veya kaldırma

Ön ekler eklemek **için ön ekler sayfasında** ön ekler **Ekle** ' yi seçin.

Listelenen önek ' in yanındaki üç nokta (...) simgesini seçin ve **Sil** seçeneğini belirleyin.

### <a name="delete-a-peering-service-connection"></a>Bir eşleme hizmeti bağlantısını silme

**Tüm kaynaklar** sayfasında, eşleme hizmetindeki onay kutusunu seçin ve sayfanın en üstündeki **Sil** seçeneğini belirleyin.
## <a name="next-steps"></a>Sonraki adımlar

- Eşleme hizmeti bağlantısı hakkında bilgi edinmek için bkz. [eşleme hizmeti bağlantısı](connection.md).
- Eşleme hizmeti bağlantı telemetrisi hakkında bilgi edinmek için bkz. [eşleme hizmeti bağlantı telemetrisi](connection-telemetry.md).
- Telemetriyi ölçmek için bkz. [ölçüm bağlantı telemetrisi](measure-connection-telemetry.md).
- Azure PowerShell kullanarak bağlantıyı kaydetmek için bkz. [eşleme hizmeti bağlantısını kaydetme-Azure PowerShell](powershell.md).
- Azure CLı kullanarak bağlantıyı kaydetmek için bkz. [eşleme hizmeti bağlantısı kaydetme-Azure CLI](cli.md).