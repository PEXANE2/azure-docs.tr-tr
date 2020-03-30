---
title: Azure IoT Merkezi uygulamanızda iş oluşturun ve çalıştırın | Microsoft Dokümanlar
description: Azure IoT Merkezi işleri, özellikleri güncelleştirme veya komut yürütme gibi toplu aygıt yönetimi özelliklerine izin verir.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 03/03/2020
ms.topic: how-to
manager: peterpr
ms.openlocfilehash: c9d5a0daa364b09e45699e898511c28d4b4d92ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157764"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Azure IoT Merkezi uygulamanızda bir iş oluşturun ve çalıştırın

Bağlı cihazlarınızı iş lerinizi kullanarak ölçekte yönetmek için Microsoft Azure IoT Central'ı kullanabilirsiniz. İşler, aygıt özelliklerinde toplu güncelleştirmeler yapmanıza ve komutları çalıştırmanıza izin verir. Bu makalede, kendi uygulamanızdaki işleri kullanmaya nasıl başlasınız gösterilmektedir.

## <a name="create-and-run-a-job"></a>İş oluşturma ve çalıştırma

Bu bölümde, bir iş nasıl oluşturulup çalıştırılasınız gösterilmektedir. Bir grup lojistik ağ geçidi aygıtı için ışık eşiğini nasıl ayarlayabileceğinizi gösterir.

1. Sol bölmeden **İşler'e** gidin.

2. Yeni bir iş oluşturmak için **+ Yeni'yi** seçin:

    ![Yeni iş oluşturma](./media/howto-run-a-job/createnewjob.png)

3. Oluşturduğunuz işi tanımlamak için bir ad ve açıklama girin.

4. İşinizin uygulanmasını istediğiniz hedef aygıt grubunu seçin. İş yapılandırmanızın **Özet** bölümünde kaç aygıta uygulandığını görebilirsiniz.

5. Ardından, yapılandırma işi türü olarak **Bulut özelliği,** **Özellik** veya **Komut'u** seçin. **Bir Özellik** iş yapılandırması ayarlamak için bir özellik seçin ve yeni değerini ayarlayın. Bir **Komut**ayarlamak için, çalıştırmak için komutu seçin. Bir özellik işi birden çok özellik ayarlayabilir:

    ![İşi yapılandırma](./media/howto-run-a-job/configurejob.png)

6. İşinizi oluşturduktan sonra **Çalıştır** veya **Kaydet'i**seçin. İş artık ana **İşler** sayfanızda görünür. Bu sayfada, şu anda çalışan işinizi ve daha önce çalıştırılabilen veya kaydedilmiş işlerin geçmişini görebilirsiniz. Kaydedilen işiniz, düzenlemeye devam etmek veya çalıştırmak için herhangi bir zamanda yeniden açılabilir:

    ![İşi görüntüleme](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > Daha önce çalıştırdığınız işler için 30 günlük geçmişi nizi görüntüleyebilirsiniz.

7. İşinize genel bir bakış almak için, listeden görüntülenebilmek için işi seçin. Bu genel bakış, iş ayrıntılarını, aygıtları ve aygıt durum değerlerini içerir. Bu genel bakışta, aygıtlar ve durum değerleri de dahil olmak üzere iş bilgilerinizin csv dosyasını indirmek için **İş Ayrıntılarını İndir'i** de seçebilirsiniz. Bu bilgiler sorun giderme için yararlı olabilir:

    ![Cihaz durumunu görüntüle](./media/howto-run-a-job/downloaddetails.png)

### <a name="manage-a-job"></a>Bir işi yönetme

Çalışan işlerinizden birini durdurmak için açın ve **Durdur'u**seçin. İş ihalini yansıtacak şekilde değişen değişiklikler durdurulur. **Özet** bölümü, hangi aygıtların tamamlandığını, başarısız olduğunu veya beklemede olduğunu gösterir.

Şu anda durdurulan bir işi çalıştırmak için, bu işi seçin ve ardından **Çalıştır'ı**seçin. İş işini yansıtacak şuandırılık durumu değişikliği yeniden çağrıda **Özet** bölümü en son ilerlemeyle birlikte güncelleştirmeye devam ediyor.

![İşi yönetme](./media/howto-run-a-job/managejob.png)

## <a name="copy-a-job"></a>İşi kopyalama

Varolan işlerinizden birini kopyalamak **için, İşler** sayfasında niçin seçin ve **Kopyala'yı**seçin. İş yapılandırmasının bir kopyası düzenlemeniz için açılır ve **Kopya** iş adına eklenir. Yeni işi kaydedebilir veya çalıştırabilirsiniz:

![İşi kopyala](./media/howto-run-a-job/copyjob.png)

## <a name="view-the-job-status"></a>İş durumunu görüntüleme

Bir iş oluşturulduktan sonra Durum **sütunu** işin en son durum iletisiyle güncellenir. Aşağıdaki tabloda olası durum değerleri listelenebilmektedir:

| Durum iletisi       | Durum anlamı                                          |
| -------------------- | ------------------------------------------------------- |
| Tamamlandı            | Bu iş tüm cihazlarda yürütüldü.              |
| Başarısız               | Bu iş başarısız oldu ve cihazlarda tam olarak yürütülmedi.  |
| Beklemede              | Bu iş henüz cihazlarda yürütmeye başlamadı.         |
| Çalışıyor              | Bu iş şu anda aygıtlarda yürütülmektedir.             |
| Durduruldu              | Bu iş bir kullanıcı tarafından el ile durduruldu.           |

Durum iletisini, işteki aygıtların genel bakışı takip edilir. Aşağıdaki tabloda olası aygıt durum değerleri listelenir:

| Durum iletisi       | Durum anlamı                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Başarılı oldu            | İşin başarıyla yürütüldettiği aygıt sayısı.       |
| Başarısız               | İşin yürütemediği aygıt sayısı.       |

### <a name="view-the-device-status"></a>Aygıt durumunu görüntüleme

İşin durumunu ve etkilenen tüm aygıtları görüntülemek için işi açın. Aygıtların listesi ve durum değerleri de dahil olmak üzere iş ayrıntılarını içeren bir CSV dosyasını indirmek için **iş ayrıntılarını karşıdan yükleyin'** i seçin. Her aygıt adının yanında aşağıdaki durum iletilerinden birini görürsünüz:

| Durum iletisi       | Durum anlamı                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Tamamlandı            | İş bu aygıtta yürütüldü.                                     |
| Başarısız               | İş bu aygıtta yürütülemedi. Hata iletisi daha fazla bilgi gösterir.  |
| Beklemede              | İş henüz bu cihazda yürütülmedi.                                   |

> [!NOTE]
> Bir aygıt silinmişse, aygıtı seçemezsiniz. Aygıt kimliğiyle silinmiş olarak görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Merkezi uygulamanızda nasıl iş yaratabileceğinizi öğrendiğinize göre, sonraki adımlar şunlardır:

- [Cihazlarınızı yönetme](howto-manage-devices.md)
- [Cihaz şablonunuzu sürüm](howto-version-device-template.md)
