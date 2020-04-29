---
title: Azure Izleyici uyarıları ve bildirimleri sorunlarını giderme
description: Azure Izleyici uyarıları ve olası çözümlerle ilgili yaygın sorunlar.
author: ofirmanor
ms.author: ofmanor
ms.topic: reference
ms.date: 03/16/2020
ms.subservice: alerts
ms.openlocfilehash: beb47f961c6f24453bd49aa5807c9d801fc199a3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80132337"
---
# <a name="troubleshooting-problems-in-azure-monitor-alerts"></a>Azure Izleyici uyarıları sorunlarını giderme 

Bu makalede, Azure Izleyici uyarı ' nda yaygın sorunlar ele alınmaktadır.

İzleme verilerinizde önemli koşullar bulunduğunda Azure Izleyici uyarıları size önceden bildirimde bulunur. Bunlar, sisteminizin kullanıcıları tarafından bildirilmeksizin sorunları tanımlamanızı ve adreslerinizi belirlemenizi sağlar. Uyarı hakkında daha fazla bilgi için bkz. [Microsoft Azure uyarılara genel bakış](alerts-overview.md).

## <a name="action-or-notification-on-my-alert-did-not-work-as-expected"></a>Uyarımda eylem veya bildirim beklendiği gibi çalışmadı

Azure portal tetiklenen bir uyarı görebilir, ancak bazı eylemler veya bildirimleriyle ilgili bir sorun yaşıyorsanız, aşağıdaki bölümlere bakın.

## <a name="did-not-receive-expected-email"></a>Beklenen e-posta alınamadı

Azure portal tetiklenen bir uyarı görebilir, ancak bununla ilgili olarak yapılandırdığınız e-postayı almadıysanız, şu adımları izleyin: 

1. **E-posta bir [eylem kuralı](alerts-action-rules.md)tarafından gizlendi**mu? 

    Portalda tetiklenen uyarıya tıklayarak ve gizlenen [eylem grupları](action-groups.md)için geçmiş sekmesine göz atın: 

    ![Uyarı eylem kuralı gizleme geçmişi](media/alerts-troubleshoot/history-action-rule.png)

1. **"E-posta Azure Resource Manager rolü" eyleminin türüdür mi?**

    Bu eylem yalnızca abonelik kapsamındaki ve *Kullanıcı*türünde Azure Resource Manager rol atamalarına bakar.  Rolü, kaynak düzeyinde veya kaynak grubu düzeyinde değil, abonelik düzeyinde atamış olduğunuzdan emin olun.

1. **E-posta sunucunuz ve posta kutunuz dış e-postaları kabul ediyor mu**

    Bu üç adresten e-postaların engellenmediğinden emin olun:
      - azure-noreply@microsoft.com  
      - azureemail-noreply@microsoft.com
      - alerts-noreply@mail.windowsazure.com

    İç posta listelerinin veya dağıtım listelerinin, dış e-posta adreslerinden gelen e-postaları engellemesi yaygındır. Yukarıdaki e-posta adreslerini beyaz listeye almanız gerekir.  
    Test etmek için, eylem grubuna düzenli bir iş e-posta adresi (posta listesi değil) ekleyin ve uyarıların bu e-postaya ulaştığını görün. 

1. **E-posta gelen kutusu kuralları veya bir istenmeyen posta Filtresi tarafından işlendi mi?** 

    Bu e-postaları silen veya bir yan klasöre taşıdığınız bir gelen kutusu kuralı olmadığından emin olun. Örneğin, gelen kutusu kuralları söz konusu gönderenleri veya ilgili belirli sözcükleri yakalayabilir.

    Ayrıca, şunu denetleyin:
    
      - e-posta istemciniz (Outlook, Gmail gibi) istenmeyen posta ayarları
      - Gönderici, e-posta sunucunuzun (Exchange, Office 365, G-Suite gibi) sınırlarını/istenmeyen posta ayarlarını/karantina ayarlarını
      - varsa, e-posta güvenlik gerecinizin ayarları (Barrampada gibi). 

1. **Yanlışlıkla eylem grubundan aboneliği kaldırtınız mı?** 

    Uyarı e-postaları, eylem grubundan abonelik kaldırma için bir bağlantı sağlar. Yanlışlıkla bu eylem grubuna abone olunmuş olup olmadığınızı denetlemek için şunlardan birini yapın:

    1. Portalda eylem grubunu açın ve durum sütununu kontrol edin:

    ![eylem grubu durum sütunu](media/alerts-troubleshoot/action-group-status.png)

    2. Abonelik kaldırma onayı için e-postanızı arayın:

    ![Uyarı eylem grubundan abone olunmaz](media/alerts-troubleshoot/unsubscribe-action-group.png)

    Yeniden abone olmak için, aldığınız abonelik kaldırma onayı e-postasında bağlantıyı kullanın veya eylem grubundan e-posta adresini kaldırın ve tekrar ekleyin. 
 
1. **Tek bir e-posta adresine giderken çok sayıda e-posta olduğundan sınırlı derecelendirdiniz mi?** 

    E-posta, her bir e-posta adresine her saat 100 'den fazla e-posta ile [sınırlandırılmıştır](alerts-rate-limiting.md) . Bu eşiği geçirirseniz, ek e-posta bildirimleri bırakılır.  E-posta adresinizin geçici olarak sınırlı olduğunu belirten bir ileti aldığınızı denetleyin: 
 
   ![E-posta hızı sınırlı](media/alerts-troubleshoot/email-paused.png)

   Hız sınırlaması olmadan yüksek hacimli bildirimler almak isterseniz, Web kancası, mantıksal uygulama, Azure işlevi veya Otomasyon Runbook 'ları gibi farklı bir eylem kullanmayı düşünün; hiçbiri hız sınırlı değildir. 

## <a name="did-not-receive-expected-sms-voice-call-or-push-notification"></a>Beklenen SMS, sesli çağrı veya anında iletme bildirimi almadı

Portalda tetiklenen bir uyarı görebilir, ancak bununla ilgili olarak yapılandırdığınız SMS, sesli çağrı veya anında iletme bildirimini almazsanız, şu adımları izleyin: 

1. **Eylem bir [eylem kuralı](alerts-action-rules.md)tarafından gizlendi mu?** 

    Portalda tetiklenen uyarıya tıklayarak ve gizlenen [eylem grupları](action-groups.md)için geçmiş sekmesine göz atın: 

    ![Uyarı eylem kuralı gizleme geçmişi](media/alerts-troubleshoot/history-action-rule.png)

   Bu, istenmeden, eylem kuralını değiştirebilir, devre dışı bırakabilir veya silebilirsiniz.
 
1. **SMS/Voice: telefon numaranız doğru mu?**

   Ülke kodunda veya telefon numarasında yazım hataları için SMS eylemini denetleyin. 
 
1. **SMS/Voice: hız sınırlı mi?** 

    SMS ve seslendirme, her telefon numarası için beş dakikada bir bildirim ile sınırlı orandır. Bu eşiği geçirirseniz, bildirimler bırakılır. 

      - Sesli çağrı – önceki beş dakika içinde, Azure 'dan farklı bir çağrı olup olmadığınızı kontrol edin. 
      - SMS-telefon numaranız oranının sınırlı olduğunu belirten bir ileti için SMS geçmişinizi kontrol edin. 

    Hız sınırlaması olmadan yüksek hacimli bildirimler almak isterseniz, Web kancası, mantıksal uygulama, Azure işlevi veya Otomasyon Runbook 'ları gibi farklı bir eylem kullanmayı düşünün; hiçbiri hız sınırlı değildir. 
 
1. **SMS: yanlışlıkla eylem grubundan aboneliği Kaldırtınız mı?**

    SMS geçmişinizi açın ve bu özel eylem grubundan (devre dışı bırakma action_group_short_name yanıtı kullanılarak) veya tüm eylem gruplarından (durdurma yanıtı kullanılarak) SMS teslimini kabul ettiyseniz emin olun. Yeniden abone olmak için ilgili SMS komutunu gönderin (action_group_short_name veya Başlat 'ı ETKINLEŞTIRIN) ya da eylem grubundan SMS eylemini kaldırın ve tekrar ekleyin.  Daha fazla bilgi için bkz. [eylem gruplarında SMS uyarı davranışı](alerts-sms-behavior.md).

1. **Telefonunuzdaki bildirimleri yanlışlıkla engellemiş musunuz?**

   Çoğu cep telefonu, belirli telefon numaralarının veya kısa kodlardan gelen çağrıları veya SMS 'yi engellemeniz ya da belirli uygulamalardan (Azure mobil uygulaması gibi) anında iletme bildirimlerini engellemeyi sağlar. Telefonunuzdaki bildirimleri yanlışlıkla engellediğinizden emin olmak için, belgeleri telefon işletim sisteminiz ve modeliniz için özel olarak arayın veya farklı bir telefon ve telefon numarasıyla test edin. 

## <a name="expected-another-type-of-action-to-trigger-but-it-did-not"></a>Tetiklenecek başka bir eylem türü bekleniyordu, ancak 

Portalda tetiklenen bir uyarı görürseniz, ancak yapılandırılan eylemi tetikleyemediyseniz, şu adımları izleyin: 

1. **Eylem bir eylem kuralı tarafından gizlendi mu?** 

    Portalda tetiklenen uyarıya tıklayarak ve gizlenen [eylem grupları](action-groups.md)için geçmiş sekmesine göz atın: 

    ![Uyarı eylem kuralı gizleme geçmişi](media/alerts-troubleshoot/history-action-rule.png)
 
    Bu, istenmeden, eylem kuralını değiştirebilir, devre dışı bırakabilir veya silebilirsiniz. 

1. **Web kancası tetiklenemedi mi?**

    1. **Kaynak IP adresleri engellensin mi?**
    
       Web kancasının çağrıldığı [IP adreslerini](action-groups.md#action-specific-information) beyaz listeye ekleyin.

    1. **Web kancası uç noktası düzgün çalışıyor mu?**

       Yapılandırdığınız Web kancası uç noktasının doğru olduğunu ve uç noktanın doğru çalıştığını doğrulayın. Web kancası günlüklerinizi denetleyin veya kodunu araştırın (örneğin, gelen yükü günlüğe kaydedin). 

    1. **Bolluk veya Microsoft ekipleri mı arıyorsunuz?**  
    Bu uç noktaların her biri belirli bir JSON biçimi bekliyor. Bunun yerine bir mantıksal uygulama eylemi yapılandırmak için [Bu yönergeleri](action-groups-logic-app.md) izleyin.

    1. **Web kancası yanıt vermiyor veya hata verdi mı?** 

        Web kancası yanıtı için zaman aşımı süresi 10 saniyedir. Aşağıdaki HTTP durum kodları döndürüldüğünde Web kancası çağrısı iki ek kez yeniden denenir: 408, 429, 503, 504, ya da HTTP uç noktası yanıt vermez. İlk yeniden deneme 10 saniye sonra yapılır. İkinci ve son yeniden deneme 100 saniye sonra gerçekleşir. İkinci yeniden deneme başarısız olursa uç nokta hiçbir eylem grubu için 30 dakika boyunca bir daha çağrılmaz.

## <a name="action-or-notification-happened-more-than-once"></a>Eylem veya bildirim birden çok kez gerçekleşti 

Bir uyarı (e-posta veya SMS gibi) için bir defadan fazla bildirim aldıysanız veya uyarının eylemi (Web kancası veya Azure işlevi gibi) birden çok kez tetikleniyorsa, aşağıdaki adımları izleyin: 

1. **Gerçekten aynı uyarı mı?** 

    Bazı durumlarda, aynı anda birden çok benzer uyarı tetiklenir. Bu nedenle, aynı uyarının eylemlerini birden çok kez tetikledi gibi görünebilir. Örneğin, bir etkinlik günlüğü uyarı kuralı hem bir olay başlatıldığında hem de tamamlandığında (başarılı veya başarısız) olay durumu alanında filtrelemeden başlatılacak şekilde yapılandırılabilir. 

    Bu eylemlerin veya bildirimlerin farklı uyarılardan geldiğini denetlemek için, zaman damgası ve uyarı kimliği ya da onun bağıntı kimliği gibi uyarı ayrıntılarını inceleyin. Alternatif olarak, portalda tetiklenen uyarıların listesini kontrol edin. Bu durumda, uyarı kuralı mantığını uyarlamanız veya başka türlü uyarı kaynağını yapılandırmanız gerekir. 

1. **Eylem birden çok eylem grubunda yineleme mi?** 

    Bir uyarı harekete geçirildiğinde, eylem gruplarının her biri bağımsız olarak işlenir. Bu nedenle, birden çok tetiklenen eylem grubunda bir eylem (örneğin, e-posta adresi) görünürse, her eylem grubu için bir kez çağırılır. 

    Hangi eylem gruplarının tetikleneceğini denetlemek için uyarı geçmişi sekmesini kontrol edin. Uyarı kuralında her iki eylem grubunun de tanımlı olduğunu ve eyleme eylem kurallarına göre eylem gruplarının eklendiğini görürsünüz: 

    ![Birden çok eylem grubunda yinelenen eylem](media/alerts-troubleshoot/action-repeated-multi-action-groups.png)

## <a name="action-or-notification-has-an-unexpected-content"></a>Eylem veya bildirimin beklenmedik bir içeriği vardır

Uyarıyı aldıysanız ancak bazı alanlarının eksik veya yanlış olduğunu düşünüyorsanız, şu adımları izleyin: 

1. **Eylem için doğru biçimi mi seçdunuz?** 

    Her eylem türü (e-posta, Web kancası vb.) iki formata sahiptir: varsayılan, eski biçimi ve [daha yeni ortak şema biçimi](alerts-common-schema.md). Bir eylem grubu oluşturduğunuzda, eylem başına istediğiniz biçimi belirtirsiniz – eylem gruplarındaki farklı eylemler farklı biçimlerde olabilir. 

    Örneğin, Web kancası eylemi için: 

    ![Web kancası eylemi şema seçeneği](media/alerts-troubleshoot/webhook.png)

    Eylem düzeyinde belirtilen biçimin beklediğiniz şey olup olmadığını denetleyin. Örneğin, uyarılara (Web kancası, işlev, mantıksal uygulama, vb.) yanıt veren kodu geliştirmiş olabilirsiniz, tek bir biçim bekleniyor, ancak daha sonra sizin veya başka bir kişi farklı bir biçim belirtti.  

    Ayrıca, [etkinlik günlüğü uyarıları](activity-log-alerts-webhook.md)için yük BIÇIMINI (JSON), [günlük araması uyarılarını](alerts-log-webhook.md) (hem Application Insights hem de Log Analytics), [ölçüm uyarıları](alerts-metric-near-real-time.md#payload-schema)için [ortak uyarı şeması](alerts-common-schema-definitions.md)için ve kullanım dışı bırakılmış olan [Klasik ölçüm uyarıları](alerts-webhooks.md)için denetleyin.

 
1. **Etkinlik günlüğü uyarıları: etkinlik günlüğünde bilgiler var mı?** 

    [Etkinlik günlüğü uyarıları](activity-log-alerts.md) , Azure etkinlik günlüğüne yazılan olayları temel alan uyarılardır; Azure kaynaklarını oluşturma, güncelleştirme veya silme, hizmet durumu ve kaynak durumu olayları, Azure Advisor ve Azure ilkesinden bulgular gibi olaylar. Etkinlik günlüğüne dayalı bir uyarı aldıysanız ancak ihtiyacınız olan bazı alanlar eksik ya da yanlış ise, önce etkinlik günlüğündeki olayları kontrol edin. Azure kaynağı, etkinlik günlüğü olayında Aradığınız alanları yazmadığından, bu alanlar ilgili uyarıya dahil edilmez. 

## <a name="action-rule-is-not-working-as-expected"></a>Eylem kuralı beklendiği gibi çalışmıyor 

Portalda tetiklenen bir uyarı görürseniz, ancak ilgili bir eylem kuralı beklendiği gibi çalışmadı, şu adımları izleyin: 

1. **Eylem kuralı etkin mi?** 

    İlgili eylem rolünün etkinleştirildiğini doğrulamak için eylem kuralı durumu sütununu kontrol edin. 

    ![sel](media/alerts-troubleshoot/action-rule-status.png) 

    Etkin değilse, seçerek ve Etkinleştir ' i tıklatarak eylem kuralını etkinleştirebilirsiniz. 

1. **Hizmet durumu uyarısı mı?** 

    Hizmet durumu uyarıları (Monitor Service = "hizmet durumu") eylem kurallarından etkilenmez. 

1. **Eylem kuralı Uyarınıza göre mi çalışıyor?** 

    Portalda tetiklenen uyarıya tıklayarak eylem kuralının uyarısında işlenip işlenmeyeceğini denetleyin ve geçmiş sekmesine bakın.

    Tüm eylem gruplarını gizleme eylem kuralına bir örnek aşağıda verilmiştir: 
 
     ![Uyarı eylem kuralı gizleme geçmişi](media/alerts-troubleshoot/history-action-rule.png)

    Başka bir eylem grubu ekleyen bir eylem kuralına örnek aşağıda verilmiştir:

    ![Birden çok eylem grubunda yinelenen eylem](media/alerts-troubleshoot/action-repeated-multi-action-groups.png)
 

1. **Eylem kuralı kapsamı ve filtresi tetiklenen uyarıyla eşleşiyor mu?** 

    Eylem kuralının tetikleneceğini, ancak olmadığını veya tetiklenmemesi gerektiğini, ancak tetiklenmemesi gerektiğini, ancak harekete geçirilen uyarının özelliklerine ve Filtre koşullarını dikkatle inceleyin. 


## <a name="how-to-find-the-alert-id-of-a-fired-alert"></a>Tetiklenen bir uyarının uyarı kimliğini bulma

Belirli bir tetiklenen uyarı (örneğin, e-posta bildirimini almadıysanız) ile ilgili bir durum açılırken, uyarı kimliğini sağlamanız gerekir. 

Bulmak için şu adımları izleyin:

1. Azure portal, tetiklenen uyarılar listesine gidin ve söz konusu uyarıyı bulun. Bu filtreleri bulmanıza yardımcı olması için kullanabilirsiniz. 

1. Uyarı ayrıntılarını açmak için uyarıya tıklayın. 

1. İlk sekmenin uyarı alanlarını (Özet sekmesi) bulana kadar aşağı kaydırın ve kopyalayın. Bu alan Ayrıca, kullanabileceğiniz bir "panoya kopyala" yardımcı düğmesine de sahiptir.  

    ![uyarı kimliğini bul](media/alerts-troubleshoot/get-alert-id.png)

## <a name="problem-creating-updating-or-deleting-action-rules-in-the-azure-portal"></a>Azure portal eylem kuralları oluşturma, güncelleştirme veya silme sorunu

Bir [eylem kuralı](alerts-action-rules.md)oluşturmaya, güncelleştirmeye veya silmeye çalışırken bir hata alırsanız, şu adımları izleyin: 

1. **Bir izin hatası mı alıyorsunuz?**  

     [Izleme katılımcısı yerleşik rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor)ya da eylem kurallarıyla ve uyarılarla ilgili belirli izinlere sahip olmanız gerekir.

1. **Eylem kuralı parametrelerini doğrulamadınız mı?**  

    [Eylem kuralı belgelerini](alerts-action-rules.md)veya [PowerShell set-azactionrule komutunu eylem kuralını](https://docs.microsoft.com/powershell/module/az.alertsmanagement/Set-AzActionRule?view=azps-3.5.0) denetleyin. 


## <a name="next-steps"></a>Sonraki adımlar
- Günlük uyarısı kullanılıyorsa, bkz. [sorun giderme günlüğü uyarıları](alert-log-troubleshoot.md).
- Yukarıdaki kılavuzdan sorununuzu çözmişseniz olup olmadığınızı denetlemek için [Azure Portal](https://portal.azure.com) geri dönün 
