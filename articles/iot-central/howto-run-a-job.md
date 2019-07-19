---
title: Azure IoT Central uygulamanızda iş oluşturma ve çalıştırma | Microsoft Docs
description: Azure IoT Central işleri, bir cihaz özelliğini güncelleştirme, ayarlama veya bir komutu yürütme gibi toplu cihaz yönetimi özelliklerine izin verir.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 07/08/2019
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: 298770b1b2da816ddef9154fafb20d7c6cb82df3
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849033"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Azure IoT Central uygulamanızda iş oluşturma ve çalıştırma

Bağlı cihazlarınızı işler kullanarak ölçeklendirerek yönetmek için Microsoft Azure IoT Central kullanabilirsiniz. İşler cihaz özelliklerine, ayarlarına ve komutlarına toplu güncelleştirmeler yapmanızı sağlar. Bu makalede, kendi uygulamanızdaki işleri kullanmaya nasıl başlayacağınız anlatılmaktadır.

## <a name="create-and-run-a-job"></a>İş oluşturma ve çalıştırma

Bu bölümde bir işi oluşturma ve çalıştırma işlemi gösterilmektedir. Bu, birden çok soğutma makinesi için fan hızını nasıl artırabileceğiniz gösterilmektedir.

1. Gezinti bölmesinden Işler ' e gidin.

1. Yeni bir iş oluşturmak için **+ Yeni** ' yi seçin.

    ![Yeni iş oluştur](./media/howto-run-a-job/createnewjob.png)

1. Oluşturmakta olduğunuz işi tanımlamak için bir ad ve açıklama girin.

1. İşinizin uygulanmasını istediğiniz cihaz kümesini seçin. Cihaz kümesini seçtikten sonra, cihaz kümesindeki cihazlarla sağ taraftaki doldurma görürsünüz. Bozuk bir cihaz kümesi seçerseniz, hiçbir cihaz görüntülenmez ve cihazınızın ayarlanmış olduğunu belirten bir ileti görürsünüz.

1. Sonra, tanımlanacak iş türünü (bir ayar, özellik veya komut) seçin. Seçilen **+** iş türünün ileri ' yi seçin ve işlemlerinizi ekleyin.

    ![İşi yapılandırma](./media/howto-run-a-job/configurejob.png)

1. Sağ tarafta, işi çalıştırmak istediğiniz cihazları seçin. Üstteki onay kutusunu seçerek tüm cihaz kümesi tüm cihazlar seçilir. **Ad**' ın yanındaki onay kutusunu seçerek, geçerli sayfadaki tüm cihazlar seçilir.

1. Cihazlarınızı seçtikten sonra **Çalıştır** veya **Kaydet**' i seçin. İş artık ana **işler** sayfanızda görüntülenir. Bu görünümde, çalışmakta olan işinizi ve daha önce çalıştırılan işlerin geçmişini görebilirsiniz. Çalışan işiniz her zaman listenin en üstünde görünür. Kaydetme veya çalışmaya devam etmek için kaydettiğiniz iş dilediğiniz zaman yeniden açılabilir.

    ![İşi görüntüle](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > Daha önce çalıştırılan işlerinizin geçmişini 30 güne kadar görüntüleyebilirsiniz.

1. İşiniz hakkında genel bilgi almak için listeden görüntülenecek işi seçin. Bu genel bakışta iş ayrıntıları, cihazlar ve cihaz durumu değerleri bulunur. Bu genel bakışta, iş ayrıntılarınızın bir. csv dosyasını indirmek için, cihazlar ve bunların durum değerleri de dahil olmak üzere **Iş ayrıntılarını indir** ' i de seçebilirsiniz. Bu bilgiler, sorun giderme için yararlı olabilir.

    ![Cihaz durumunu görüntüle](./media/howto-run-a-job/downloaddetails.png)

### <a name="stop-a-running-job"></a>Çalışan bir işi durdur

Çalışan bir işi durdurmak için, seçin ve panelde **Durdur** ' u seçin. İş durumu işi yansıtacak şekilde değişir.

   ![İşi durdur](./media/howto-run-a-job/stopjob.png)

### <a name="run-a-stopped-job"></a>Durdurulmuş bir işi çalıştırma

Şu anda durdurulmuş bir işi çalıştırmak için durdurulan işi seçin. Panelde **Çalıştır** ' ı seçin. İş durumu işi yansıtacak şekilde değişir.

   ![İş sürdürülüyor](./media/howto-run-a-job/resumejob.png)

## <a name="copy-a-job"></a>İşi kopyalama

Oluşturduğunuz mevcut bir işi kopyalamak için ana işler sayfasından seçin ve **Kopyala**' yı seçin. Düzenlemeniz için iş yapılandırmasının yeni bir kopyası açılır. Yeni işi kaydedebilir veya çalıştırabilirsiniz. Seçtiğiniz cihaz kümesinde herhangi bir değişiklik yapıldıysa, bunlar düzenlemeniz için bu kopyalanmış işe yansıtılır.

   ![İşi Kopyala](./media/howto-run-a-job/copyjob.png)

## <a name="view-the-job-status"></a>İş durumunu görüntüleme

Bir iş oluşturulduktan sonra **durum** sütunu, işin en son durum iletisiyle güncellenir. Aşağıdaki tabloda olası durum değerleri listelenmektedir:

| Durum iletisi       | Durum anlamı                                          |
| -------------------- | ------------------------------------------------------- |
| Tamamlandı            | Bu iş tüm cihazlarda yürütüldü.              |
| Başarısız               | Bu iş başarısız oldu ve cihazlarda tam olarak yürütülmedi.  |
| Bekleniyor              | Bu iş henüz cihazlarda yürütülmeye başlamadı.         |
| Çalışıyor              | Bu iş şu anda cihazlarda yürütülüyor.             |
| Durduruldu              | Bu iş, bir kullanıcı tarafından el ile durduruldu.           |

Durum iletisinin ardından, işteki cihazlara bir genel bakış gönderilir. Aşağıdaki tabloda olası cihaz durumu değerleri listelenmektedir:

| Durum iletisi       | Durum anlamı                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Başarılı oldu            | İşin başarıyla yürütüldüğü cihazların sayısı.       |
| Başarısız               | İşin üzerinde yürütüleyemedi cihaz sayısı.       |

### <a name="view-the-device-status"></a>Cihaz durumunu görüntüleme

İşin durumunu ve etkilenen tüm cihazları görüntülemek için işi seçin. Cihazların listesi ve durum değerleri dahil olmak üzere iş ayrıntılarını içeren bir. csv dosyasını indirmek için, **iş ayrıntılarını indir**' i seçin. Her bir cihaz adının yanında aşağıdaki durum iletilerinden birini görürsünüz:

| Durum iletisi       | Durum anlamı                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Tamamlandı            | Bu cihazda iş yürütüldü.                                     |
| Başarısız               | İşin bu cihazda yürütmesi başarısız oldu. Hata iletisinde daha fazla bilgi gösterilir.  |
| Bekleniyor              | İş bu cihazda henüz yürütülmedi.                                   |

> [!NOTE]
> Bir cihaz silinmişse cihazı seçemezsiniz ve cihaz KIMLIĞIYLE silinmiş olarak görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda nasıl iş oluşturulacağını öğrendiğinize göre, bazı sonraki adımlar aşağıda verilmiştir:

- [Cihaz kümelerini kullanma](howto-use-device-sets.md)
- [Cihazlarınızı yönetme](howto-manage-devices.md)
- [Cihaz şablonunuzun sürümü](howto-version-device-template.md)
