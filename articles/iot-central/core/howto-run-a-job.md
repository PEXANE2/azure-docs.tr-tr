---
title: Azure IoT Central uygulamanızda iş oluşturma ve çalıştırma | Microsoft Docs
description: Azure IoT Central işleri, özelliklerinin güncelleştirilmesi veya bir komutun çalıştırılması gibi toplu cihaz yönetimi özelliklerine izin verir.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 06/08/2020
ms.topic: how-to
ms.openlocfilehash: 406881a9131aae35b91dcab248745bb426cecf0e
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88797845"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Azure IoT Central uygulamanızda iş oluşturma ve çalıştırma

Bağlı cihazlarınızı işler arasında ölçeklendirerek yönetmek için Microsoft Azure IoT Central kullanabilirsiniz. İşler, cihaz özelliklerine toplu güncelleştirmeler yapmanızı ve komutları çalıştırmanızı sağlar. Bu makalede, kendi uygulamanızdaki işleri kullanmaya nasıl başlacağınız gösterilmektedir.

## <a name="create-and-run-a-job"></a>İş oluşturma ve çalıştırma

Bu bölümde bir lojistik ağ geçidi cihazı grubu için ışık eşiğini ayarlama biçiminde bir işi oluşturma ve çalıştırma işlemi gösterilmektedir.

1. Sol bölmede **işler**' i seçin.

2. **+ Yeni** seçeneğini belirleyin.

   ![İş oluşturma seçimlerini gösteren ekran görüntüsü.](./media/howto-run-a-job/create-new-job.png)

3. Oluşturmakta olduğunuz işi tanımlamak için bir ad ve açıklama girin.

4. İşinizin uygulanmasını istediğiniz hedef cihaz grubunu seçin. **Özet** bölümünde iş yapılandırmanızın kaç cihaz için geçerli olduğunu görebilirsiniz.

5. Yapılandırılacak iş türü olarak **bulut özelliğini**, **özelliği**veya **komutu** seçin. 

   Bir **özellik** işi yapılandırması ayarlamak için bir özellik seçin ve yeni değerini ayarlayın. Bir **komut** iş yapılandırması ayarlamak için çalıştırılacak komutu seçin. Bir özellik işi birden çok özellik ayarlayabilir.

   ![Hafif eşik kümesi adlı bir özellik işi oluşturma seçimlerini gösteren ekran görüntüsü.](./media/howto-run-a-job/configure-job.png)

6. **Çalıştır** veya **Kaydet**' i seçin. İş artık ana **işler** sayfanızda görüntülenir. Bu sayfada, çalışmakta olan işinizi ve daha önce çalıştırılan veya kaydedilmiş işlerin geçmişini görebilirsiniz. Kayıt işlemini dilediğiniz zaman yeniden açarak veya çalıştırmaya devam edebilirsiniz.

   ![Oluşturulan bir işin adını, durumunu ve açıklamasını gösteren ekran görüntüsü.](./media/howto-run-a-job/view-job.png)

   > [!NOTE]
   > Daha önce çalıştırılan işleriniz için 30 günlük geçmişi görüntüleyebilirsiniz.

7. Kaydedilen işi seçin ve **Çalıştır** düğmesini seçerek çalıştırın. 

   **Işinizi Çalıştır?** iletişim kutusu görüntülenir. **Işi Çalıştır** düğmesini seçerek onaylayın. 

   ![Bir işi çalıştırmak istediğinizi onaylayan iletişim kutusunun ekran görüntüsü.](./media/howto-run-a-job/run-job.png)

8. Bir iş, bekleyen, çalışan ve tamamlanan aşamalardan geçer. İş yürütme ayrıntıları, sonuç ölçümlerini, süre ayrıntılarını ve bir cihaz listesi kılavuzunu içerir. 

   Bu genel bakışta, cihazlar ve bunların durum değerleri dahil olmak üzere iş ayrıntılarınızın CSV dosyasını indirmek için **sonuç günlüğü** ' nü de seçebilirsiniz. Bu bilgiler, sorun giderme için yararlı olabilir.

   ![Cihaz durumunu gösteren ekran görüntüsü.](./media/howto-run-a-job/download-details.png)

## <a name="manage-jobs"></a>İşleri yönetme

Çalışan bir işi durdurmak için açın ve **Durdur**' u seçin. İş durumu, işin durdurulduğunu yansıtacak şekilde değişir. **Özet** bölümü, hangi cihazların tamamlandığını, başarısız olduğunu veya hala beklendiğini gösterir.

![Çalışan bir işi ve işi durdurma düğmesini gösteren ekran görüntüsü.](./media/howto-run-a-job/manage-job.png)

Bir iş durdurulmuş durumdaysa, işi çalıştırmaya devam etmek için **devam** ' ı seçebilirsiniz. İş durumu, işin şimdi yeniden çalıştığını yansıtacak şekilde değişir. **Özet** bölümü en son ilerleme ile güncellenmeye devam eder.

![Durdurulmuş bir işi ve bir işe devam eden düğmeyi gösteren ekran görüntüsü.](./media/howto-run-a-job/stopped-job.png)

## <a name="copy-a-job"></a>İşi kopyalama

Mevcut işlerinizin birini kopyalamak için **işler** sayfasında bunu seçin ve **iş ayrıntıları**' nı seçin. **İş ayrıntıları** sayfası görüntülenir. 

![İş ayrıntıları sayfasını gösteren ekran görüntüsü.](./media/howto-run-a-job/job-details.png)

**Kopyala**’yı seçin.

![Kopyala düğmesini gösteren ekran görüntüsü.](./media/howto-run-a-job/job-details-copy.png)

Düzenlemeniz için iş yapılandırmasının bir kopyası açılır ve **kopyalama** işi adına eklenir. Yeni işi kaydedebilir veya çalıştırabilirsiniz.

![İş yapılandırmasının bir kopyasını gösteren ekran görüntüsü.](./media/howto-run-a-job/copy-job.png)

## <a name="view-job-status"></a>İş durumunu görüntüleme

Bir iş oluşturulduktan sonra, **durum** sütunu iş için en son durum iletisiyle güncellenir. Aşağıdaki tabloda olası iş durumu değerleri listelenmektedir:

| Durum iletisi       | Durum anlamı                                          |
| -------------------- | ------------------------------------------------------- |
| Tamamlandı            | Bu iş tüm cihazlarda çalıştırıldı.              |
| Başarısız               | Bu iş başarısız oldu ve cihazlarda tam olarak çalıştırılmadı.  |
| Beklemede              | Bu iş henüz cihazlarda çalışmaya başlamadı.         |
| Çalışma              | Bu iş şu anda cihazlarda çalışıyor.             |
| Durduruldu              | Bir Kullanıcı bu işi el ile durdurdu.           |

Durum iletisinin ardından, işteki cihazlara bir genel bakış gönderilir. Aşağıdaki tabloda olası cihaz durumu değerleri listelenmektedir:

| Durum iletisi       | Durum anlamı                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Başarılı            | İşin başarıyla çalıştırıldığı cihazların sayısı.       |
| Başarısız               | İşin üzerinde çalışması başarısız olan cihazların sayısı.       |

İşin durumunu ve etkilenen tüm cihazları görüntülemek için işi açın. Her bir cihaz adının yanında aşağıdaki durum iletilerinden birini görürsünüz:

| Durum iletisi       | Durum anlamı                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Tamamlandı            | İş bu cihazda çalıştırıldı.                                     |
| Başarısız               | Bu cihazda iş çalıştırılamadı. Hata iletisinde daha fazla bilgi gösterilir.  |
| Beklemede              | İş henüz bu cihazda çalıştırılmadı.                                   |

İş ayrıntılarını ve cihazların listesini ve bunların durum değerlerini içeren bir CSV dosyasını indirmek için **İndir**' i seçin.

## <a name="filter-the-device-list"></a>Cihaz listesini filtreleme

Filtre simgesini seçerek, **iş ayrıntıları** sayfasındaki cihaz listesini filtreleyebilirsiniz. **CIHAZ kimliği** veya **durum** alanında filtre uygulayabilirsiniz.

:::image type="content" source="media/howto-run-a-job/filter.png" alt-text="Bir cihaz listesini filtrelemek için seçimleri gösteren ekran görüntüsü.":::

## <a name="customize-columns-in-the-device-list"></a>Cihaz listesindeki sütunları özelleştirme

Sütun Seçenekleri simgesini seçerek cihaz listesinde görüntülenecek ek sütunları seçebilirsiniz.

:::image type="content" source="media/howto-run-a-job/column-options.png" alt-text="Sütun seçeneklerinin simgesini gösteren ekran görüntüsü.":::

İletişim kutusu, cihaz listesinde görüntülenecek sütunları seçmenizi sağlar. Göstermek istediğiniz sütunları seçin, sağ oku seçin ve ardından **Tamam**' ı seçin. Tüm kullanılabilir sütunları seçmek için **Tümünü Seç**' i işaretleyin.

:::image type="content" source="media/howto-run-a-job/column-picker-popup.png" alt-text="Görüntülenecek sütunları seçme iletişim kutusunu gösteren ekran görüntüsü.":::

Seçilen sütunlar cihaz listesinde görünür.

:::image type="content" source="media/howto-run-a-job/column-picker-column-selected.png" alt-text="Cihaz listesinde seçilen sütunları gösteren ekran görüntüsü.":::

Seçili sütunlar, bir kullanıcı oturumunda veya uygulamaya erişimi olan kullanıcı oturumlarında kalıcıdır.

## <a name="rerun-jobs"></a>İşleri yeniden çalıştır

Hatalı cihazları olan bir işi yeniden çalıştırabilirsiniz. **Başarısız olarak yeniden çalıştır '** ı seçin.

:::image type="content" source="media/howto-run-a-job/rerun.png" alt-text="Başarısız cihazlarda bir işi yeniden çalıştırmaya yönelik düğmeyi gösteren ekran görüntüsü.":::

Bir iş adı ve açıklama girin ve ardından **Işi yeniden çalıştır**' ı seçin. Başarısız cihazlarda eylemi yeniden denemek için yeni bir iş gönderilir.

:::image type="content" source="media/howto-run-a-job/rerun-failed.png" alt-text="Başarısız cihazları yeniden çalıştırmaya yönelik iletişim kutusunu gösteren ekran görüntüsü.":::

> [!NOTE]
> Azure IoT Central uygulamasından beş taneden fazla işi aynı anda çalıştıramazsınız.
>
> Bir iş tamamlandığında ve işin cihaz listesindeki bir cihazı sildiğinizde, cihaz girdisi cihaz adında silinmiş olarak görünür. Ayrıntılar bağlantısı, Silinen cihaz için kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda nasıl iş oluşturulacağını öğrendiğinize göre, bazı sonraki adımlar aşağıda verilmiştir:

- [Cihazlarınızı yönetme](howto-manage-devices.md)
- [Cihaz şablonunuzun sürümü](howto-version-device-template.md)
