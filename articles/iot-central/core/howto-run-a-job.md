---
title: Azure IoT Central uygulamanızda iş oluşturma ve çalıştırma | Microsoft Docs
description: Azure IoT Central işleri, özelliklerinin güncelleştirilmesi veya bir komutun çalıştırılması gibi toplu cihaz yönetimi özelliklerine izin verir.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 09/10/2020
ms.topic: how-to
ms.openlocfilehash: ae8b830469a9b52ae68310dde2e65dcffdf4e3be
ms.sourcegitcommit: 51df05f27adb8f3ce67ad11d75cb0ee0b016dc5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90060824"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Azure IoT Central uygulamanızda iş oluşturma ve çalıştırma

Bağlı cihazlarınızı işler arasında ölçeklendirerek yönetmek için Azure IoT Central kullanabilirsiniz. İşler, cihaz ve bulut özelliklerine toplu güncelleştirmeler yapmanızı ve komutları çalıştırmanızı sağlar. Bu makalede, kendi uygulamanızdaki işleri kullanmaya nasıl başlacağınız gösterilmektedir.

## <a name="create-and-run-a-job"></a>İş oluşturma ve çalıştırma

Aşağıdaki örnek, bir lojistik ağ geçidi cihazı grubu için ışık eşiğini ayarlamak üzere bir işi nasıl oluşturup çalıştıracağınızı gösterir. İşleri oluşturmak ve çalıştırmak için iş Sihirbazı 'nı kullanırsınız. Daha sonra çalıştırmak için bir işi kaydedebilirsiniz.

1. Sol bölmede **işler**' i seçin.

1. **+ Yeni iş**' ı seçin.

1. **Işinizi yapılandırın** sayfasında, oluşturmakta olduğunuz işi tanımlamak için bir ad ve açıklama girin.

1. İşinizin uygulanmasını istediğiniz hedef cihaz grubunu seçin. İş yapılandırmanızın **cihaz grubu** seçiminizin altına göre kaç cihaz uygulanacağını görebilirsiniz.

1. **İş türü**olarak **bulut özelliğini**, **özelliği**veya **komutunu** seçin:

    Bir **özellik** işi yapılandırması ayarlamak için bir özellik seçin ve yeni değerini ayarlayın. Bir **komut** iş yapılandırması ayarlamak için çalıştırılacak komutu seçin. Bir özellik işi birden çok özellik ayarlayabilir.

    :::image type="content" source="media/howto-run-a-job/configure-job.png" alt-text="Hafif eşik kümesi adlı bir özellik işi oluşturma seçimlerini gösteren ekran görüntüsü":::

    İşi **işler** sayfasındaki kaydedilen işler listesine eklemek için **Kaydet ve çık ' ı** seçin. Daha sonra kaydedilen işler listesinden bir işe dönebilirsiniz.

    **İnceleme** sayfasına gitmek için **İleri ' yi** seçin. **İnceleme** sayfasında iş yapılandırması ayrıntıları gösterilir. İşi göndermek için **Çalıştır** ' ı seçin.

    :::image type="content" source="media/howto-run-a-job/job-wizard-review.png" alt-text="İş Sihirbazı inceleme sayfasının ekran görüntüsü":::

1. Bir iş, *bekleyen*, *çalışan*ve *tamamlanmış* aşamalardan geçer. İş yürütme ayrıntıları, sonuç ölçümlerini, süre ayrıntılarını ve bir cihaz listesi kılavuzunu içerir.

    İş tamamlandığında, cihazlar ve bunların durum değerleri dahil olmak üzere iş ayrıntılarınızın CSV dosyasını indirmek için **sonuçlar günlüğünü** seçebilirsiniz. Bu bilgiler, sorun giderme için yararlı olabilir.

    :::image type="content" source="media/howto-run-a-job/download-details.png" alt-text="Cihaz durumunu gösteren ekran görüntüsü":::

1. İş artık **işler** sayfasında **son 30 gün** listesinde görüntülenir. Bu sayfa, çalışmakta olan işleri ve daha önce çalışan veya kaydedilmiş işlerin geçmişini gösterir.

    > [!NOTE]
    > Daha önce çalıştırılan işleriniz için 30 günlük geçmişi görüntüleyebilirsiniz.

## <a name="manage-jobs"></a>İşleri yönetme

Çalışan bir işi durdurmak için açın ve **Durdur**' u seçin. İş durumu, işin durdurulduğunu yansıtacak şekilde değişir. **Özet** bölümü, hangi cihazların tamamlandığını, başarısız olduğunu veya hala beklendiğini gösterir.

:::image type="content" source="media/howto-run-a-job/manage-job.png" alt-text="Çalışan bir işi ve işi durdurma düğmesini gösteren ekran görüntüsü":::

Bir iş durdurulmuş durumdaysa, işi çalıştırmaya devam etmek için **devam** ' ı seçebilirsiniz. İş durumu, işin şimdi yeniden çalıştığını yansıtacak şekilde değişir. **Özet** bölümü en son ilerleme ile güncelleştirilmeye devam eder.

:::image type="content" source="media/howto-run-a-job/stopped-job.png" alt-text="Durdurulmuş bir işi ve bir işe devam eden düğmeyi gösteren ekran görüntüsü":::

## <a name="copy-a-job"></a>İşi kopyalama

Var olan bir işi kopyalamak için yürütülen bir işi seçin. İş sonuçları sayfası veya iş ayrıntıları sayfasında **Kopyala** ' yı seçin:

:::image type="content" source="media/howto-run-a-job/job-details-copy.png" alt-text="Kopyala düğmesini gösteren ekran görüntüsü":::

Düzenlemeniz için iş yapılandırmasının bir kopyası açılır ve **kopyalama** işi adına eklenir.

## <a name="view-job-status"></a>İş durumunu görüntüleme

Bir iş oluşturulduktan sonra **durum** sütunu en son iş durum iletisiyle güncellenir. Aşağıdaki tabloda olası *iş durumu* değerleri listelenmektedir:

| Durum iletisi       | Durum anlamı                                          |
| -------------------- | ------------------------------------------------------- |
| Tamamlandı            | Bu iş tüm cihazlarda çalıştırıldı.              |
| Başarısız               | Bu iş başarısız oldu ve cihazlarda tam olarak çalıştırılmadı.  |
| Beklemede              | Bu iş henüz cihazlarda çalışmaya başlamadı.         |
| Çalışma              | Bu iş şu anda cihazlarda çalışıyor.             |
| Durduruldu              | Bir Kullanıcı bu işi el ile durdurdu.           |

Durum iletisinin ardından, işteki cihazlara bir genel bakış gönderilir. Aşağıdaki tabloda olası *cihaz durumu* değerleri listelenmektedir:

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

İş ayrıntılarını ve cihazların listesini ve bunların durum değerlerini içeren bir CSV dosyasını indirmek için **sonuçlar günlüğü**' nü seçin.

## <a name="filter-the-device-list"></a>Cihaz listesini filtreleme

Filtre simgesini seçerek, **iş ayrıntıları** sayfasındaki cihaz listesini filtreleyebilirsiniz. **CIHAZ kimliği** veya **durum** alanında filtre uygulayabilirsiniz:

:::image type="content" source="media/howto-run-a-job/filter.png" alt-text="Bir cihaz listesini filtrelemek için seçimleri gösteren ekran görüntüsü.":::

## <a name="customize-columns-in-the-device-list"></a>Cihaz listesindeki sütunları özelleştirme

Sütun Seçenekleri simgesini seçerek cihaz listesine sütun ekleyebilirsiniz:

:::image type="content" source="media/howto-run-a-job/column-options.png" alt-text="Sütun seçeneklerinin simgesini gösteren ekran görüntüsü.":::

Cihaz listesi sütunlarını seçmek için **sütun seçenekleri** iletişim kutusunu kullanın. Göstermek istediğiniz sütunları seçin, sağ oku seçin ve ardından **Tamam**' ı seçin. Tüm kullanılabilir sütunları seçmek için **Tümünü Seç**' i seçin. Seçilen sütunlar cihaz listesinde görünür.

Seçili sütunlar, bir kullanıcı oturumunda veya uygulamaya erişimi olan kullanıcı oturumlarında kalır.

## <a name="rerun-jobs"></a>İşleri yeniden çalıştır

Hatalı cihazları olan bir işi yeniden çalıştırabilirsiniz. **Başarısız olarak yeniden çalıştır '** ı seçin:

:::image type="content" source="media/howto-run-a-job/rerun.png" alt-text="Başarısız cihazlarda bir işi yeniden çalıştırmaya yönelik düğmeyi gösteren ekran görüntüsü.":::

Bir iş adı ve açıklama girin ve ardından **Işi yeniden çalıştır**' ı seçin. Başarısız cihazlarda eylemi yeniden denemek için yeni bir iş gönderilir.

> [!NOTE]
> Azure IoT Central uygulamasından beş taneden fazla işi aynı anda çalıştıramazsınız.
>
> Bir iş tamamlandığında ve işin cihaz listesindeki bir cihazı sildiğinizde, cihaz girdisi cihaz adında silinmiş olarak görünür. Ayrıntılar bağlantısı, Silinen cihaz için kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda nasıl iş oluşturulacağını öğrendiğinize göre, bazı sonraki adımlar aşağıda verilmiştir:

- [Cihazlarınızı yönetme](howto-manage-devices.md)
- [Cihaz şablonunuzun sürümü](howto-version-device-template.md)
