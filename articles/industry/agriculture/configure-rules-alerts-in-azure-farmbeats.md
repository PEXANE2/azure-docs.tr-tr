---
title: Kuralları yapılandırma ve uyarıları yönetme
description: FarmBeats'te kuralları yapılandırmave uyarıları yönetme
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 02373da114b6dc4663cd3ffc9a0780ac6a7a9e23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75482991"
---
# <a name="configure-rules-and-manage-alerts"></a>Kuralları yapılandırma ve uyarıları yönetme

Azure FarmBeats, çiftliğinizde dağıtılan sensörlerden ve aygıtlardan akan sensör verilerine ek olarak iş mantığına dayalı kurallar oluşturmanıza olanak tanır. Kurallar, sensör değerleri bir eşik değerini geçtiğinde sistemdeki uyarıları tetikler. Eşik değerlerinden sonra oluşturulan uyarıları görüntüleyip çözümleyerek, herhangi bir sorunda hızlı bir şekilde hareket edebilir ve gerekli çözümleri alabilirsiniz.

## <a name="create-rule"></a>Kural oluşturma

1. Ana sayfada **Kurallar'a**gidin.
2. **Yeni Kural'ı**seçin. Yeni Kural penceresi görüntülenir.

    ![Proje Çiftlik Beats](./media/configure-rules-and-alerts-in-azure-farmbeats/new-rule-1.png)

3. Kural **Adı** ve **Kural Açıklaması'nı** girin ve ardından **Çiftlik** açılır menüsünden bir çiftlik seçin.
4. Çiftlik ve **Koşullar** bölümünü seçmek için çiftlik adınızı yazın aynı pencerede görünür.  

    ![Proje Çiftlik Beats](./media/configure-rules-and-alerts-in-azure-farmbeats/new-rule-condition-1.png)

5. **Koşullarda**, **Ölçü**, **İşleç** ve **Değer**değerlerini girin.
6. **Ölçü** açılır menüsüne ölçü adını yazın.
7. Kurala daha fazla koşul eklemek için **+Ekle Koşul'u** seçin.
8. Önem **Düzeyi'ni**seçin.
9. **Eylem'de,** e-posta uyarılarını etkinleştirmek için **E-posta yı etkinleştirme** düğmesini etkinleştirme özelliğini açın.

    ![Proje Çiftlik Beats](./media/configure-rules-and-alerts-in-azure-farmbeats/new-rule-email-1.png)

10. **E-posta öznesi** ve **Ek Notlar**ile birlikte e-posta uyarısı göndermek istediğiniz **E-posta adreslerini** girin.  
11. Kural **Durumu'nda,** kuralı etkinleştirmek veya devre dışı etmek için **Etkin** geçiş düğmesini açın.
    Kuraldan etkilenecek aygıt sayısını görüntüleyebilirsiniz.
12. Kuralı oluşturmak için **Uygula'yı** seçin.

## <a name="view-rule"></a>Kuralı görüntüle

**Çiftlik** sayfası kullanılabilir kuralların listesini görüntüler. Kural **Adı**seçin. Bir pencere, seçili kural için geçerli olan aşağıdaki ayrıntıları görüntüler:
 - Kural adı
 - Kuralın ilişkili olduğu çiftliğe bağlantı
 - Oluşturulan tarih
 - Son güncellenen tarih
 - Önem derecesi
 - Kural durumu
 - Koşullar listesi  
 - Kuraldan etkilenen aygıt sayısı

    ![Proje Çiftlik Beats](./media/configure-rules-and-alerts-in-azure-farmbeats/view-rule-1.png)

## <a name="edit-rule"></a>Kuralı düzenleme

Bir kuralı yönetmek için aşağıdaki adımları izleyin:

1. Ana sayfada, soldaki gezinti menüsünden **Kurallar'ı** seçin.
   Kurallar penceresi görüntülenir.
2. Yönetmek istediğiniz kuralı seçin.

    ![Proje Çiftlik Beats](./media/configure-rules-and-alerts-in-azure-farmbeats/edit-rule-action-bar-1.png)

3. Eylem çubuğundan **Edit'i** seçin, **Kuralı Edit** penceresi görüntülenir.

    ![Proje Çiftlik Beats](./media/configure-rules-and-alerts-in-azure-farmbeats/edit-rule-one-1.png)

4. Kural **Adı**ve **Kural Açıklaması'nı** değiştirin ve ardından **Çiftlik** açılır menüsünden bir çiftlik seçin.
5. Çiftliği seçmek için çiftlik adınızı yazın ve **Koşullar** aynı pencerede görünür.  
6. **Koşullarda**, Ölçü'nü, **İşleçve** **Değer'i**edit. **Measure**
7. **Ölçü** açılır menüsüne ölçü adını yazın.
8. Kurallara koşul eklemek/biçimlendirmek için **+Ekle Koşul'u** seçin.

    ![Proje Çiftlik Beats](./media/configure-rules-and-alerts-in-azure-farmbeats/edit-rule-two-1.png)

9.  Önem **Düzeyi'ni**seçin.  
10. **Eylem'de,** e-posta uyarılarını etkinleştirmek için **E-posta yı etkinleştirme** düğmesini etkinleştirme özelliğini açın.
11. E-posta öznesi ve **Ek Notlar**ile birlikte e-posta uyarısı göndermek istediğiniz **E-posta adreslerini** **edin.**  
12. Kural **Durumu'nda,** kuralı etkinleştirmek veya devre dışı etmek için **Etkin** geçiş düğmesini açın.
Bu kuraldan etkilenecek aygıt sayısını görüntüleyebilirsiniz.
13. Kuralı yönetmek için **Uygula'yı** seçin.

## <a name="change-rule-status"></a>Kural durumunu değiştir

Bir kuralın durumunu değiştirmek için aşağıdaki adımları izleyin:

1. Ana sayfada, soldaki gezinti menüsünden **Kurallar'ı** seçin. Kurallar penceresi görüntülenir.
2. Durumu değiştirmek istediğiniz kuralı seçin.

    ![Proje Çiftlik Beats](./media/configure-rules-and-alerts-in-azure-farmbeats/change-status-rule-action-bar-1.png)

3. Eylem çubuğundan **Durumu Değiştir'i** seçin. **Durum Değiştir** penceresi görüntülenir.

    ![Proje Çiftlik Beats](./media/configure-rules-and-alerts-in-azure-farmbeats/rule-change-status-1.png)

3. **Durum** değiştirme düğmesini kullanarak kural durumunu değiştirin.
   Kuraldan etkilenecek aygıt sayısını görüntüleyebilirsiniz.
4. Kural durumunu değiştirmek için **Uygula'yı** seçin.

## <a name="delete-rule"></a>Kuralı silme

Bir kuralı silmek için aşağıdaki adımları izleyin:

1. Ana sayfada, soldaki gezinti menüsünden **Kurallar'ı** seçin. Kurallar penceresi görüntülenir.
2. Silmek istediğiniz kuralı seçin.

    ![Proje Çiftlik Beats](./media/configure-rules-and-alerts-in-azure-farmbeats/delete-rule-action-bar-1.png)

3. Eylem çubuğundan **Sil'i** seçin.

    ![Proje Çiftlik Beats](./media/configure-rules-and-alerts-in-azure-farmbeats/delete-rule-1.png)

4. **Kuralı Sil** iletişim kutusu görüntülenir. **Sil**’i seçin.
