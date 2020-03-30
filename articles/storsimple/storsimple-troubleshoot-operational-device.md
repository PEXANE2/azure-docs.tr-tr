---
title: Dağıtılan StorSimple aygıtının sorun giderme sorunu | Microsoft Dokümanlar
description: Şu anda dağıtılan ve çalışır durumda olan bir StorSimple aygıtında oluşan hataların nasıl tanılanıp düzeltileni açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: carmonm
editor: ''
ms.assetid: ea5d89ae-e379-423f-b68b-53785941d9d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: twooley
ms.openlocfilehash: ca79e4240c1a82e46bea44a9d018a3c681920480
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75933287"
---
# <a name="troubleshoot-an-operational-storsimple-device"></a>Operasyonel bir StorSimple aygıtının sorun giderme
> [!NOTE]
> StorSimple için klasik portal kullanım dışıdır. StorSimple Cihaz Yöneticileriniz, yeni Azure portalına kullanımdan kaldırma zamanlamasına göre otomatik olarak taşınacaktır. Bu taşımayla ilgili bir e-posta ve portal bildirimi alacaksınız. Bu belge de yakında kullanımdan kaldırılacaktır. Taşıma hakkında tüm sorularınız için bkz. [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
Bu makalede, StorSimple aygıtınız dağıtıldıktan ve kullanıma çıktıktan sonra karşılaşabileceğiniz yapılandırma sorunlarını çözmek için yararlı sorun giderme kılavuzu sağlar. Microsoft Azure StorSimple'ı çalıştırDığınızda karşılaşabileceğiniz sorunları çözmenize yardımcı olacak sık karşılaşılan sorunları, olası nedenleri ve önerilen adımları açıklar. Bu bilgiler hem StorSimple şirket içi fiziksel aygıt hem de StorSimple sanal aygıtı için geçerlidir.

Bu makalenin sonunda, Microsoft Azure StorSimple işlemi sırasında karşılaşabileceğiniz hata kodlarının bir listesini ve hataları gidermek için atabileceğiniz adımları bulabilirsiniz. 

## <a name="setup-wizard-process-for-operational-devices"></a>Operasyonel aygıtlar için kurulum sihirbazı işlemi
Aygıt yapılandırmasını denetlemek ve gerekirse düzeltici eylemde bulunmanız için kurulum sihirbazını[(Invoke-HcsSetupWizard)][1]kullanırsınız.

Kurulum sihirbazını önceden yapılandırılmış ve operasyonel bir aygıtta çalıştırdığınızda, işlem akışı farklıdır. Yalnızca aşağıdaki girişleri değiştirebilirsiniz:

* IP adresi, alt ağ maskesi ve ağ geçidi
* Birincil DNS sunucusu
* Birincil NTP sunucusu
* İsteğe bağlı web proxy yapılandırması

Kurulum sihirbazı parola toplama ve aygıt kaydıyla ilgili işlemleri gerçekleştirmez.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Kurulum sihirbazının sonraki çalıştırmaları sırasında oluşan hatalar
Aşağıdaki tabloda, bir operasyonel aygıtta kurulum sihirbazını çalıştırdığınızda karşılaşabileceğiniz hatalar, hataların olası nedenleri ve bunları gidermek için önerilen eylemler açıklanmaktadır. 

| Hayır. | Hata iletisi veya koşul | Olası nedenler | Önerilen eylem |
|:--- |:--- |:--- |:--- |
| 1 |Hata 350032: Bu aygıt zaten devre dışı bırakıldı. |Devre dışı bırakılan bir aygıtta kurulum sihirbazını çalıştırdığınızda bu hatayı görürsünüz. |Sonraki adımlar için [Microsoft Destek'e başvurun.](storsimple-contact-microsoft-support.md) Devre dışı bırakılmış bir aygıt hizmete konulamıyor. Aygıtın yeniden etkinleştirilen önce bir fabrika sıfırlaması gerekebilir. |
| 2 |Invoke-HcsSetupWizard : ERROR_INVALID_FUNCTION(HRESULT'dan İstisna: 0x80070001) |DNS sunucu güncelleştirmesi başarısız oluyor. DNS ayarları genel ayarlardır ve etkin ağ arabirimleriboyunca uygulanır. |Arabirimi etkinleştirin ve DNS ayarlarını yeniden uygulayın. Bu ayarlar genel olduğundan, diğer etkin arabirimler için ağ bozabilir. |
| 3 |Aygıt StorSimple Manager hizmet portalında çevrimiçi gibi görünür, ancak minimum kurulumu tamamlamaya ve yapılandırmayı kaydetmeye çalıştığınızda işlem başarısız olur. |İlk kurulum sırasında, web proxy yerinde gerçek bir proxy sunucusu olmasına rağmen, yapılandırılmamada. |Hatayı bulmak için [Test-HcsmConnection cmdlet'i][2] kullanın. Sorunu düzeltemiyorsanız [Microsoft Destek'e başvurun.](storsimple-contact-microsoft-support.md) |
| 4 |Invoke-HcsSetupWizard: Değer beklenen aralıkta düşmez. |Yanlış bir alt ağ maskesi bu hatayı üretir. Olası nedenler şunlardır: <ul><li> Alt ağ maskesi eksik veya boş.</li><li>Ipv6 öneki biçimi yanlış.</li><li>Arabirim bulut etkin, ancak ağ geçidi eksik veya yanlış.</li></ul>Kurulum sihirbazı aracılığıyla yapılandırılırsa DATA 0'ın otomatik olarak bulut etkin olduğunu unutmayın. |Sorunu belirlemek için 0.0.0.0 veya 256.256.256.256 alt ağı kullanın ve çıktıya bakın. Gerektiğinde alt ağ maskesi, ağ geçidi ve Ipv6 öneki için doğru değerleri girin. |

## <a name="error-codes"></a>Hata kodları
Hatalar sayısal sırada listelenir.

| Hata Numarası | Hata metni veya açıklaması | Önerilen kullanıcı eylemi |
|:--- |:--- |:--- |
| 10502 |Depolama hesabınıza erişirken bir hatayla karşılaşıldı. |Birkaç dakika bekleyin ve sonra tekrar deneyin. Hata devam ederse, sonraki adımlar için lütfen Microsoft Destek'e başvurun. |
| 40017 |Yedekleme ilkesinde belirtilen bir birim aygıtta bulunamadıkça yedekleme işlemi başarısız oldu. |Hata devam ederse yedekleme işlemini yeniden deneyin, Microsoft Destek'e başvurun. sonraki adımlar için. |
| 40018 |Yedekleme ilkesinde belirtilen birimlerin hiçbiri aygıtta bulunmadığından yedekleme işlemi başarısız oldu. |Hata devam ederse yedekleme işlemini yeniden deneyin, Microsoft Destek'e başvurun. sonraki adımlar için. |
| 390061 |Sistem meşgul veya kullanılamıyor. |Birkaç dakika bekleyin ve sonra tekrar deneyin. Hata devam ederse, sonraki adımlar için lütfen Microsoft Destek'e başvurun. |
| 390143 |390143 hata kodunda bir hata oluştu. (Bilinmeyen hata.) |Hata devam ederse, lütfen sonraki adımlar için Microsoft Destek'e başvurun. |

## <a name="next-steps"></a>Sonraki adımlar
Sorunu çözemiyorsanız, yardım için [Microsoft Destek'e başvurun.](storsimple-contact-microsoft-support.md) 

[1]: https://technet.microsoft.com/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/%5Clibrary/Dn715782(v=WPS.630).aspx
