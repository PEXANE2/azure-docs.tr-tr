---
title: Azure Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimleri
description: Bu belge, Azure Güvenlik Merkezi'nde azure makinelerinde çalışan uygulamaları beyaz listeye almak için uyarlamalı uygulama denetimini kullanmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/23/2019
ms.author: memildin
ms.openlocfilehash: 1dc94c5ec08cc27fb1819ccc16fd766c62aad796
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604681"
---
# <a name="adaptive-application-controls"></a>Uyarlamalı uygulama denetimleri
Bu kılavuzu kullanarak Azure Güvenlik Merkezi'ndeki uygulama denetimi özelliklerini yapılandırmayı öğrenebilirsiniz.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimleri nelerdir?
Uyarlanabilir uygulama denetimi, Azure Güvenlik Merkezi'nin azure ve Azure olmayan makinelerinizde (Windows ve Linux) hangi uygulamaların çalıştırılamayabileceğini kontrol etmenize yardımcı olan akıllı, otomatik, uçtan uca bir çözümdür. Diğer faydaların yanı sıra, bu kötü amaçlı yazılımlara karşı makineleri sertleştirmek yardımcı olur. Güvenlik Merkezi, makinelerinizde çalışan uygulamaları analiz etmek için makine öğrenimini kullanır ve bu istihbarattan izin listesi oluşturur. Bu özellik, uygulama izin listesi ilkelerini yapılandırma ve koruma işlemini büyük ölçüde basitleştirerek şunları yapmanızı sağlar:

- Kötü amaçlı yazılımdan koruma çözümleri tarafından kaçırılabilecek uygulamalar da dahil olmak üzere kötü amaçlı uygulamaları çalıştırma girişimlerini engelleyin veya uyarı verin.
- Kuruluşunuzun yalnızca lisanslı yazılım kullanımını gerektiren kuruluş güvenlik ilkelerine uygun hareket etme.
- Ortamınızda istenmeyen yazılımların kullanılmasını önleme.
- Eski ve desteklenmeyen uygulamaların çalışmasını önleme.
- Kuruluşunuzda kullanılmasına izin verilmeyen belirli yazılım araçlarını engelleme.
- BT ekibinin uygulama üzerinden gizli verilere erişimi denetlemesini mümkün kılma.

> [!NOTE]
> Azure olmayan ve Linux makineleri için uyarlanabilir uygulama denetimleri yalnızca denetim modunda desteklenir.

## <a name="how-to-enable-adaptive-application-controls"></a>Uyarlamalı uygulama denetimleri nasıl etkinleştirilir?

Uyarlanabilir uygulama denetimleri, yapılandırılmış makine gruplarında çalışmasına izin verilen bir uygulama kümesini tanımlamanıza yardımcı olur. Bu özellik hem Azure hem de Azure olmayan Windows (tüm sürümler, klasik veya Azure Kaynak Yöneticisi) ve Linux makineleri için kullanılabilir. Uygulama izin listelerinizi yapılandırmak için aşağıdaki adımları kullanın:

1. **Güvenlik Merkezi** panosunu açın.

1. Sol bölmeden **Gelişmiş bulut savunması** altında bulunan **Uyarlamalı uygulama denetimlerini** seçin.

    [![Savunma](./media/security-center-adaptive-application/security-center-adaptive-application-fig1-new.png)](./media/security-center-adaptive-application/security-center-adaptive-application-fig1-new.png#lightbox)

**Uyarlamalı uygulama denetimleri** sayfası açılır.

![denetimler](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

**VM grupları** bölümünde üç sekme bulunur:

* **Yapılandırılan**: Uygulama denetimiyle yapılandırılan VM’leri içeren grupların listesidir.
* **Önerilen**: Uygulama denetiminin önerildiği grupların listesidir. Güvenlik Merkezi makine öğrenimi özelliklerini kullanarak VM'lerin tutarlı bir şekilde aynı uygulamaları çalıştırıp çalıştırmadığına bakar ve uygulama denetimi için uygun olan VM'leri tanımlar.
* **Öneri olmayan**: Uygulama denetimi önerisi olmayan VM’leri içeren grupların listesidir. Örneğin, uygulamaların sürekli değiştiği ve kararlı bir duruma geçmediği VM'ler.

> [!NOTE]
> Güvenlik Merkezi benzer VM’lerin önerilen en iyi uygulama denetimi ilkesini alması için VM grupları oluşturan özel bir kümeleme algoritması kullanır.
>
>

### <a name="configure-a-new-application-control-policy"></a>Yeni bir uygulama denetim ilkesi yapılandırma

1. Uygulama denetimi önerileri içeren grupların listesi için **Önerilen** sekmeyi seçin:

   ![Önerilen](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

   Liste aşağıdakileri içerir:

   - **Grup Adı**: Aboneliğin ve grubun adı
   - **VM'ler ve Bilgisayarlar**: Gruptaki sanal makine sayısı
   - **Durum**: önerilerin durumu
   - **Önem derecesi**: önerilerin önem düzeyi

2. **Uygulama denetim kuralları oluştur** seçeneğini açmak için bir grubu tıklatın.

   [![Uygulama denetimi kuralları](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png#lightbox)

3. Select **VM'lerde,** önerilen VM'lerin listesini gözden geçirin ve bir uygulama beyaz liste ilkesi uygulamak istemediğiniz tüm ünün onayını kaldırın. Daha sonra iki liste görürsünüz:

   - **Önerilen uygulamalar:** Bu gruptaki VM'lerde sık görülen ve çalışmasına izin verilmesi önerilen uygulamaların listesi.
   - **Daha fazla uygulama:** Bu gruptaki VM'lerde daha az sıklıkta olan veya Exploitables (daha fazla bkz. daha fazla bilgi) olarak bilinen ve gözden geçirilmesi önerilen uygulamaların listesi.

4. Her bir listedeki uygulamaları gözden geçirin ve uygulamak istemediklerinizin işaretini kaldırın. Her liste aşağıdakileri içerir:

   - **ADI**: sertifika bilgileri veya bir uygulamanın tam yolu
   - **DOSYA TÜRLERİ**: Uygulama dosya türü. Bu EXE, Script, MSI veya bu tür herhangi bir permütasyon olabilir.
   - **EXPLOITABLE**: bir uyarı simgesi, belirli bir uygulamanın bir uygulama izin listesini atlamak için saldırgan tarafından kullanılabileceğini gösterir. Bu uygulamaları onaylamadan önce gözden geçirmeniz önerilir.
   - **KULLANICILAR**: Bir uygulama çalıştırmasına izin verilmesi önerilen kullanıcılar

5. Seçimlerinizi tamamladıktan sonra **Oluştur**’u seçin. <br>
   Azure Güvenlik Merkezi, Oluştur'u seçtikten sonra, Windows sunucularında (AppLocker) kullanılabilen yerleşik uygulama izin listesi çözümünün üstünde ki uygun kuralları otomatik olarak oluşturur.

> [!NOTE]
> - Güvenlik Merkezi, temel yapılandırma oluşturmak ve VM gruplarına benzersiz öneri sunmak için en az iki haftalık veri kullanmaktadır. Güvenlik Merkezi standart katmanının yeni müşterileri başlangıçta VM gruplarının *öneri yok* sekmesi altında olduğunu görebilir.
> - Güvenlik Merkezi'ndeki Uyarlamalı Uygulama Denetimleri, GPO veya yerel güvenlik ilkesi ile AppLocker ilkesinin önceden etkinleştirilmiş olduğu VM'leri desteklemez.
> -  Güvenlik en iyi uygulaması olarak, Güvenlik Merkezi her zaman izin verilen uygulamalar için bir yayımcı kuralı oluşturmaya çalışır ve yalnızca bir uygulamanın yayımcı bilgileri yoksa (aka imzalanmaz), tam yol için bir yol kuralı oluşturulur özel bir uygulama.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Uygulama denetimiyle yapılandırılmış bir grubu düzenleme ve izleme

1. Bir uygulama izin listesi ilkesi ile yapılandırılan bir grubu döndürmek ve izlemek için **Uyarlanabilir uygulama denetimleri** sayfasına dönün ve **VM Grupları**altında **KONFIGÜRASYON'u** seçin:

   ![Gruplar](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

   Liste aşağıdakileri içerir:

   - **Grup Adı**: aboneliğin ve grubun adı
   - **VM'ler ve Bilgisayarlar**: gruptaki sanal makine sayısı
   - **Mod**: Denetim modu, izin listesinde olmayan uygulamaları çalıştırma girişimlerini günlüğe kaydeder; İzin listesinde olmadıkça uygulamaların çalışmasına izin vermez
   - **Uyarılar**: herhangi bir güncel ihlalleri

2. **Uygulama denetimi ilkesini edin** sayfasında değişiklik yapmak için bir grubu tıklatın.

   ![Koruma](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

3. **Koruma modu** altında aşağıdakilerden birini seçebilirsiniz:

   - **Denetim**: Bu modda uygulama denetimi çözümü kuralları zorunlu kılmaz ve yalnızca korumalı VM'lerdeki etkinliği denetler. Bu, bir uygulamanın hedef VM'de çalışmasını engellemeden önce genel davranışını gözlemlemek istediğiniz senaryolar için önerilir.
   - **Zorunlu kıl**: Bu modda uygulama denetimi çözümü kuralları zorunlu kılar ve çalışmasına izin verilmeyen uygulamaların engellenmesini sağlar.

   > [!NOTE]
   > -  **Koruma modunu uygulayın** bir sonraki duyuruya kadar devre dışı bırakılır.
   > - Yukarıda belirtildiği gibi yeni uygulama denetimi ilkeleri her zaman *Denetim* modunda yapılandırılır. 
   >

4. **İlke uzantısı**altında, izin vermek istediğiniz herhangi bir uygulama yolu ekleyin. Bu yolları ekledikten sonra, Güvenlik Merkezi, seçilen VMS grubu içindeki VM'lerde liste ilkesini güncelleştirir ve zaten yürürlükte olan kurallara ek olarak bu uygulamalar için uygun kuralları oluşturur.

5. **Son uyarılar** bölümünde listelenen geçerli ihlalleri gözden geçirin. Azure Güvenlik Merkezi'ndeki **Uyarılar** sayfasına yönlendirilecek her satırı tıklatın ve ilişkili VM'lerde Azure Güvenlik Merkezi tarafından algılanan tüm uyarıları görüntüleyin.
   - **Uyarılar**: günlüğe kaydedilmiş tüm ihlaller.
   - **Hayır, hayır. VM'lerin**: Bu uyarı türüne sahip sanal makine sayısı.

6. **Publisher beyaz liste kuralları,** Yol beyaz liste **kuralları**ve Karma beyaz **liste kuralları** altında, kural toplama türüne göre, hangi uygulama beyaz liste kurallarının şu anda bir grup içindeki VM'lerde yapılandırıldığınızı görebilirsiniz. Gördüğünüz her kural için:

   - **Kural**: Bir uygulamanın çalışmasına izin verilip verilmediğini belirlemek için AppLocker tarafından bir uygulamanın incelendiği belirli parametrelerdir.
   - **Dosya türü**: Belirli bir kural tarafından kapsanan dosya türleri. Bu aşağıdakilerden biri olabilir: EXE, Script, MSI veya bu dosya türlerinin herhangi bir permütasyon.
   - **Kullanıcılar**: Bir uygulama beyaz liste kuralı kapsamında olan bir uygulamayı çalıştırmasına izin verilen kullanıcı adı veya sayısı.

   ![Beyaz listeye ekleme kuralları](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

7. Belirli kuralı silmek veya izin verilen kullanıcıları yönetmek istiyorsanız, her satırın sonundaki üç noktayı tıklatın.

8. **Uyarlanabilir uygulama denetimleri** ilkesinde değişiklik yaptıktan sonra **Kaydet'i**tıklatın.

### <a name="not-recommended-list"></a>Önerilmeyenler listesi

Güvenlik Merkezi yalnızca kararlı bir uygulama kümesi çalıştıran sanal makineler için uygulama beyaz liste ilkeleri önerir. Bağlantılı VM'lerdeki uygulamalar sürekli değişiyorsa öneriler oluşturulmayacaktır.

![Öneri](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

Liste aşağıdakileri içerir:
- **Grup Adı**: aboneliğin ve grubun adı
- **VM'ler ve Bilgisayarlar**: gruptaki sanal makine sayısı

Azure Güvenlik Merkezi, tavsiye edilmeyen VM grupları üzerinde de bir uygulama beyaz liste ilkesi tanımlamanıza olanak tanır. Bu gruplar üzerinde de bir uygulama beyaz liste ilkesi yapılandırmak için, daha önce açıklandığı gibi aynı ilkeleri izleyin.

## <a name="move-a-vm-from-one-group-to-another"></a>VM'yi bir gruptan diğerine taşıma

 Bir VM'yi bir gruptan diğerine taşıdığınızda, ona uygulanan uygulama denetim ilkesi, onu taşıdığınız grubun ayarlarında değişir. VM'yi yapılandırılmış bir gruptan yapılandırılmamış bir gruba taşıyabilirsiniz ve bu da vm'ye daha önce uygulanan uygulama denetim ilkesini kaldırmayla sonuçlanır.

 1. **Uyarlamalı uygulama denetimleri** sayfasından, **YAPılandırılan** sekmesinden, VM'nin şu anda ait olduğu grubu tıklatın.
1. **Yapılandırılmış VM'leri ve Bilgisayarları**tıklatın.
1. Taşımak için VM satırındaki üç noktayı tıklatın ve **Taşı'yı**tıklatın. **Bilgisayarı farklı grup penceresine taşı** açılır.

    ![Koruma](./media/security-center-adaptive-application/adaptive-application-move-group.png)

 1. VM'yi taşımak için grubu seçin ve **Bilgisayarı Taşı'yı**tıklatın ve **Kaydet'i**tıklatın.

    ![Koruma](./media/security-center-adaptive-application/adaptive-application-move-group2.png)

 > [!NOTE]
> **Bilgisayarı Taşı'yı**tıklattıktan sonra **Kaydet'i** tıklattığınızdan emin olun. **Kaydet'i**tıklatmazsanız, bilgisayar taşınmayacak.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Güvenlik Merkezi'nde azure ve Azure olmayan VM'lerde çalışan uygulamaları beyaz listeye almak için uyarlamalı uygulama denetiminin nasıl kullanılacağını öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtverme.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) Güvenlik Merkezi’nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu.](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide) Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin.
* [Azure Güvenlik Blog.](https://blogs.msdn.com/b/azuresecurity/) Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
