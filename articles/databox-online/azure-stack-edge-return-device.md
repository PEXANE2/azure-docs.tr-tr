---
title: Azure Stack Edge cihazınızı döndürün veya değiştirin | Microsoft Docs
description: Azure Stack Edge cihazının nasıl dönebileceğinizi veya değiştirileceğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/26/2020
ms.author: alkohli
ms.openlocfilehash: 6e0581912a619bec11be6d322e2987a4498c5170
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84339374"
---
# <a name="return-or-replace-your-azure-stack-edge-device"></a>Azure Stack Edge cihazınızı döndürme veya değiştirme

Bu makalede, verileri silme ve sonra Azure Stack Edge cihazınızı döndürme açıklanmaktadır. Cihazı geri aldıktan sonra, cihazla ilişkili kaynağı da silebilir veya bir değiştirme cihazını sipariş edebilirsiniz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Cihazdaki veri disklerinin verileri silme
> * Cihazınızı döndürmek için bir destek bileti açın
> * Cihazı paketleme ve bir toplama zamanlama
> * Azure portal kaynağı silme
> * Yeni bir cihaz alma

## <a name="erase-data-from-the-device"></a>Cihazdan verileri silme

Cihazınızın veri disklerinin verilerini silmek için cihazınızı sıfırlamanız gerekir. Yerel Web Kullanıcı arabirimini veya PowerShell arabirimini kullanarak cihazınızı sıfırlayabilirsiniz.

Sıfırlamadan önce, gerekirse, cihazdaki yerel verilerin bir kopyasını oluşturun. Cihazdan verileri bir Azure depolama kapsayıcısına kopyalayabilirsiniz.

Yerel Web Kullanıcı arabirimini kullanarak cihazınızı sıfırlamak için aşağıdaki adımları uygulayın.

1. Yerel Web Kullanıcı arabiriminde **bakım > cihaz sıfırlaması**' na gidin.
2. **Cihazı Sıfırla**' yı seçin.

    ![Cihaz sıfırlama](media/azure-stack-edge-return-device/device-reset-1.png)

3. Onay istendiğinde, uyarıyı gözden geçirin ve devam etmek için **Evet** ' i seçin.

    ![Sıfırlamayı Onayla](media/azure-stack-edge-return-device/device-reset-2.png)  

Sıfırlama, verileri cihaz veri disklerinin dışına siler. Bu işlem, cihazınızdaki veri miktarına bağlı olarak yaklaşık 30-40 dakika sürer.

Alternatif olarak, cihazın PowerShell arabirimine bağlanın ve `Reset-HcsAppliance` veri disklerinden verileri silmek için cmdlet 'ini kullanın. Daha fazla bilgi için bkz. [cihazınızı sıfırlama](azure-stack-edge-connect-powershell-interface.md#reset-your-device).

> [!NOTE]
> - Yeni bir cihaza değiş tokuş yapıyorsanız veya yükseltiyorsanız, Cihazınızı yalnızca yeni cihazı aldıktan sonra sıfırlamanız önerilir.
> - Cihaz sıfırlaması yalnızca cihazdaki tüm yerel verileri siler. Buluttaki veriler silinmez ve [ücretleri](https://azure.microsoft.com/pricing/details/storage/)toplar. Bu verilerin, [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)gibi bir bulut depolama yönetim aracı kullanılarak ayrı olarak silinmesi gerekir.

## <a name="open-a-support-ticket"></a>Bir destek bileti açın

Geri dönüş işlemini başlatmak için aşağıdaki adımları uygulayın.

1. Cihazı döndürmek istediğinizi belirten Microsoft Desteği bir destek bileti açın. **Azure Stack Edge donanımı**olarak sorun türünü seçin.

    ![Destek bileti açma](media/azure-stack-edge-return-device/open-support-ticket-1.png)  

2. Bir Microsoft Desteği mühendisi sizinle iletişim kuracaktır. Sevkıyat ayrıntılarını belirtin.
3. İade sevk etme kutusuna ihtiyacınız varsa, isteği isteyebilirsiniz. Sorunun **geri dönmesi Için** **Evet** yanıtını yazın.


## <a name="schedule-a-pickup"></a>Çekme zamanlaması

1. Cihazı kapatın. Yerel Web Kullanıcı arabiriminde **bakım > güç ayarları**' na gidin.
2. **Kapat ' ı**seçin. Onay istendiğinde, devam etmek için **Evet** ' e tıklayın. Daha fazla bilgi için bkz. [güç yönetimi](data-box-gateway-manage-access-power-connectivity-mode.md#manage-power).
3. Güç kablolarını çıkarın ve cihazdaki tüm ağ kablolarını kaldırın.
4. Kendi kutusunu veya Azure 'dan aldığınız boş kutuyu kullanarak sevkiyat paketini hazırlayın. Cihazı ve cihazla birlikte gelen güç gücünü kutuya yerleştirin.
5. Paket üzerinde Azure 'dan aldığınız sevkiyat etiketini AFBir şekilde düzeltir.
6. Bölgenizdeki taşıyıcıdan bir kargo toplama randevusu alın. Cihaz bıze döndürülürken, taşıyıcınızın UPS veya FedEx olması mümkündür. KESINTISIZ bir çekme planı zamanlamak için:

    1. Yerel UPS şubesini (ülkeye/bölgeye özel ücretsiz hatlar) arayın.
    2. Çağrınızda, basılı etiketinizde gösterildiği gibi ters Sevkiyat izleme numarasını tırnak içine edin.
    3. İzleme numarası tırnak işareti yoksa, UPS toplama sırasında ek ücret ödemeniz gerekir.

    Toplama zamanlamak yerine, en yakın bırakma konumundaki Azure Stack ucunu da kapatabilirsiniz.

## <a name="delete-the-resource"></a>Kaynağı Sil

Cihaz Azure veri merkezinde alındıktan sonra, cihaz hasar veya herhangi bir değişiklik işareti için denetlenir.

- Cihaz bozulmadan ve iyi bir şekle ulaştığında, faturalandırma ölçümü o kaynak için duraklar. Microsoft Desteği, cihazın döndürüldüğünden emin olmak için sizinle iletişim kuracaktır. Daha sonra Azure portal cihazla ilişkili kaynağı silebilirsiniz.
- Cihaz önemli ölçüde hasar görmüşse, cezaları uygulanabilir. Ayrıntılar için bkz. [kayıp veya hasarlı cihaz](https://azure.microsoft.com/pricing/details/databox/edge/) ve [ürün hizmet koşulları](https://www.microsoft.com/licensing/product-licensing/products)hakkında SSS.  


Azure portal cihazı silebilirsiniz:
-    Siparişi yerleştirdikten ve cihaz Microsoft tarafından hazırlanmadan önce.
-    Cihazı Microsoft 'a döndürdüyseniz, Azure veri merkezinde fiziksel incelemeyi geçirir ve cihazın döndürüldüğünden emin olmak için çağrılar Microsoft Desteği.

Cihazı başka bir abonelik veya konuma karşı etkinleştirdiyseniz, Microsoft siparişinizi bir iş günü içinde yeni abonelik veya konuma taşır. Sipariş taşındıktan sonra bu kaynağı silebilirsiniz.


Azure portal, cihazı ve kaynağı silmek için aşağıdaki adımları uygulayın.

1. Azure portal, kaynağına gidin ve **genel bakış**' a bakın. Komut çubuğundan **Sil**' i seçin.

    ![Sil ' i seçin](media/azure-stack-edge-return-device/delete-resource-1.png)

2. **Cihazı Sil** dikey penceresinde, silmek istediğiniz cihazın adını yazın ve **Sil**' i seçin.

    ![Silmeyi onayla](media/azure-stack-edge-return-device/delete-resource-2.png)

Cihaz ve ilişkili kaynak başarıyla silindikten sonra size bildirimde bulunuluyoruz.

## <a name="get-a-replacement-device"></a>Yeni bir cihaz alma

Mevcut cihazda bir donanım arızası olduğunda veya bir yükseltme gerektiğinde bir değiştirme cihazı gerekir. Cihazınızda bir donanım sorunu olduğunda aşağıdaki adımları uygulayın:

1. [Donanım sorunu için bir destek bileti açın](#open-a-support-ticket). Microsoft Desteği, bu örnek için bir alan değiştirme birimi (FRU) bulunmadığını veya cihazın bir donanım yükseltmesine ihtiyacı olduğunu belirlemektir. Her iki durumda da destek, bir değiştirme cihazını sıraya alır.
2. Değiştirme cihazı için [Yeni bir kaynak oluşturun](azure-stack-edge-deploy-prep.md#create-a-new-resource) . **Azure Stack Edge cihazım olmasına**karşı onay kutusunu seçtiğinizden emin olun. 
3. Bir değiştirme aygıtı aldıktan sonra, yeni kaynağa karşı değiştirme cihazını [yükleyip](azure-stack-edge-deploy-install.md) [etkinleştirin](azure-stack-edge-deploy-connect-setup-activate.md) .
4. Özgün cihazı döndürmek için tüm adımları izleyin:
    1. Özgün cihazı döndürmek için başka bir bilet açın.
    2. [Cihazdaki verileri silin](#erase-data-from-the-device).
    3. [Bir toplama zamanlayın](#schedule-a-pickup).
    5. Döndürülen cihazla ilişkili [kaynağı silin](#delete-the-resource) .



## <a name="next-steps"></a>Sonraki adımlar

- [Bant genişliğini yönetmeyi](azure-stack-edge-manage-bandwidth-schedules.md) öğrenin.
