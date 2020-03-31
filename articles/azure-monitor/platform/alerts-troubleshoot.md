---
title: Azure Monitör uyarılarını ve bildirimlerini sorun giderme
description: Azure Monitor uyarıları ve olası çözümlerle ilgili sık karşılaşılan sorunlar.
author: ofirmanor
ms.author: ofmanor
ms.topic: reference
ms.date: 03/16/2020
ms.subservice: alerts
ms.openlocfilehash: beb47f961c6f24453bd49aa5807c9d801fc199a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132337"
---
# <a name="troubleshooting-problems-in-azure-monitor-alerts"></a>Azure Monitor uyarılarındaki sorun giderme sorunları 

Bu makalede, Azure İzleme uyarılarında sık karşılaşılan sorunlar anlatılmaktadır.

Azure Monitor uyarıları, izleme verilerinizde önemli koşullar bulunduğunda sizi proaktif olarak bilgilendirir. Sisteminizin kullanıcıları bunları fark etmeden önce sorunları tanımlamanızı ve çözmenizi sağlar. Uyarı hakkında daha fazla bilgi için Microsoft [Azure'daki uyarılara genel bakış](alerts-overview.md)bölümüne bakın.

## <a name="action-or-notification-on-my-alert-did-not-work-as-expected"></a>Uyarımdaki eylem veya bildirim beklendiği gibi çalışmadı

Azure portalında bir uyarı görebiliyorsanız, ancak bazı eylemleri veya bildirimleri ile ilgili bir sorununuz varsa, aşağıdaki bölümlere bakın.

## <a name="did-not-receive-expected-email"></a>Beklenen e-postayı almadım

Azure portalında bir uyarı görebiliyor, ancak bu konuda yapılandırdığınız e-postayı almadıysanız aşağıdaki adımları izleyin: 

1. **E-posta bir [eylem kuralı](alerts-action-rules.md)tarafından bastırıldı mı?** 

    Portaldaki uyarıyı tıklayarak kontrol edin ve bastırılmış [eylem gruplarıiçin](action-groups.md)geçmiş sekmesine bakın: 

    ![Uyarı eylem kuralı bastırma geçmişi](media/alerts-troubleshoot/history-action-rule.png)

1. **Eylem türü "E-posta Azure Kaynak Yöneticisi Rolü" mi?**

    Bu eylem yalnızca abonelik kapsamında ki Azure Kaynak Yöneticisi rol atamalarına ve *Kullanıcı*türüne bakar.  Rolü kaynak düzeyinde veya kaynak grubu düzeyinde değil, abonelik düzeyinde atadığınızdan emin olun.

1. **E-posta sunucunuz ve posta kutunuz harici e-postaları kabul ediyor mu?**

    Bu üç adresten gelen e-postaların engellenmediğini doğrulayın:
      - azure-noreply@microsoft.com  
      - azureemail-noreply@microsoft.com
      - alerts-noreply@mail.windowsazure.com

    Dahili posta listelerinin veya dağıtım listelerinin harici e-posta adreslerinden gelen e-postaları engellemesi yaygındır. Yukarıdaki e-posta adreslerini beyaz listeye almanız gerekir.  
    Test etmek için, eylem grubuna normal bir iş e-posta adresi (posta listesi değil) ekleyin ve uyarıların bu e-postaya gelip gelmediğini görün. 

1. **E-posta gelen kutusu kuralları veya spam filtresi tarafından işlendi mi?** 

    Bu e-postaları silen veya bir yan klasöre taşıyacak gelen kutusu kuralları olmadığını doğrulayın. Örneğin, gelen kutusu kuralları konuyla ilgili belirli gönderenleri veya belirli sözcükleri yakalayabilir.

    Ayrıca, kontrol edin:
    
      - e-posta istemcinizin spam ayarları (Outlook, Gmail gibi)
      - gönderenin sınırları / spam ayarları / e-posta sunucunuzun karantina ayarları (Exchange, Office 365, G-suite gibi)
      - varsa e-posta güvenlik cihazınızın ayarları (Barracuda, Cisco gibi). 

1. **Eylem grubundan yanlışlıkla aboneliğinizi iptal mi edildiniz?** 

    Uyarı e-postaları, eylem grubundan aboneliği iptal etmek için bir bağlantı sağlar. Bu eylem grubundan yanlışlıkla aboneliğinizi iptal edip etmediğinizi kontrol etmek için aşağıdakilerden biri:

    1. Portaldaki eylem grubunu açın ve Durum sütununa bakın:

    ![eylem grubu durum sütunu](media/alerts-troubleshoot/action-group-status.png)

    2. Abonelikten çıkma onayı için e-postanızda arama yapın:

    ![uyarı eylem grubundan aboneliği ni iptal](media/alerts-troubleshoot/unsubscribe-action-group.png)

    Yeniden abone olmak için – aldığınız abonelikten çıkma onay e-postasındaki bağlantıyı kullanın veya e-posta adresini eylem grubundan kaldırın ve sonra tekrar ekleyin. 
 
1. **Tek bir e-posta adresine giden birçok e-posta nedeniyle sınırlı olarak derecelendirildiniz mi?** 

    E-posta oranı, her e-posta adresine her saat başı en fazla 100 e-posta ile [sınırlıdır.](alerts-rate-limiting.md) Bu eşiği geçerseniz, ek e-posta bildirimleri bırakılır.  E-posta adresinizin geçici olarak sınırlandığını belirten bir ileti olup olmadığını kontrol edin: 
 
   ![E-posta oranı sınırlı](media/alerts-troubleshoot/email-paused.png)

   Yüksek hacimli bildirimleri ücret sınırlaması olmadan almak istiyorsanız, webhook, mantık uygulaması, Azure işlevi veya otomasyon runbook'ları gibi hiçbiri oran sınırlı olmayan farklı bir eylem kullanmayı düşünün. 

## <a name="did-not-receive-expected-sms-voice-call-or-push-notification"></a>Beklenen SMS, sesli arama veya anında iletme bildirimi almadı

Portalda bir uyarı görebiliyorsanız, ancak bu konuda yapılandırdığınız SMS, sesli arama veya anında iletme bildirimini almadıysanız aşağıdaki adımları izleyin: 

1. **Eylem bir [eylem kuralı](alerts-action-rules.md)tarafından bastırıldı mı?** 

    Portaldaki uyarıyı tıklayarak kontrol edin ve bastırılmış [eylem gruplarıiçin](action-groups.md)geçmiş sekmesine bakın: 

    ![Uyarı eylem kuralı bastırma geçmişi](media/alerts-troubleshoot/history-action-rule.png)

   Bu kasıtsız sayılsaydı, eylem kuralını değiştirebilir, devre dışı edebilir veya silebilirsiniz.
 
1. **SMS / ses: Telefon numaranız doğru mu?**

   Ülke koduveya telefon numarasındaki yazım hataları için SMS eylemini kontrol edin. 
 
1. **SMS / ses: Eğer oranı sınırlı oldu mu?** 

    SMS ve sesli aramalar, telefon numarası başına her beş dakikada bir birden fazla bildirimle sınırlıdır. Bu eşiği geçerseniz, bildirimler bırakılır. 

      - Sesli arama – arama geçmişinizi kontrol edin ve önceki beş dakika içinde Azure'dan farklı bir arama nız olup olmadığını görün. 
      - SMS - Telefon numaranızın fiyatının sınırlı olduğunu belirten bir mesaj için SMS geçmişinizi kontrol edin. 

    Yüksek hacimli bildirimleri ücret sınırlaması olmadan almak istiyorsanız, webhook, mantık uygulaması, Azure işlevi veya otomasyon runbook'ları gibi hiçbiri oran sınırlı olmayan farklı bir eylem kullanmayı düşünün. 
 
1. **SMS: Eylem grubundan yanlışlıkla aboneliğinizi iptal mi edildiniz?**

    SMS geçmişinizi açın ve bu belirli eylem grubundan (Action_group_short_name yanıtını kullanarak) veya tüm eylem gruplarından (STOP yanıtını kullanarak) SMS gönderimini devre dışı bırakıp bırakmadığınızı kontrol edin. Yeniden abone olmak için, ilgili SMS komutunu (ENABLE action_group_short_name veya START) gönderin veya SMS eylemini eylem grubundan kaldırın ve sonra tekrar ekleyin.  Daha fazla bilgi [için, eylem gruplarında SMS uyarı davranışına](alerts-sms-behavior.md)bakın.

1. **Telefonunuzdaki bildirimleri yanlışlıkla engellediniz mi?**

   Çoğu cep telefonu, belirli telefon numaralarından veya kısa kodlardan gelen aramaları veya SMS'leri veya belirli uygulamalardan (Azure mobil uygulaması gibi) gelen anında iletme bildirimlerini engellemenize olanak tanır. Telefonunuzdaki bildirimleri yanlışlıkla engelleyip engellemediğinizi kontrol etmek için, telefonunuzun işletim sistemi ve modeline özel belgelerde arama yapın veya farklı bir telefon ve telefon numarasıyla test edin. 

## <a name="expected-another-type-of-action-to-trigger-but-it-did-not"></a>Tetiklemek için başka bir eylem türü bekleniyor, ancak vermedi 

Portalda bir uyarı görebiliyorsanız, ancak yapılandırılan eylemi tetiklenmiyorsa, aşağıdaki adımları izleyin: 

1. **Eylem bir eylem kuralı tarafından bastırıldı mı?** 

    Portaldaki uyarıyı tıklayarak kontrol edin ve bastırılmış [eylem gruplarıiçin](action-groups.md)geçmiş sekmesine bakın: 

    ![Uyarı eylem kuralı bastırma geçmişi](media/alerts-troubleshoot/history-action-rule.png)
 
    Bu kasıtsız sayılsaydı, eylem kuralını değiştirebilir, devre dışı edebilir veya silebilirsiniz. 

1. **Bir webhook tetiklemedi mi?**

    1. **Kaynak IP adresleri engellendi mi?**
    
       Webhook'un çağrıldığı [IP adreslerini](action-groups.md#action-specific-information) beyaz listele.

    1. **Webhook bitiş noktası doğru çalışıyor mu?**

       Yapılandırdığınız webhook bitiş noktasının doğru olduğunu ve bitiş noktasının doğru çalıştığını doğrulayın. Webhook günlüklerinizi veya enstrümanınızın kodunu kontrol edin, böylece araştırabilirsiniz (örneğin, gelen yükü kaydedin). 

    1. **Slack'i mi yoksa Microsoft Teams'i mi arıyorsunuz?**  
    Bu uç noktaların her biri belirli bir JSON biçimi bekler. Bunun yerine bir mantık uygulaması eylemi yapılandırmak için [bu yönergeleri](action-groups-logic-app.md) izleyin.

    1. **Webhook yanıt vermiyor veya geri döndü hataları oldu mu?** 

        Bir webhook yanıt için zaman dilimimiz 10 saniyedir. Webhook araması, aşağıdaki HTTP durum kodları döndürüldüğünde en fazla iki kez daha denenecektir: 408, 429, 503, 504 veya HTTP bitiş noktası yanıt vermiyor. İlk yeniden deneme 10 saniye sonra yapılır. İkinci ve son tekrar deneme 100 saniye sonra gerçekleşir. İkinci yeniden deneme başarısız olursa uç nokta hiçbir eylem grubu için 30 dakika boyunca bir daha çağrılmaz.

## <a name="action-or-notification-happened-more-than-once"></a>Eylem veya bildirim birden fazla kez oldu 

Bir uyarı için birden fazla kez bir uyarı (e-posta veya SMS gibi) bildirimi aldıysanız veya uyarının eylemi (webhook veya Azure işlevi gibi) birden çok kez tetiklendiyse aşağıdaki adımları izleyin: 

1. **Gerçekten aynı uyarı mı?** 

    Bazı durumlarda, aynı anda birden fazla benzer uyarı ateşlenir. Yani, aynı uyarı birçok kez eylemlerini tetikledi gibi görünebilir. Örneğin, bir etkinlik günlüğü uyarı kuralı, olay durumu alanında filtre uygulanmayarak hem bir olay başladığında hem de tamamlandığında (başarılı veya başarısız) ateş lenecek şekilde yapılandırılabilir. 

    Bu eylemlerin veya bildirimlerin farklı uyarılardan gelip gelmediğini kontrol etmek için, zaman damgası ve uyarı kimliği veya korelasyon kimliği gibi uyarı ayrıntılarını inceleyin. Alternatif olarak, portaldaki uyarı listesini kontrol edin. Bu durumda, uyarı kuralı mantığını uyarlamanız veya uyarı kaynağını yapılandırmanız gerekir. 

1. **Eylem birden çok eylem grubunda yineler mi?** 

    Bir uyarı ateşlendiğinde, eylem gruplarının her biri bağımsız olarak işlenir. Bu nedenle, birden çok tetiklenen eylem grubunda bir eylem (e-posta adresi gibi) görünüyorsa, eylem grubu başına bir kez çağrılır. 

    Hangi eylem gruplarının tetikleniyi denetlediğini denetlemek için uyarı geçmişi sekmesini denetleyin. Uyarı kuralında tanımlanan her iki eylem grubu ve eylem kurallarına göre uyarıya eklenen eylem grupları vardır: 

    ![Birden çok eylem grubunda yinelenen eylem](media/alerts-troubleshoot/action-repeated-multi-action-groups.png)

## <a name="action-or-notification-has-an-unexpected-content"></a>Eylem veya bildirim beklenmeyen bir içeriğe sahiptir

Uyarıyı aldıysanız, ancak bazı alanlarının eksik veya yanlış olduğuna inanıyorsanız aşağıdaki adımları izleyin: 

1. **Eylem için doğru biçimi seçtiniz mi?** 

    Her eylem türü (e-posta, webhook, vb) iki biçimi vardır - varsayılan, eski biçimi ve [yeni ortak şema biçimi.](alerts-common-schema.md) Bir eylem grubu oluşturduğunuzda, eylem başına istediğiniz biçimi belirtirsiniz – eylem gruplarındaki farklı eylemlerin farklı biçimleri olabilir. 

    Örneğin, webhook eylemi için: 

    ![webhook eylem şema seçeneği](media/alerts-troubleshoot/webhook.png)

    Eylem düzeyinde belirtilen biçimin beklediğiniz şey olup olmadığını denetleyin. Örneğin, uyarılara (webhook, işlev, mantık uygulaması, vb.) yanıt veren, bir biçim bekleyen, ancak daha sonra eylemde sizin veya başka bir kişinin farklı bir biçim belirlediği bir kod geliştirmiş olabilirsiniz.  

    Ayrıca, günlük [arama uyarıları](alerts-log-webhook.md) (hem Uygulama İstatistikleri ve günlük analitiği), [metrik uyarılar](alerts-metric-near-real-time.md#payload-schema)için , ortak uyarı [şeması](alerts-common-schema-definitions.md)için , ve amortismana klasik [metrik uyarılar](alerts-webhooks.md) [için, aktivite günlüğü uyarıları](activity-log-alerts-webhook.md)için yük biçimini (JSON) kontrol edin.

 
1. **Etkinlik günlüğü uyarıları: Etkinlik günlüğünde bulunan bilgiler var mı?** 

    [Etkinlik günlüğü uyarıları,](activity-log-alerts.md) Azure kaynakları oluşturma, güncelleme veya silme, hizmet durumu ve kaynak durumu etkinlikleri veya Azure Danışmanı ve Azure İlkesi'nden elde edilen bulgular gibi Azure Etkinlik Günlüğü'ne yazılan olaylara dayanan uyarılardır. Etkinlik günlüğüne dayalı bir uyarı aldıysanız, ancak ihtiyacınız olan bazı alanlar eksik veya yanlışsa, önce etkinlik günlüğündeki olayları denetleyin. Azure kaynağı, etkinlik günlüğü olayında aradığınız alanları yazmadıysa, bu alanlar ilgili uyarıya dahil edilmez. 

## <a name="action-rule-is-not-working-as-expected"></a>Eylem kuralı beklendiği gibi çalışmıyor 

Portalda bir uyarı görebiliyorsanız, ancak ilgili bir eylem kuralı beklendiği gibi çalışmadıysa, aşağıdaki adımları izleyin: 

1. **Eylem kuralı etkin mi?** 

    İlgili eylem rolünün etkin olduğunu doğrulamak için eylem kuralı durum sütununa bakın. 

    ![Grafik](media/alerts-troubleshoot/action-rule-status.png) 

    Etkin değilse, eylem kuralını seçip Etkinleştir'i tıklatarak etkinleştirebilirsiniz. 

1. **Bu bir hizmet sağlık uyarısı mı?** 

    Hizmet durumu uyarıları (izleme hizmeti = "Hizmet Durumu") eylem kurallarından etkilenmez. 

1. **Eylem kuralı tetikte mi hareket etti?** 

    Portaldaki uyarıyı tıklayarak eylem kuralının uyarınızı işleyip işlemediğini kontrol edin ve geçmiş sekmesine bakın.

    Aşağıda, tüm eylem gruplarını bastıran eylem kuralıörneği verilmiştir: 
 
     ![Uyarı eylem kuralı bastırma geçmişi](media/alerts-troubleshoot/history-action-rule.png)

    Başka bir eylem grubu ekleyen bir eylem kuralı örneği aşağıda verilmiştir:

    ![Birden çok eylem grubunda yinelenen eylem](media/alerts-troubleshoot/action-repeated-multi-action-groups.png)
 

1. **Eylem kuralı kapsamı ve filtresi ateşlenen uyarıyla eşleşir mi?** 

    Eylem kuralının ateşlenmiş olması gerektiğini, ancak ateş etmemiş olması gerektiğini düşünüyorsanız, ancak ateş etmemiş olması gerektiğini düşünüyorsanız, eylem kuralı kapsamını ve filtre koşullarını, ateşlenen uyarının özelliklerine göre dikkatle inceleyin. 


## <a name="how-to-find-the-alert-id-of-a-fired-alert"></a>Ateşlenen bir uyarının uyarı kimliğini bulma

Belirli bir uyarı yla ilgili bir servis talebi açarken (örneğin, e-posta bildirimini almadıysanız), uyarı kimliğini sağlamanız gerekir. 

Bulmak için aşağıdaki adımları izleyin:

1. Azure portalında, ateşlenen uyarılar listesine gidin ve bu özel uyarıyı bulun. Filtreleri, yerini bulmanıza yardımcı olması için kullanabilirsiniz. 

1. Uyarı ayrıntılarını açmak için uyarıyı tıklatın. 

1. Siz bulana kadar ilk sekmenin (özet sekmesi) uyarı alanlarında aşağı kaydırın ve kopyalayın. Bu alan da kullanabileceğiniz bir "Panoya kopyala" yardımcı düğmesini içerir.  

    ![uyarı kimliğini bul](media/alerts-troubleshoot/get-alert-id.png)

## <a name="problem-creating-updating-or-deleting-action-rules-in-the-azure-portal"></a>Azure portalında eylem kuralları oluşturma, güncelleştirme veya silme sorunu

Bir [eylem kuralı](alerts-action-rules.md)oluşturmaya, güncelleştirmeye veya silmeye çalışırken bir hata aldıysanız, aşağıdaki adımları izleyin: 

1. **İzin hatası aldınız mı?**  

     [İzleme Katılımcısı yerleşik rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor)veya eylem kuralları ve uyarılarıyla ilgili belirli izinlere sahip olmalısınız.

1. **Eylem kuralı parametrelerini doğruladınız mı?**  

    Eylem [kuralı belgelerini](alerts-action-rules.md)veya [PowerShell Set-AzActionRule](https://docs.microsoft.com/powershell/module/az.alertsmanagement/Set-AzActionRule?view=azps-3.5.0) komutunu kontrol edin. 


## <a name="next-steps"></a>Sonraki adımlar
- Günlük uyarısı kullanıyorsanız, [Sorun Giderme Günlüğü Uyarıları'na](alert-log-troubleshoot.md)da bakın.
- Yukarıdaki kılavuzla sorununuzu çözüp çözmediğinizi kontrol etmek için [Azure portalına](https://portal.azure.com) geri dön 
