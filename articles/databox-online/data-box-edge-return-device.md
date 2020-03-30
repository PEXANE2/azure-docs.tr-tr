---
title: Azure Veri Kutusu Kenarı aygıtınızı iade edin veya değiştirin | Microsoft Dokümanlar
description: Azure Veri Kutusu Kenarı aygıtının nasıl döndürüldebildiğini veya değiştirilmeye devam edilebildiğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 02/26/2020
ms.author: alkohli
ms.openlocfilehash: 521277b2eed7edfba016f6a80e8f877decfb0ac5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651109"
---
# <a name="return-or-replace-your-azure-data-box-edge-device"></a>Azure Veri Kutusu Kenarı aygıtınızı iade edin veya değiştirin

Bu makalede, verileri nasıl silip azure veri kutusu kenar aygıtınızı nasıl döndürebilirsiniz. Aygıtı iade ettikten sonra, aygıtla ilişkili kaynağı da silebilir veya yeni bir aygıt sipariş edebilirsiniz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Aygıttaki veri disklerinden verileri silme
> * Cihazınızı iade etmek için destek bileti açma
> * Cihazı topla ve bir paket alma programı
> * Azure portalındaki kaynağı silme
> * Yeni bir cihaz alın

## <a name="erase-data-from-the-device"></a>Aygıttan veri silme

Cihazınızın veri disklerinden verileri silmek için aygıtınızı sıfırlamanız gerekir. Yerel web ui veya PowerShell arabirimini kullanarak cihazınızı sıfırlayabilirsiniz.

Sıfırlamadan önce, gerekirse aygıttaki yerel verilerin bir kopyasını oluşturun. Verileri aygıttan bir Azure Depolama kapsayıcısına kopyalayabilirsiniz.

Yerel web web ui kullanarak cihazınızı sıfırlamak için aşağıdaki adımları izleyin.

1. Yerel web web web ui'sinde, **Maintenance > Device sıfırlama**adresine gidin.
2. **Cihazı Sıfırla'yı**seçin.

    ![Cihaz sıfırlama](media/data-box-edge-return-device/device-reset-1.png)

3. Onay istendiğinde, uyarıyı gözden geçirin ve devam etmek için **Evet'i** seçin.

    ![Sıfırlamayı onayla](media/data-box-edge-return-device/device-reset-2.png)  

Sıfırlama, aygıt veri disksindeki verileri siler. Cihazınızdaki veri miktarına bağlı olarak, bu işlem yaklaşık 30-40 dakika sürer.

Alternatif olarak, aygıtın PowerShell arabirimine `Reset-HcsAppliance` bağlanın ve verileri veri disklerinden silmek için cmdlet'i kullanın. Daha fazla bilgi için [bkz.](data-box-edge-connect-powershell-interface.md#reset-your-device)

> [!NOTE]
> - Yeni bir aygıtla alışveriş için değiştiriyor veya yeni bir cihaza modernizasyonu yapıyorsanız, cihazınızı ancak yeni aygıtı aldıktan sonra sıfırlamanızı öneririz.
> - Aygıt sıfırlama yalnızca aygıttaki tüm yerel verileri siler. Buluttaki veriler silinmez ve [ücret](https://azure.microsoft.com/pricing/details/storage/)toplar. Bu verilerin [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)gibi bir bulut depolama yönetimi aracı kullanılarak ayrı olarak silinmesi gerekir.

## <a name="open-a-support-ticket"></a>Destek bileti açma

İade işlemini başlatmak için aşağıdaki adımları izleyin.

1. Cihazı iade etmek istediğinizi belirten Microsoft Destek ile bir Destek bileti açın. **Veri Kutusu Kenar Donanımı**olarak sorun türünü seçin.

    ![Destek bileti açma](media/data-box-edge-return-device/open-support-ticket-1.png)  

2. Bir Microsoft Destek mühendisi sizinle iletişim erecektir. Sevkiyat ayrıntılarını sağlayın.
3. İade gönderim kutusuna ihtiyacınız varsa, bunu talep edebilirsiniz. Soruya **Evet** **yanıtı Dönmek için boş bir kutu gerekir.**


## <a name="schedule-a-pickup"></a>Bir teslim alma planı

1. Cihazı kapatın. Yerel web ui'sinde, **Bakım > Güç ayarlarına**gidin.
2. **Kapat'ı**seçin. Onay istendiğinde, devam etmek için **Evet'i** tıklatın. Daha fazla bilgi için [bkz.](data-box-gateway-manage-access-power-connectivity-mode.md#manage-power)
3. Güç kablolarını çıkarın ve tüm ağ kablolarını cihazdan çıkarın.
4. Kendi kutunuzu veya Azure'dan aldığınız boş kutuyu kullanarak gönderi paketini hazırlayın. Cihazı ve cihazla birlikte gönderilen güç kablolarını kutuya yerleştirin.
5. Pakete Azure'dan aldığınız gönderi etiketini yapıştırın.
6. Bölgenizdeki taşıyıcıdan bir kargo toplama randevusu alın. Cihazı ABD'de iade ediyorsanız, operatörünüz UPS veya FedEx olabilir. UPS ile bir alım planlamak için:

    1. Yerel UPS'i (ülkeye özel ücretsiz numara) arayın.
    2. Aramanızda, yazdırılan etiketinizde gösterildiği gibi ters sevkiyat takip numarasını alıntı.
    3. Takip numarası belirtilmiş değilse, UPS teslim alma sırasında ek bir ücret ödemenizi gerektirir.

    Alıcıyı planlamak yerine, Veri Kutusu Kenarını en yakın bırakma konumuna da bırakabilirsiniz.

## <a name="delete-the-resource"></a>Kaynağı silme

Cihaz Azure veri merkezinden alındıktan sonra, cihaz hasar veya kurcalama belirtisi olup olmadığı denetlenir.

- Aygıt sağlam ve iyi durumda gelirse, faturaölçer ibu kaynak için durur. Microsoft Destek, aygıtın döndürüldüğünü onaylamak için sizinle temasa geçer. Daha sonra Azure portalındaki aygıtla ilişkili kaynağı silebilirsiniz.
- Cihaz önemli ölçüde hasar görürse, para cezaları uygulanabilir. Ayrıntılar için, [kaybolan veya hasar gören cihazla ilgili SSS'ye](https://azure.microsoft.com/pricing/details/databox/edge/) ve [Ürün Hizmet Şartları'na](https://www.microsoft.com/licensing/product-licensing/products)bakın.  


Aygıtı Azure portalında silebilirsiniz:
-   Siparişi verdikten sonra ve cihaz Microsoft tarafından hazırlanmadan önce.
-   Aygıtı Microsoft'a iade ettikten sonra, Azure veri merkezinde fiziksel denetimden geçer ve microsoft destek, aygıtın döndürüldünden onay almak için çağrılarda bulunur.

Aygıtı başka bir aboneye veya konuma göre etkinleştirdiyseniz, Microsoft siparişinizi bir iş günü içinde yeni aboneye veya konuma taşır. Sipariş taşındıktan sonra bu kaynağı silebilirsiniz.


Azure portalındaki aygıtı ve kaynağı silmek için aşağıdaki adımları izleyin.

1. Azure portalında kaynağınıza gidin ve ardından **Genel Bakış'a gidin.** Komut çubuğundan **Sil'i**seçin.

    ![Sil'i seçin](media/data-box-edge-return-device/delete-resource-1.png)

2. Sil **aygıt** bıçağına, silmek istediğiniz aygıtın adını yazın ve **Sil'i**seçin.

    ![Silmeyi onayla](media/data-box-edge-return-device/delete-resource-2.png)

Aygıt tan sonra bilgilendirilirsiniz ve ilişkili kaynak başarıyla silinir.

## <a name="get-a-replacement-device"></a>Yeni bir cihaz alın

Varolan aygıtın donanım arızası olduğunda veya yükseltmeye ihtiyacı olduğunda değiştirme aygıtı gereklidir. Cihazınızda donanım sorunu olduğunda aşağıdaki adımları izleyin:

1. [Donanım sorunu için destek bileti açın.](#open-a-support-ticket) Microsoft Destek, bu örnekiçin bir Alan Değiştirme Birimi'nin (FRU) kullanılmadığını veya aygıtın bir donanım yükseltmesi gerektiğini belirler. Her iki durumda da, Destek bir yedek cihaz sipariş edecektir.
2. Değiştirme aygıtı için [yeni bir kaynak oluşturun.](data-box-edge-deploy-prep.md#create-a-new-resource) Ben bir Veri Kutusu **Edge cihaz var**karşı onay kutusunu seçtiğinizden emin olun. 
3. Değiştirilen bir aygıt aldıktan sonra, değiştirme aygıtını yeni kaynağa karşı [yükleyin](data-box-edge-deploy-install.md) ve [etkinleştirin.](data-box-edge-deploy-connect-setup-activate.md)
4. Özgün aygıtı döndürmek için tüm adımları izleyin:
    1. Orijinal cihazı iade etmek için başka bir bilet açın.
    2. [Aygıttaki verileri silin.](#erase-data-from-the-device)
    3. [Bir pikap planlayın.](#schedule-a-pickup)
    5. Döndürülen aygıtla ilişkili [kaynağı silin.](#delete-the-resource)



## <a name="next-steps"></a>Sonraki adımlar

- [Bant genişliğini yönetmeyi](data-box-edge-manage-bandwidth-schedules.md) öğrenin.
