---
title: Azure IoT Central uygulamanızda iş oluşturma ve çalıştırma | Microsoft Docs
description: Azure IoT Central işleri, özellikleri güncelleştirme veya bir komutu yürütme gibi toplu cihaz yönetimi özelliklerine izin verir.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 06/08/2020
ms.topic: how-to
ms.openlocfilehash: 6073f71eb21ba4a6739647964d4888044d6ee59a
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87283731"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Azure IoT Central uygulamanızda iş oluşturma ve çalıştırma

Bağlı cihazlarınızı işler kullanarak ölçeklendirerek yönetmek için Microsoft Azure IoT Central kullanabilirsiniz. İşler, cihaz özelliklerine toplu güncelleştirmeler yapmanızı ve komutları çalıştırmanızı sağlar. Bu makalede, kendi uygulamanızdaki işleri kullanmaya nasıl başlacağınız gösterilmektedir.

## <a name="create-and-run-a-job"></a>İş oluşturma ve çalıştırma

Bu bölümde bir işi oluşturma ve çalıştırma işlemi gösterilmektedir. Bir lojistik ağ geçidi cihazı grubu için ışığın eşiğini nasıl ayarlayagösterdiğini gösterir.

1. Sol bölmedeki **işler** ' e gidin.

2. Yeni bir iş oluşturmak için **+ Yeni** ' yi seçin:

    ![Yeni iş oluştur](./media/howto-run-a-job/create-new-job.png)

3. Oluşturmakta olduğunuz işi tanımlamak için bir ad ve açıklama girin.

4. İşinizin uygulanmasını istediğiniz hedef cihaz grubunu seçin. **Özet** bölümünde iş yapılandırmanızın kaç cihaz için geçerli olduğunu görebilirsiniz.

5. Sonra, yapılandırılacak iş türü olarak **bulut özelliğini**, **özelliği**veya **komutunu** seçin. Bir **özellik** işi yapılandırması ayarlamak için bir özellik seçin ve yeni değerini ayarlayın. Bir **komut**ayarlamak için çalıştırılacak komutu seçin. Bir özellik işi birden çok özellik ayarlayabilir:

    ![İşi yapılandırma](./media/howto-run-a-job/configure-job.png)

6. İşinizi oluşturduktan sonra **Çalıştır** veya **Kaydet**' i seçin. İş artık ana **işler** sayfanızda görüntülenir. Bu sayfada, çalışmakta olan işinizi ve daha önce çalıştırılan veya kaydedilmiş işlerin geçmişini görebilirsiniz. Kayıtlı işiniz herhangi bir zamanda yeniden açılabilir veya çalışmaya devam edebilir:

    ![İşi görüntüle](./media/howto-run-a-job/view-job.png)

    > [!NOTE]
    > Daha önce çalıştırılan işleriniz için 30 günlük geçmişi görüntüleyebilirsiniz.

7. Kaydedilen işe tıklayın ve Çalıştır düğmesine tıklayarak işi yürütün. Çalışan bir iş açılır penceresi görüntülenir. Işi Çalıştır düğmesine tıklayarak onaylayın. 

    ![Iş çalıştırma](./media/howto-run-a-job/run-job.png)

8. İş, bekleyen, çalışan ve tamamlanan farklı aşamalarda geçiyor. İş yürütme ayrıntıları, sonuç ölçümleri, süre ayrıntıları ve cihaz listesi kılavuzunu içerir. Bu genel bakışta, cihazlar ve bunların durum değerleri dahil olmak üzere iş ayrıntılarınızın CSV dosyasını indirmek için **sonuç günlüğü** ' nü de seçebilirsiniz. Bu bilgiler, sorun giderme için yararlı olabilir.

    ![Cihaz durumunu görüntüle](./media/howto-run-a-job/download-details.png)

## <a name="manage-jobs"></a>İşleri yönetme

Çalışan işlerinizin birini durdurmak için açın ve **Durdur**' u seçin. İş durumu işi yansıtacak şekilde değişir. **Özet** bölümü, hangi cihazların tamamlandığını, başarısız olduğunu veya hala beklendiğini gösterir.

![İşi Yönet](./media/howto-run-a-job/manage-job.png)

Iş durdurulmuş durumdaysa, iş yürütmeyi sürdürmek için **devam** ' a tıklayabilirsiniz. İş durumu işi yansıtacak şekilde değişir. **Özet** bölümü en son ilerleme ile güncelleştirilmeye devam eder.

![Durdurulan Iş](./media/howto-run-a-job/stopped-job.png)

## <a name="copy-a-job"></a>İşi kopyalama

Mevcut işlerinizin birini kopyalamak için **işler** sayfasında bunu seçin ve **iş ayrıntıları**' nı seçin. İş ayrıntıları sayfası görüntülenir. 

![İş Ayrıntıları](./media/howto-run-a-job/job-details.png)

**Kopyala** öğesine tıklayın

![İş Ayrıntıları](./media/howto-run-a-job/job-details-copy.png)

Düzenlemeniz için iş yapılandırmasının bir kopyası açılır ve **kopyalama** işi adına eklenir. Yeni işi kaydedebilir veya çalıştırabilirsiniz:

![İşi Kopyala](./media/howto-run-a-job/copy-job.png)

## <a name="view-job-status"></a>İş durumunu görüntüleme

Bir iş oluşturulduktan sonra **durum** sütunu, işin en son durum iletisiyle güncellenir. Aşağıdaki tabloda olası durum değerleri listelenmektedir:

| Durum iletisi       | Durum anlamı                                          |
| -------------------- | ------------------------------------------------------- |
| Tamamlandı            | Bu iş tüm cihazlarda yürütüldü.              |
| Başarısız               | Bu iş başarısız oldu ve cihazlarda tam olarak yürütülmedi.  |
| Beklemede              | Bu iş henüz cihazlarda yürütülmeye başlamadı.         |
| Çalışıyor              | Bu iş şu anda cihazlarda yürütülüyor.             |
| Durduruldu              | Bu iş, bir kullanıcı tarafından el ile durduruldu.           |

Durum iletisinin ardından, işteki cihazlara bir genel bakış gönderilir. Aşağıdaki tabloda olası cihaz durumu değerleri listelenmektedir:

| Durum iletisi       | Durum anlamı                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Başarılı            | İşin başarıyla yürütüldüğü cihazların sayısı.       |
| Başarısız               | İşin üzerinde yürütüleyemedi cihaz sayısı.       |

### <a name="view-the-device-status-values"></a>Cihaz durumu değerlerini görüntüleme

İşin durumunu ve etkilenen tüm cihazları görüntülemek için işi açın. Her bir cihaz adının yanında aşağıdaki durum iletilerinden birini görürsünüz:

| Durum iletisi       | Durum anlamı                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Tamamlandı            | Bu cihazda yürütülen iş.                                     |
| Başarısız               | İşin bu cihazda yürütmesi başarısız oldu. Hata iletisinde daha fazla bilgi gösterilir.  |
| Beklemede              | İş bu cihazda henüz yürütülmedi.                                   |

İş ayrıntılarını ve cihazların listesini ve bunların durum değerlerini içeren bir CSV dosyasını indirmek için **İndir**' i seçin.

### <a name="filter-the-list-of-devices"></a>Cihaz listesini filtrele

Filtre simgesini seçerek, iş ayrıntıları sayfasındaki cihaz listesini filtreleyebilirsiniz. **CIHAZ kimliği** veya **durum** alanlarına filtre uygulayabilirsiniz:

:::image type="content" source="media/howto-run-a-job/filter.png" alt-text="Cihaz listesini filtreleme":::

### <a name="customize-columns-in-the-device-list"></a>Cihaz listesindeki sütunları özelleştirme

Sütun Seçenekleri simgesini seçerek cihaz listesinde görüntülenecek ek sütunları seçebilirsiniz:

:::image type="content" source="media/howto-run-a-job/column-options.png" alt-text="Sütun seçenekleri":::

Cihaz listesinde görüntülenecek sütunları seçmenizi sağlayan bir iletişim kutusu görürsünüz. Göstermek istediğiniz sütunları seçin, sağ ok simgesini ve **Tamam**seçeneğini belirleyin. Tüm kullanılabilir sütunları seçmek için, **Tümünü Seç**' i işaretleyin.

:::image type="content" source="media/howto-run-a-job/column-picker-popup.png" alt-text="Sütun Seçici iletişim kutusu":::

Seçilen sütunlar cihaz listesinde görüntülenir:

:::image type="content" source="media/howto-run-a-job/column-picker-column-selected.png" alt-text="Sütun seçme":::

Seçili sütunlar, bir kullanıcı oturumunda veya uygulamaya erişimi olan kullanıcı oturumlarında kalıcıdır.

## <a name="rerun-jobs"></a>İşleri yeniden çalıştır

Hatalı cihazları olan bir işi yeniden çalıştırabilirsiniz. **Yeniden çalıştır**' ı seçin:

:::image type="content" source="media/howto-run-a-job/rerun.png" alt-text="İşi yeniden çalıştırma":::

Bir iş adı ve açıklama girin ve ardından **Işi yeniden çalıştır**' ı seçin. Başarısız cihazlarda eylemi yeniden denemek için yeni bir iş gönderildi:

:::image type="content" source="media/howto-run-a-job/rerun-failed.png" alt-text="Başarısız cihazları yeniden çalıştır":::

> [!NOTE]
> IoT Central uygulamasından daha fazla işi aynı anda çalıştıramazsınız.

> [!NOTE]
> Bir iş tamamlandığında ve işin cihaz listesindeki bir cihazı sildiğinizde, cihaz girişinin cihaz adı ve cihaz ayrıntıları bağlantısı silinmiş cihaz için kullanılamaz olarak gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda nasıl iş oluşturulacağını öğrendiğinize göre, bazı sonraki adımlar aşağıda verilmiştir:

- [Cihazlarınızı yönetme](howto-manage-devices.md)
- [Cihaz şablonunuzun sürümü](howto-version-device-template.md)
