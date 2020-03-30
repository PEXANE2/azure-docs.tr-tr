---
title: Azure IoT Merkezi uygulamanızdaki cihazları yönetme | Microsoft Dokümanlar
description: Operatör olarak, Azure IoT Central uygulamanızdaki cihazları nasıl yöneteceğimize öğrenin.
author: sarahhubbard
ms.author: sahubbar
ms.date: 12/06/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 8725a822c575ce80b9810d56bfd072241ded4c86
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157951"
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Azure IoT Merkezi uygulamanızdaki cihazları yönetme



Bu makalede, bir operatör olarak Azure IoT Merkezi uygulamanızdaki aygıtları nasıl yöneteceğimiz açıklanmaktadır. Bir operatör olarak şunları yapabilirsiniz:

- Azure IoT Merkezi uygulamanıza bağlı aygıtları görüntülemek, eklemek ve silmek için **Cihazlar** sayfasını kullanın.
- Aygıtlarınızın güncel envanterini koruyun.
- Görünümlerinizden aygıt özelliklerinde depolanan değerleri değiştirerek cihazınızın meta verilerini güncel tutun.
- Görünümlerinizden belirli bir aygıttaki bir ayarı güncelleyerek aygıtlarınızın davranışını denetler.

## <a name="view-your-devices"></a>Cihazlarınızı görüntüleme

Tek bir aygıtı görüntülemek için:

1. Sol bölmedeki **Aygıtlar'ı** seçin. Burada tüm aygıtların ve aygıt şablonlarınızın bir listesini görürsünüz.

1. Bir aygıt şablonu seçin.

1. **Aygıtlar** sayfasının sağ bölmesinde, bu aygıt şablonundan oluşturulan aygıtların listesini görürsünüz. Bu aygıtın aygıt ayrıntıları sayfasını görmek için tek bir aygıt seçin:

    ![Cihaz Detayları Sayfası](./media/howto-manage-devices/devicelist.png)


## <a name="add-a-device"></a>Cihaz ekleme

Azure IoT Merkezi uygulamanıza bir aygıt eklemek için:

1. Sol bölmedeki **Aygıtlar'ı** seçin.

1. Aygıt oluşturmak istediğiniz aygıt şablonunu seçin.

1. Seçin + **Yeni**.

1. **Benzetimli** geçişini **Açma** veya **Kapatma'ya**çevirin. Gerçek bir aygıt, Azure IoT Merkezi uygulamanıza bağladığınız fiziksel bir aygıt içindir. Benzetimli bir aygıtta Azure IoT Central tarafından sizin için oluşturulan örnek veriler vardır.

1. **Oluştur'u**tıklatın.

1. Bu aygıt artık bu şablon için aygıt listenizde görünür. Aygıtın tüm görünümlerini içeren aygıt ayrıntıları sayfasını görmek için aygıtı seçin.

## <a name="import-devices"></a>Aygıtları içe aktarma

Çok sayıda aygıtı uygulamanıza bağlamak için, aygıtları bir CSV dosyasından toplu olarak aktarabilirsiniz. CSV dosyasında aşağıdaki sütunlar ve üstbilgi olmalıdır:

* **IOTC_DeviceID** - cihaz kimliği tüm küçük olmalıdır.
* **IOTC_DeviceName** - Bu sütun isteğe bağlıdır.

Uygulamanızdaki aygıtları toplu olarak kaydetmek için:

1. Sol bölmedeki **Aygıtlar'ı** seçin.

1. Sol panelde, aygıtları toplu olarak oluşturmak istediğiniz aygıt şablonunu seçin.

    > [!NOTE]
    > Henüz bir aygıt şablonunuzun yoksa, aygıtları **Tüm aygıtların** altından içe aktarabilir ve şablon olmadan kaydedebilirsiniz. Aygıtlar içe aktarıldıktan sonra bunları bir şablona geçirebilirsiniz.

1. **İçeri aktar**'ı seçin.

    ![İthalat Eylemi](./media/howto-manage-devices/bulkimport1a.png)


1. Akit olunacak Aygıt iAnd'lerinin listesinin bulunduğu CSV dosyasını seçin.

1. Dosya yüklendikten sonra aygıt alma işlemi başlar. Aygıt İşlemleri panelinde alma durumunu izleyebilirsiniz. Bu panel, içe aktarma başladıktan sonra otomatik olarak görüntülenir veya sağ üst köşedeki zil simgesinden erişebilirsiniz.

1. Alma işlemi tamamlandıktan sonra Aygıt İşlemleri panelinde bir başarı iletisi gösterilir.

    ![İthalat Başarısı](./media/howto-manage-devices/bulkimport3a.png)


Aygıt alma işlemi başarısız olursa, Aygıt İşlemleri panelinde bir hata iletisi görürsünüz. İndirebileceğiniz tüm hataları yakalayan bir günlük dosyası oluşturulur.

**Aygıtları şablona geçirme**

**Tüm aygıtlar**altında içe aktarmayı başlatarak aygıtları kaydederseniz, aygıtlar herhangi bir aygıt şablonu ilişkilendirme olmadan oluşturulur. Aygıtlar, verileri ve aygıtla ilgili diğer ayrıntıları keşfetmek için bir şablonla ilişkilendirilmelidir. Aygıtları bir şablonla ilişkilendirmek için aşağıdaki adımları izleyin:

1. Sol bölmedeki **Aygıtlar'ı** seçin.

1. Sol panelde **Tüm aygıtları**seçin:

    ![İlişkisiz Aygıtlar](./media/howto-manage-devices/unassociateddevices1a.png)


1. **Aygıt Şablonu** sütunundaki değerin aygıtlarınızdan herhangi biri için "İlişkisiz" olup olmadığını belirlemek için ızgaradaki filtreyi kullanın.

1. Şablonla ilişkilendirmek istediğiniz aygıtları seçin:

1. **Geçir'i**seçin :

    ![Yardımcı Aygıtlar](./media/howto-manage-devices/unassociateddevices2a.png)


1. Kullanılabilir şablonlar listesinden şablonu seçin ve **Geçir'i**seçin.

1. Seçili aygıtlar seçtiğiniz aygıt şablonuyla ilişkilidir.


## <a name="export-devices"></a>Aygıtları dışa aktarma

Gerçek bir aygıtı IoT Central'a bağlamak için bağlantı dizesine ihtiyacınız vardır. Aygıt bağlantı dizeleri oluşturmak için ihtiyacınız olan bilgileri almak için aygıt ayrıntılarını toplu olarak dışa aktarabilirsiniz. Dışa aktarma işlemi, seçili tüm aygıtlar için aygıt kimliği, aygıt adı ve anahtarları içeren bir CSV dosyası oluşturur.

Uygulamanızdan toplu dışa aktarma aygıtları için:

1. Sol bölmedeki **Aygıtlar'ı** seçin.

1. Sol bölmede, aygıtları dışa aktarmak istediğiniz aygıt şablonunu seçin.

1. Dışa aktarmak istediğiniz aygıtları seçin ve ardından **Dışa Aktarma** eylemini seçin.

    ![Dışarı Aktarma](./media/howto-manage-devices/export1a.png)


1. Dışa aktarma işlemi başlar. Aygıt İşlemleri panelini kullanarak durumu izleyebilirsiniz.

1. Dışa aktarma tamamlandığında, oluşturulan dosyayı indirmek için bir bağlantı ile birlikte bir başarı iletisi gösterilir.

1. Dosyayı diskteki yerel bir klasöre indirmek için **Dosyayı İndir** bağlantısını seçin.

    ![İhracat Başarısı](./media/howto-manage-devices/export2a.png)


1. Dışa aktarılan CSV dosyası aşağıdaki sütunları içerir: aygıt kimliği, aygıt adı, aygıt anahtarları ve X509 sertifikası parmak izleri:

    * IOTC_DEVICEID
    * IOTC_DEVICENAME
    * IOTC_SASKEY_PRIMARY
    * IOTC_SASKEY_SECONDARY
    * IOTC_X509THUMBPRINT_PRIMARY
    * IOTC_X509THUMBPRINT_SECONDARY

Bağlantı dizeleri ve gerçek aygıtları IoT Central uygulamanıza bağlama hakkında daha fazla bilgi için [Azure IoT Central'daki Aygıt bağlantısına](concepts-get-connected.md)bakın.

## <a name="delete-a-device"></a>Cihazı silme

Azure IoT Merkezi uygulamanızdan gerçek veya simüle edilmiş bir aygıtı silmek için:

1. Sol bölmedeki **Aygıtlar'ı** seçin.

1. Silmek istediğiniz aygıtın aygıt şablonunu seçin.

1. Aygıtlarınızı filtrelemek ve aramak için filtre araçlarını kullanın. Silmek için aygıtların yanındaki kutuyu işaretleyin.

1. **Sil**’i seçin. Bu silme durumunu Aygıt İşlemleri panelinizde izleyebilirsiniz.

## <a name="change-a-property"></a>Bir özelliği değiştirme

Bulut özellikleri, aygıtla ilişkili şehir ve seri numarası gibi aygıt meta verileridir. Yazılabilir özellikler aygıtın davranışını denetler. Başka bir deyişle, cihazınıza giriş sağlamanızı sağlarlar.  Aygıt özellikleri aygıt tarafından ayarlanır ve IoT Central içinde salt okunur. Cihazınızın **Aygıt Ayrıntıları** görünümlerinde özellikleri görüntüleyebilir ve güncelleyebilirsiniz.

1. Sol bölmedeki **Aygıtlar'ı** seçin.

1. Özelliklerini değiştirmek istediğiniz aygıtın aygıt şablonunu seçin ve hedef aygıtı seçin.

1. Cihazınızın özelliklerini içeren görünümü seçin, bu görünüm değerleri girebilmenizi ve sayfanın üst kısmında **Kaydet'i** seçmenize olanak tanır. Burada cihazınızın özelliklerini ve geçerli değerlerini görürsünüz. Bulut özellikleri ve yazılabilir özellikleri, aygıt özellikleri salt okunurken, değiştirilebilir alanlara sahiptir. Yazılabilir özellikler için, alanın alt kısmında eşitleme durumlarını görebilirsiniz. 

1. Özellikleri gereksinim duyduğunuz değerlerle değiştirin. Aynı anda birden çok özelliği değiştirebilir ve aynı anda tümözellikleri güncelleştirebilirsiniz.

1. **Kaydet**'i seçin. Yazılabilir özellikleri kaydettiyseniz, değerler cihazınıza gönderilir. Aygıt yazılabilir özelliğin değişikliğini onayladığında, durum **eşitlenmiş**olarak geri döner. Bir bulut özelliği kaydettiyseniz, değer güncelleştirilir.


## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızdaki cihazları nasıl yöneteceğimize dair öğrendiğinize göre, önerilen bir sonraki adım şudur:

> [!div class="nextstepaction"]
> [Cihaz grupları nasıl kullanılır?](tutorial-use-device-groups.md)

<!-- Next how-tos in the sequence -->
