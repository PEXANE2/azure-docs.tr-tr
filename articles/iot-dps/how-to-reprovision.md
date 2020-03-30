---
title: Azure IoT Hub Aygıt Sağlama Hizmeti'ndeki aygıtları yeniden sağlama
description: Aygıt Sağlama Hizmeti (DPS) örneğinizle aygıtları nasıl yeniden sağabileceğinizi ve bunu neden yapmanız gerekebileceğini öğrenin.
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 0ded494debab19daa15a953715b1ab7b0b10ad18
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74974913"
---
# <a name="how-to-reprovision-devices"></a>Aygıtları yeniden sağlama

Bir IoT çözümünün yaşam döngüsü sırasında aygıtları IoT hub'ları arasında taşımak yaygındır. Bu hareketin nedenleri aşağıdaki senaryoları içerebilir:

* **Coğrafi konum**: Aygıt konumlar arasında hareket ettikçe, aygıtın her konuma daha yakın bir IoT hub'ına geçirilmesiyle ağ gecikmesi artar.

* **Çoklu kiralama**: Bir cihaz aynı IoT çözümü içinde kullanılabilir, ancak yeni bir müşteriye veya müşteri sitesine yeniden atanmış veya kiralanmış olabilir. Bu yeni müşteriye farklı bir IoT hub'ı kullanılarak hizmet verilebilir.

* **Çözüm değişikliği**: Bir aygıt yeni veya güncelleştirilmiş bir IoT çözümüne taşınabilir. Bu yeniden atama, aygıtın diğer arka uç bileşenlerine bağlı yeni bir IoT hub'ı ile iletişim kurmasını gerektirebilir. 

* **Karantina**: Çözelti değişikliğine benzer. Arızalı, gizliliği ihlal edilen veya güncelliğini yitirmiş bir aygıt, tüm yapabileceği şeyin güncelleştirilip uyumlu hale gelmek olduğu bir IoT hub'ına yeniden atanabilir. Aygıt düzgün çalıştığında, ana hub'ına geri geçirilir.

Yeniden sağlamayla ilgili daha ayrıntılı bir genel bakış için, [IoT Hub Aygıt yeniden sağlama kavramlarına](concepts-device-reprovision.md)bakın.


## <a name="configure-the-enrollment-allocation-policy"></a>Kayıt tahsisat ilkesini yapılandırma

Ayırma ilkesi, kayıtla ilişkili aygıtların yeniden sağlandıktan sonra bir IoT hub'ına nasıl tahsis edilmeyeceğini veya atanacağını belirler.

Aşağıdaki adımlar, aygıtın kaydı için ayırma ilkesini yapılandırır:

1. [Azure portalında](https://portal.azure.com) oturum açın ve Cihaz Sağlama Hizmeti örneğinize gidin.

2. **Kayıtları Yönet'i**tıklatın ve yeniden sağlama için yapılandırmak istediğiniz kayıt grubunu veya tek tek kaydı tıklatın. 

3. **Hub'lara aygıtları nasıl atamak istediğinizi seçin'in**altında, aşağıdaki ayırma ilkelerinden birini seçin:

    * **En düşük gecikme :** Bu ilke, aygıt ve IoT Hub arasındaki en düşük gecikme gecikmesi iletişimine neden olacak aygıtları bağlantılı IoT Hub'ına atar. Bu seçenek, aygıtın konuma göre en yakın IoT hub'ı ile iletişim kurmasını sağlar. 
    
    * **Eşit ağırlıklı dağıtım**: Bu ilke, aygıtları bağlantılı her IoT hub'ına atanan ayırma ağırlığına göre bağlantılı IoT Hub'larına dağıtır. Bu ilke, bakiye aygıtlarını bu hub'larda ayarlanan ayırma ağırlıklarına göre bağlantılı hub'lar grubuna yüklemenize olanak tanır. Aygıtları yalnızca bir IoT Hub'ına sayarsanız, bu ayarı öneririz. Bu varsayılan ayardır. 
    
    * **Statik yapılandırma**: Bu ilke, bir aygıtın sağlanması için kayıt girişinde istenilen bir IoT Hub'ın listelenmesini gerektirir. Bu ilke, aygıtları atamak istediğiniz belirli bir IoT hub'ı belirlemenize olanak tanır.

4. **IoT hub'larını seçin'in altında bu gruba atanabilir,** tahsisat politikanıza dahil olmasını istediğiniz bağlantılı IoT hub'larını seçin. İsteğe bağlı olarak, Yeni Bir **IoT Hub** düğmesini bağla'yı kullanarak yeni bir bağlantılı Iot hub ekleyin.

    En **Düşük gecikme ayırma** ilkesiyle, seçtiğiniz hub'lar aygıt ataması için en yakın hub'ı belirlemek için gecikme sonu değerlendirmesine dahil edilir.

    Eşit **ağırlıklı dağıtım** ayırma ilkesiyle, aygıtlar yapılandırılmış ayırma ağırlıklarına ve geçerli aygıt yüklerine bağlı olarak seçtiğiniz hub'lar arasında yük dengeli hale gelir.

    Statik **yapılandırma** ayırma ilkesiyle, atanmasını istediğiniz aygıtlar için IoT hub'ını seçin.

4. **Kaydet'i**tıklatın veya yeniden sağlama ilkesini ayarlamak için bir sonraki bölüme gidin.

    ![Kayıt tahsisat ilkesini seçin](./media/how-to-reprovision/enrollment-allocation-policy.png)



## <a name="set-the-reprovisioning-policy"></a>Yeniden sağlama ilkesini ayarlama

1. [Azure portalında](https://portal.azure.com) oturum açın ve Cihaz Sağlama Hizmeti örneğinize gidin.

2. **Kayıtları Yönet'i**tıklatın ve yeniden sağlama için yapılandırmak istediğiniz kayıt grubunu veya tek tek kaydı tıklatın.

3. **Aygıt verilerinin farklı bir IoT hub'ına yeniden sağlanmasında nasıl işleneceğini seçin,** aşağıdaki yeniden sağlama ilkelerinden birini seçin:

    * **Verileri yeniden sağlama ve geçirme**: Bu ilke, kayıt girişiyle ilişkili aygıtlar yeni bir sağlama isteği gönderdiğinde harekete geçer. Kayıt girişi yapılandırmasına bağlı olarak, aygıt başka bir IoT hub'ına yeniden atanabilir. Aygıt IoT hub'larını değiştiriyorsa, ilk IoT hub'ına sahip aygıt kaydı kaldırılır. Bu ilk IoT hub'ından gelen tüm aygıt durumu bilgileri yeni IoT hub'ına taşınır. Geçiş sırasında, aygıtın durumu **Atama** olarak bildirilir

    * **Yeniden sağlama ve ilk config'e sıfırlama**: Bu ilke, kayıt girişiyle ilişkili aygıtlar yeni bir sağlama isteği gönderdiğinde harekete geçer. Kayıt girişi yapılandırmasına bağlı olarak, aygıt başka bir IoT hub'ına yeniden atanabilir. Aygıt IoT hub'larını değiştiriyorsa, ilk IoT hub'ına sahip aygıt kaydı kaldırılır. Aygıt sağlandığında sağlama hizmeti örneğinin aldığı ilk yapılandırma verileri yeni IoT hub'ına sağlanır. Geçiş sırasında, aygıtın durumu **Atama**olarak bildirilecektir.

4. Değişikliklerinize göre aygıtın yeniden sağlanmasını etkinleştirmek için **Kaydet'i** tıklatın.

    ![Kayıt tahsisat ilkesini seçin](./media/how-to-reprovision/reprovisioning-policy.png)



## <a name="send-a-provisioning-request-from-the-device"></a>Aygıttan bir sağlama isteği gönderme

Aygıtların önceki bölümlerde yapılan yapılandırma değişikliklerine göre yeniden sağlanabilmesi için, bu aygıtların yeniden sağlama talebinde bulunmaları gerekir. 

Bir aygıtın sağlama isteği gönderme sıklığı senaryoya bağlıdır. Ancak, cihazlarınızı yeniden başlatmada bir sağlama hizmeti örneğine sağlama isteği gönderecek şekilde programlamanız ve isteğe bağlı olarak sağlamayı el ile tetikleyen bir [yöntemi](../iot-hub/iot-hub-devguide-direct-methods.md) desteklemeniz önerilir. Sağlama da istenilen bir [özellik](../iot-hub/iot-hub-devguide-device-twins.md#desired-property-example)ayarlayarak tetiklenebilir. 

Bir kayıt girişiyle ilgili yeniden sağlama ilkesi, aygıt sağlama hizmeti örneğinin bu sağlama isteklerini nasıl işleyeceğini ve aygıt durumu verilerinin yeniden sağlama sırasında geçirilip geçirilmeyeceğini belirler. Aynı ilkeler tek tek kayıt ve kayıt grupları için kullanılabilir:

Örneğin önyükleme sırası sırasında bir aygıttan sağlama istekleri gönderme kodu, [bkz.](quick-create-simulated-device.md)


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla Yeniden sağlama öğrenmek için [IoT Hub Aygıt yeniden sağlama kavramlarına](concepts-device-reprovision.md) bakın 
- Daha fazla Sağlama hakkında bilgi edinmek [için, daha önce otomatik olarak sağlanan aygıtların nasıl yok edilir](how-to-unprovision-devices.md) hale geldiğini öğrenin 











