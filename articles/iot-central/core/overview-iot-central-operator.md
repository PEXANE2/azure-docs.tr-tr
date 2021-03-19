---
title: Azure IoT Central operatör Kılavuzu
description: Azure IoT Central, IoT çözümlerinin oluşturulmasını kolaylaştıran bir IoT uygulama platformudur. Bu makale, IoT Central operatör rolüne genel bir bakış sağlar.
author: TheJasonAndrew
ms.author: v-anjaso
ms.date: 03/17/2021
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom:
- mvc
- device-developer
ms.openlocfilehash: a5c307b8c3c186fd7dff2084c728f02b8165b861
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610002"
---
# <a name="iot-central-operator-guide"></a>IoT Central operatör Kılavuzu

Bu makale, IoT Central operatör rolüne genel bir bakış sağlar. Operatör, uygulamaya bağlı cihazları yönetir.

_Operatör_, uygulamaya bağlı cihazları yönetir.

Bir işleç olarak şunları yapabilirsiniz:

* Azure IoT Central uygulamanıza bağlı cihazları görüntülemek, eklemek ve silmek için **cihazlar** sayfasını kullanın.
* Cihazları toplu olarak içeri ve dışarı aktarın.
* Cihazlarınızın güncel envanterini saklayın.
* Görünümlerinizin cihaz özelliklerinde depolanan değerlerini değiştirerek cihaz meta verilerini güncel tutun.
* Belirli bir cihazdaki bir ayarı görünümlarınızdan güncelleştirerek cihazlarınızın davranışını denetleyin.

## <a name="iot-central-homepage"></a>IoT Central giriş sayfası

[IoT Central giriş](https://aka.ms/iotcentral-get-started) sayfası sayfası, IoT Central sunulan en son haberler ve özellikler hakkında daha fazla bilgi edinebilirsiniz, yeni uygulamalar oluşturabilir ve var olan uygulamanızı görebilir ve başlatabilir.

:::image type="content" source="media/overview-iot-central-tour/iot-central-homepage.png" alt-text="IoT Central giriş sayfasının ekran görüntüsü":::

## <a name="view-your-devices"></a>Cihazlarınızı görüntüleme

Tek bir cihazı görüntülemek için:

1. Sol bölmedeki **cihazlar** ' ı seçin. Burada tüm cihazların ve cihaz şablonlarınızın bir listesini görürsünüz.

1. Bir cihaz şablonu seçin.

1. **Cihazlar** sayfasının sağ bölmesinde, bu cihaz şablonundan oluşturulan cihazların bir listesini görürsünüz. Bu cihaz için cihaz ayrıntıları sayfasını görmek için tek bir cihaz seçin:

    ![Cihaz Ayrıntıları sayfasının ekran görüntüsü](./media/overview-iot-central-operator/device-list.png)

## <a name="add-a-device"></a>Cihaz ekleme

Azure IoT Central uygulamanıza bir cihaz eklemek için:

1. Sol bölmedeki **cihazlar** ' ı seçin.

1. Cihaz oluşturmak istediğiniz cihaz şablonunu seçin.

1. + **Yeni** seçeneğini belirleyin.

1. **Benzetimli** geçişi **Açık** veya **kapalı** olarak açın. Gerçek bir cihaz, Azure IoT Central uygulamanıza bağlandığınız fiziksel bir cihaza yöneliktir. Sanal cihaz, Azure IoT Central tarafından sizin için oluşturulan örnek verilere sahiptir.

1. **Oluştur**’u seçin.

1. Bu cihaz artık cihaz listenizde bu şablon için görünüyor. Cihazın tüm görünümlerini içeren cihaz ayrıntıları sayfasını görmek için cihazı seçin.

## <a name="import-devices"></a>Cihazları içeri aktar

Çok sayıda cihazı uygulamanıza bağlamak için, cihazları bir CSV dosyasından toplu içeri aktarabilirsiniz. CSV dosyası aşağıdaki sütunlara ve üstbilgilere sahip olmalıdır:

* **IOTC_DeviceID** -cihaz kimliği harfler, rakamlar ve `-` karakter içerebilir.
* **IOTC_DeviceName** -bu sütun isteğe bağlıdır.

Uygulamanızdaki cihazları toplu olarak kaydetmek için:

1. Sol bölmedeki **cihazlar** ' ı seçin.

1. Sol bölmede, cihazlarını toplu olarak oluşturmak istediğiniz cihaz şablonunu seçin.

    > [!NOTE]
    > Henüz bir cihaz şablonunuz yoksa, cihazları **tüm cihazlarda** içeri aktarabilir ve şablon olmadan kaydedebilirsiniz. Cihazlar alındıktan sonra bunları bir şablona geçirebilirsiniz.

1. **İçeri aktar**'ı seçin.

    ![Içeri aktarma eyleminin ekran görüntüsü](./media/overview-iot-central-operator/bulk-import-1-a.png)


1. İçeri aktarılacak cihaz kimliklerinin listesini içeren CSV dosyasını seçin.

1. Dosya karşıya yüklendikten sonra cihaz içeri aktarma işlemi başlar. İçeri aktarma durumunu cihaz Işlemleri panelinde izleyebilirsiniz. Bu panel, içeri aktarma başladıktan sonra otomatik olarak görünür veya sağ üst köşedeki zil simgesiyle erişebilirsiniz.

1. İçeri aktarma işlemi tamamlandıktan sonra, cihaz Işlemler panelinde bir başarı iletisi gösterilir.

    ![Içeri aktarma başarısı ekran görüntüsü](./media/overview-iot-central-operator/bulk-import-3-a.png)

Cihaz içeri aktarma işlemi başarısız olursa, cihaz Işlemleri panelinde bir hata iletisi görürsünüz. İndirebileceğiniz tüm hataları yakalayan bir günlük dosyası oluşturulur.

## <a name="migrate-devices-to-a-template"></a>Cihazları bir şablona geçirme

**Tüm cihazlarda** içeri aktarma işlemi başlatarak cihazları kaydedersiniz, cihazlar herhangi bir cihaz şablonu ilişkilendirmesi olmadan oluşturulur. Cihazların verileri ve cihazla ilgili diğer ayrıntıları araştırmak için bir şablonla ilişkilendirilmesi gerekir. Cihazları bir şablonla ilişkilendirmek için aşağıdaki adımları izleyin:

1. Sol bölmedeki **cihazlar** ' ı seçin.

1. Sol panelde **tüm cihazlar**' ı seçin:

    ![Ilişkilendirilmemiş cihazların ekran görüntüsü](./media/overview-iot-central-operator/unassociated-devices-2-a.png)

1. **Cihaz şablonu** sütunundaki değerin cihazlarınızdan herhangi biri için **ilişkilendirilmemiş** olup olmadığını öğrenmek için kılavuzdaki filtreyi kullanın.

1. Bir şablonla ilişkilendirmek istediğiniz cihazları seçin:

1. **Geçişi** seçin:

    ![Cihazların Ilişkilendirme ekran görüntüsü](./media/overview-iot-central-operator/unassociated-devices-1-a.png)

1. Kullanılabilir şablonlar listesinden şablonu seçin ve **geçir**' i seçin.

1. Seçilen cihazlar seçtiğiniz cihaz şablonuyla ilişkili.

## <a name="export-devices"></a>Cihazları dışarı aktar

Gerçek bir cihazı IoT Central bağlamak için, bağlantı dizesine ihtiyacınız vardır. Cihaz bağlantı dizeleri oluşturmak için gereken bilgileri almak üzere cihaz ayrıntılarını toplu olarak dışarı aktarabilirsiniz. Dışarı aktarma işlemi, seçilen tüm cihazların cihaz kimliği, cihaz adı ve anahtarlarıyla bir CSV dosyası oluşturur.

Uygulamanızdaki cihazları toplu olarak dışarı aktarmak için:

1. Sol bölmedeki **cihazlar** ' ı seçin.

1. Sol bölmede, cihazlarını dışarı aktarmak istediğiniz cihaz şablonunu seçin.

1. Dışarı aktarmak istediğiniz cihazları seçin ve ardından **dışarı aktarma** eylemini seçin.

    ![Dışarı aktarma ekran görüntüsü](./media/overview-iot-central-operator/export-1-a.png)

1. Dışarı aktarma işlemi başlar. Cihaz Işlemleri panelini kullanarak durumu izleyebilirsiniz.

1. Dışarı aktarma işlemi tamamlandığında, oluşturulan dosyayı indirmek için bir bağlantı ile birlikte bir başarı iletisi gösterilir.

1. Dosyayı diskteki yerel bir klasöre indirmek için **dosya indir** bağlantısını seçin.

    ![Dışarı aktarma başarısı ekran görüntüsü](./media/overview-iot-central-operator/export-2-a.png)

1. İçe aktarılmış CSV dosyası şu sütunları içerir: cihaz KIMLIĞI, cihaz adı, cihaz anahtarları ve x509 sertifikası parmak izleri:

    * IOTC_DEVICEID
    * IOTC_DEVICENAME
    * IOTC_SASKEY_PRIMARY
    * IOTC_SASKEY_SECONDARY
    * IOTC_X509THUMBPRINT_PRIMARY
    * IOTC_X509THUMBPRINT_SECONDARY

Bağlantı dizeleri ve gerçek Cihazları IoT Central uygulamanıza bağlama hakkında daha fazla bilgi için bkz. [Azure IoT Central cihaz bağlantısı](concepts-get-connected.md).

## <a name="delete-a-device"></a>Cihazı silme

Azure IoT Central uygulamanızdan gerçek ya da sanal bir cihazı silmek için:

1. Sol bölmedeki **cihazlar** ' ı seçin.

1. Silmek istediğiniz cihazın cihaz şablonunu seçin.

1. Cihazlarınızı filtrelemek ve aramak için filtre araçlarını kullanın. Silinecek cihazların yanındaki kutuyu işaretleyin.

1. **Sil**’i seçin. Bu silmenin durumunu cihaz Işlemleri panelinden izleyebilirsiniz.

## <a name="change-a-property"></a>Bir özelliği değiştirme

Bulut özellikleri, cihazla ilişkili, şehir ve seri numarası gibi cihaz meta verileriydi. Bulut özellikleri yalnızca IoT Central uygulamasında bulunur ve cihazlarınızla eşitlenmez. Yazılabilir Özellikler bir cihazın davranışını denetler ve örneğin bir termostat cihazının hedef sıcaklığını ayarlayarak bir cihazın durumunu uzaktan ayarlamanıza olanak sağlar.  Cihaz özellikleri cihaz tarafından ayarlanır ve IoT Central içinde salt okunurdur. Cihazınızın **cihaz ayrıntıları** görünümlerinde özellikleri görüntüleyebilir ve güncelleştirebilirsiniz.

1. Sol bölmedeki **cihazlar** ' ı seçin.

1. Özelliklerini değiştirmek istediğiniz cihazın cihaz şablonunu seçin ve hedef cihazı seçin.

1. Cihazınızın özelliklerini içeren görünümü seçin. Bu görünüm, değerleri girmenizi ve sayfanın en üstünde **Kaydet** ' i seçmenizi sağlar. Burada, cihazınızın sahip olduğu özellikleri ve bunların geçerli değerlerini görürsünüz. Bulut özellikleri ve yazılabilir özellikler, cihaz özellikleri salt okunurdur, düzenlenebilir alanlara sahiptir. Yazılabilir özellikler için alanın en altında eşitleme durumlarını görebilirsiniz. 

1. Özellikleri, ihtiyacınız olan değerlerle değiştirin. Aynı anda birden çok özelliği değiştirebilir ve tümünü aynı anda güncelleştirebilirsiniz.

1. **Kaydet**'i seçin. Yazılabilir özellikleri kaydettiyseniz, değerler cihazınıza gönderilir. Cihaz yazılabilir özelliğin değişikliğini onayladığında, durum **eşitlenmiş** olarak geri döner. Bir bulut özelliğini kaydettiyseniz, değer güncellenir.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda cihazların nasıl yönetileceğini öğrendiğinize göre, önerilen sonraki adım cihazlarınıza yönelik [kuralların nasıl yapılandırılacağını](howto-configure-rules.md) öğrenirsiniz.