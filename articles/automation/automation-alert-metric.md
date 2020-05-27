---
title: Azure Otomasyonu runbook 'larını ölçüm uyarıları ile izleme
description: Bu makalede, ölçümler temelinde runbook 'ların nasıl izleneceği açıklanır.
services: automation
ms.date: 11/01/2018
ms.topic: article
ms.openlocfilehash: 20aaee5b699e9721bf9083030604df1385da1915
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83828755"
---
# <a name="monitor-runbooks-with-metric-alerts"></a>Ölçüm uyarıları ile runbook’ları izleme

Bu makalede, runbook 'ların tamamlanma durumuna göre uyarı oluşturmayı öğreneceksiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure'da oturum açın

## <a name="create-alert"></a>Uyarı oluşturma

Uyarılar, izlemek için bir koşul ve bu koşul karşılandığında gerçekleştirilecek bir eylem tanımlamanızı sağlar.

Azure portal Otomasyon hesabınıza gidin. **İzleme**altında **Uyarılar** ' ı seçin ve **+ Yeni uyarı kuralı**' nı tıklatın. Hedefin kapsamı zaten Otomasyon hesabınızla tanımlanmış.

### <a name="configure-alert-criteria"></a>Uyarı ölçütlerini yapılandırma

1. **+ Ölçüt Ekle**' ye tıklayın. **Sinyal türü**için **ölçümler** ' i seçin ve tablodaki **toplam iş** ' ı seçin.

2. **Sinyal mantığını Yapılandır** sayfası, uyarıyı tetikleyen mantığı tanımladığınız yerdir. Geçmiş grafiğinde, **Runbook adı** ve **durum**olmak üzere iki boyut sunulur. Boyutlar, sonuçları filtrelemek için kullanılabilen bir ölçüm için farklı özelliklerdir. **Runbook adı**için, uyarı vermek istediğiniz runbook 'u seçin veya tüm runbook 'larda uyarı almak için boş bırakın. **Durum**için, izlemek istediğiniz açılan listeden bir durum seçin. Açılan listede görünen Runbook adı ve durum değerleri yalnızca geçen hafta çalıştırılan işler içindir.

   Açılan listede görünmeyen bir durum veya Runbook üzerinde uyarı vermek istiyorsanız, **\+** boyutun yanına tıklayın. Bu eylem, bu boyut için yakın zamanda yayılmayan özel bir değer girmenizi sağlayan bir iletişim kutusu açar. Bir özellik için mevcut olmayan bir değer girerseniz, uyarılarınız tetiklenmez.

   > [!NOTE]
   > **Runbookname** boyutu için bir ad uygulamazsanız, gizli sistem runbook 'larını içeren durum ölçütlerine uyan runbook 'lar varsa, bir uyarı alırsınız.

3. **Uyarı mantığı**altında, uyarınızın koşulunu ve eşiğini tanımlayın. Tanımlanmış koşullarınızın önizlemesi aşağıda gösterilmiştir.

4. **Temelinde değerlendirilen**altında sorgu için TimeSpan değerini ve bu sorgunun ne sıklıkta çalıştığını seçin. Örneğin, **Dönem** için **son 5 dakika boyunca** ve **Sıklık**için **1 dakikada** bir seçeneğini belirlerseniz, uyarı, son 5 dakika içinde ölçütlerinizi karşılayan runbook sayısını arar. Bu sorgu her dakikada çalıştırılır ve tanımladığınız uyarı ölçütleri 5 dakikalık bir pencerede artık bulunamazsa, uyarı kendisini çözer. Tamamladığınızda **Bitti**’ye tıklayın.

   ![Uyarı için bir kaynak seçin](./media/automation-alert-activity-log/configure-signal-logic.png)

### <a name="define-alert-details"></a>Uyarı ayrıntılarını tanımlama

1. **2 altında. Uyarı ayrıntılarını tanımlayın**, uyarıya kolay bir ad ve açıklama sağlayın. **Önem derecesini** uyarı koşulunuz ile eşleşecek şekilde ayarlayın. 0 ile 5 arasında beş önem derecesi vardır. Uyarılar önem derecesine göre aynı olarak değerlendirilir, iş mantığınızla eşleşecek şekilde önem derecesini de eşleştirebilirsiniz.

1. Bölümünün en altında, işlemi tamamladıktan sonra kuralı etkinleştirmenizi sağlayan bir düğmedir. Varsayılan olarak kurallar oluşturma sırasında etkinleştirilir. Hayır ' ı seçerseniz, uyarıyı oluşturabilir ve **devre dışı** bir durumda oluşturulur. Azure Izleyici 'deki **kurallar** sayfasında, bunu seçebilir ve hazırsanız uyarıyı etkinleştirmek için **Etkinleştir** ' e tıklayabilirsiniz.

### <a name="define-the-action-to-take"></a>Gerçekleştirilecek eylemi tanımlayın

1. 3 ' ün altında **. Eylem grubunu tanımlayın**, **+ yeni eylem grubu**' na tıklayın. Bir eylem grubu, birden fazla uyarı genelinde kullanabileceğiniz bir eylem grubudur. Bunlar, e-posta bildirimleri, runbook 'lar, Web kancaları ve çok daha fazlasını içerebilir ancak bunlarla sınırlı değildir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md)

1. **Eylem grubu adı** kutusuna bir kolay ad bir de kısa ad yazın. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

1. **Eylem türü**altındaki **Eylemler** bölümünde **e-posta/SMS/Push/Voice**' ı seçin.

1. **E-posta/SMS/Anında İletme/Ses** sayfasında buna bir ad verin. **E-posta** onay kutusunu işaretleyin ve kullanılması için geçerli bir e-posta adresi girin.

   ![E-posta eylem grubunu yapılandırma](./media/automation-alert-activity-log/add-action-group.png)

1. **E-posta/SMS/Anında İletme/Ses** sayfasında **Tamam**’a tıklayarak sayfayı kapatın ve **Eylem grubu ekle** sayfasında **Tamam**’a tıklayarak bu sayfayı da kapatın. Bu sayfada belirtilen ad **eylem adı**olarak kaydedilir.

1. Tamamlandığında **Kaydet**' e tıklayın. Bu eylem, bir runbook belirli bir durumla tamamlandığında sizi uyaran kuralı oluşturur.

> [!NOTE]
> Bir eylem grubuna e-posta adresi eklerken, adresi bir eylem grubuna eklendiğini bildiren bir bildirim e-postası gönderilir.

## <a name="receive-notification"></a>Bildirim al

Uyarı ölçütleri karşılandığında, eylem grubu tanımlanan eylemi çalıştırır. Bu makalenin örneğinde, bir e-posta gönderilir. Aşağıdaki görüntü, uyarı tetiklendikten sonra aldığınız e-postanın bir örneğidir:

![E-posta uyarısı](./media/automation-alert-activity-log/alert-email.png)

Ölçüm artık tanımlanan eşik dışında olmadığında, uyarı devre dışı bırakılır ve eylem grubu tanımlanan eylemi çalıştırır. E-posta eylemi türü seçilirse, Çözümlenmiş olduğunu bildiren bir çözüm e-postası gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi için bkz. [Azure Otomasyonu runbook 'u tetiklemek için uyarı kullanma](automation-create-alert-triggered-runbook.md).
