---
title: Azure Otomasyon runbook'larını metrik uyarılarla izleyin
description: Bu makale, ölçümlere dayalı Azure Otomasyon uyrularını izleme konusunda size yol
services: automation
ms.date: 11/01/2018
ms.topic: article
ms.openlocfilehash: f288029bb35fe4e3c71db37a1de265edbcd913b1
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310551"
---
# <a name="monitoring-runbooks-with-metric-alerts"></a>Çalışma defterlerini metrik uyarılarla izleme

Bu makalede, runbook'ların tamamlanma durumuna bağlı olarak nasıl uyarı oluşturulacak larını öğrenirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure'da oturum açın

## <a name="create-alert"></a>Uyarı oluşturma

Uyarılar, izlenecek bir koşul ve bu koşul yerine getirildiğinde yapılacak bir eylem tanımlamanıza olanak sağlar.

Azure portalında Otomasyon hesabınıza gidin. **İzleme**altında, **Uyarılar'ı** seçin ve **+ Yeni Uyarı Kuralı'nı**tıklatın. Hedefin kapsamı otomasyon hesabınıza zaten tanımlanmıştır.

### <a name="configure-alert-criteria"></a>Uyarı ölçütlerini yapılandırma

1. + **Ölçüt ekle'yi**tıklatın. Sinyal türü için **Ölçümler'i** seçin ve tablodan **Toplam İşler'i** seçin. **Signal type**

2. **Yapılaşı sinyali mantığı** sayfası, uyarıyı tetikleyen mantığı tanımladığınız yerdir. Tarihsel grafik altında iki boyut, **Runbook Adı** ve **Durum**ile sunulmaktadır. Boyutlar, sonuçları filtrelemek için kullanılabilecek bir metrik için farklı özelliklerdir. **Runbook Adı**için, uyarmak istediğiniz runbook'u seçin veya tüm runbook'larda uyarmak için boş bırakın. **Durum**için, izlemek istediğiniz açılır yerden bir durum seçin. Açılan defterde görünen runbook adı ve durum değerleri yalnızca geçen hafta çalışan işler içindir.

   Açılır açılır da gösterilmeyen bir durum veya runbook üzerinde uyarıda bulunan **\+** lar varsa, boyutun yanındaki nitu tıklatın. Bu eylem, son zamanlarda bu boyut için yayılan değil özel bir değer, girmenizi sağlayan bir iletişim açar. Bir özellik için var olmayan bir değer girerseniz, uyarınız tetiklenmez.

   > [!NOTE]
   > **RunbookName** boyutu için bir ad uygulamazsanız, gizli sistem runbook'larını içeren durum ölçütlerini karşılayan runbook'lar varsa, bir uyarı alırsınız.

3. **Alert mantığı**altında, uyarınızın koşulunu ve eşiğini tanımlayın. Tanımlanan durumunuzun önizlemesi aşağıda gösterilir.

4. **Değerlendirildi'ye göre,** sorgu için zaman lığını ve bu sorgunun ne sıklıkta çalıştırılsın' ı seçin. Örneğin, **Dönem** için **son 5 dakika ve** **Sıklık**için **Her 1 Dakika'yı** seçerseniz, uyarı son 5 dakika içinde ölçütlerinizi karşılayan runbook sayısını arar. Bu sorgu her dakika çalıştırılır ve tanımladığınız uyarı ölçütleri 5 dakikalık bir pencerede artık bulunamaz, uyarı kendiliğinden çözülür. Tamamladığınızda **Bitti**’ye tıklayın.

   ![Uyarı için bir kaynak seçme](./media/automation-alert-activity-log/configure-signal-logic.png)

### <a name="define-alert-details"></a>Uyarı ayrıntılarını tanımlama

1. **2'nin altında. Uyarı ayrıntılarını tanımlayın,** uyarıya dostça bir ad ve açıklama verin. Önem **Derecesini** uyarı durumunuza uyacak şekilde ayarlayın. 0 ile 5 arasında değişen beş şiddeti vardır. Uyarılar önem açısından aynı şekilde işlenir, iş mantığınıza uyacak şekilde önem derecesi eşleşebilirsiniz.

1. Bölümün alt kısmında, tamamlandıktan sonra kuralı etkinleştirmek için izin veren bir düğmedir. Varsayılan kurallar oluşturma da etkinleştirilir. Hayır'ı seçerseniz, uyarıyı oluşturabilirsiniz ve bu uyarı **Devre Dışı bırakılmış** durumda oluşturulur. Azure Monitor'daki **Kurallar** sayfasından, hazır olduğunuzda uyarıyı etkinleştirmek için bu sayfayı seçebilir ve **Etkinleştir'i** tıklatabilirsiniz.

### <a name="define-the-action-to-take"></a>Yapılacak eylemi tanımlama

1. **3'ün altında. Eylem grubunu tanımla**, tıklayın **+ Yeni eylem grubu**. Eylem grubu, birden fazla uyarıda kullanabileceğiniz bir eylem grubudur. Bunlar, e-posta bildirimlerini, runbook'ları, web hook'ları ve daha fazlasını içerebilir, ancak bunlarla sınırlı değildir. Eylem grupları hakkında daha fazla bilgi edinmek için [bkz.](../azure-monitor/platform/action-groups.md)

1. **Eylem grubu adı** kutusuna bir kolay ad bir de kısa ad yazın. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

1. **ACTION TYPE**altındaki **Eylemler** bölümünde **E-posta/SMS/Push/Voice'ı**seçin.

1. **E-posta/SMS/Anında İletme/Ses** sayfasında buna bir ad verin. **E-posta** onay kutusunu işaretleyin ve kullanılması için geçerli bir e-posta adresi girin.

   ![E-posta eylem grubunu yapılandırma](./media/automation-alert-activity-log/add-action-group.png)

1. **E-posta/SMS/Anında İletme/Ses** sayfasında **Tamam**’a tıklayarak sayfayı kapatın ve **Eylem grubu ekle** sayfasında **Tamam**’a tıklayarak bu sayfayı da kapatın. Bu sayfada belirtilen Ad EYLEM **ADI**olarak kaydedilir.

1. Tamamlandığında **Kaydet'i**tıklatın. Bu eylem, belirli bir durumla tamamlanan bir runbook'ta sizi uyaran bir kural oluşturur.

> [!NOTE]
> Bir Eylem Grubuna e-posta adresi eklerken, bir Eylem Grubu'na adresin eklendiğini belirten bir bildirim e-postası gönderilir.

## <a name="notification"></a>Bildirim

Uyarı ölçütleri karşılandığında, eylem grubu tanımlanan eylemi çalıştırZ. Bu makalenin örneğinde, bir e-posta gönderilir. Aşağıdaki resim, uyarı tetiklendikten sonra aldığınız bir e-postanın bir örneğidir:

![E-posta uyarısı](./media/automation-alert-activity-log/alert-email.png)

Metrik tanımlanan eşiğin dışına artık çıktıktan sonra, uyarı devre dışı bırakılır ve eylem grubu tanımlanan eylemi çalıştırAr. Bir e-posta eylem türü seçilirse, çözüldüğünü belirten bir çözüm e-postası gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

Uyarıları Otomasyon hesabınıza entegre etmenin diğer yolları hakkında bilgi edinmek için aşağıdaki makaleye devam edin.

> [!div class="nextstepaction"]
> [Azure Otomasyonu runbook'u tetiklemek için uyarı kullanma](automation-create-alert-triggered-runbook.md)
