---
title: Azure için Avere vFXT desteği nasıl alabilirsiniz?
description: Azure için Avere vFXT hakkında destek biletleri açma açıklaması
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: b3c604003f78a150067875fb2064ab5eca0600db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252564"
---
# <a name="get-help-with-your-system"></a>Sisteminizle ilgili yardım alın

Azure için Avere vFXT sisteminizle ilgili yardım için destek almanın yolları şunlardır:

* **Avere vFXT sorunu** - [Aşağıda](#open-a-support-ticket-for-your-avere-vfxt)açıklandığı gibi Avere vFXT'niz için bir destek bileti açmak için Azure portalını kullanın.
* **Kota** - Kota ile ilgili bir sorununz varsa, [kota artışı isteyin](#request-a-quota-increase)
* **Dokümantasyon ve örnekler** - Bu belge veya örneklerle ilgili sorunlar la ilgili sorunlar la ilgili olarak, sorunla birlikte sayfanın altına gidin ve varolan sorunları aramak ve gerekirse yeni bir dosya açmak için **Geri Bildirim** bölümünü kullanın.

## <a name="open-a-support-ticket-for-your-avere-vfxt"></a>Avere vFXT'niz için bir destek bileti açın

Avere vFXT'yi dağıtırken veya kullanırken sorunlarla karşılaşırsanız, Azure portalı üzerinden yardım isteyin.

Destek biletinizin kümenizden bir kaynakla etiketlendirilediğinden emin olmak için aşağıdaki adımları izleyin. Bileti etiketlemek, bileti doğru destek kaynağına yönlendirmemize yardımcı olur.

1. Kaynak [https://portal.azure.com](https://portal.azure.com) **Grupları'nı**seçin. Sorunun oluştuğu vFXT kümesini içeren kaynak grubuna göz atın ve Avere kümesi sanal makinelerinden birine tıklayın.

    ![belirli bir VM daire içine alınmış Azure portal kaynak grubu "genel bakış" panelinin ekran görüntüsü](media/avere-vfxt-ticket-vm.png)

1. VM sayfasında, sol panelin altına doğru ilerleyin ve **Yeni destek isteğini**tıklatın.

    ![Önceki ekran görüntüsünden VM için Azure portalı VM sayfasının ekran görüntüsü. Sol menü alta kaydırılır ve 'Yeni destek isteği' daire içine çevrilir.](media/avere-vfxt-ticket-request.png)

1. Destek isteğinin ilk sayfasında, sorun türünü seçin ve doğru aboneliğin seçildiğinden emin olun.

   **Hizmet**altında, **Tüm Hizmetler** tıklayın ve **Avere vFXT**seçmek için **Depolama** altında bakmak.

   Kısa bir özet ekleyin ve sorun türünü seçin.

    ![Azure portalındaki yeni bir destek isteği ekranının ekran görüntüsü. Temel bilgiler sekmesi seçilir. Ekran öğeleri Sorun türü, Abonelik, Hizmet, Özet ve Sorun türünü içerir.](media/ticket-basics.png)

   Devam etmek için **İleri** 'ye tıklayın.

1. Destek formunun ikinci sayfasında, özet açıklamanıza dayalı olarak sorunu gidermek için öneriler bulunur. Destek bileti oluşturmanız gerekiyorsa, alttaki **İleri** düğmesini tıklatın.

   ![seçilen Çözümler sekmesi ile yeni destek isteği ekranının ekran görüntüsü. Ortadaki metin alanı 'Önerilen çözüm' başlığına sahiptir ve olası çözüm lerini açıklar.](media/ticket-solutions.png)

1. Üçüncü sayfada, ayrıntıları sağlayın - bu, kümeniz, sorunun oluştuğu saat, önem ve sizinle nasıl iletişim kurabileceğiniz hakkında bilgiler içerir. Bilgileri doldurun ve alttaki **İleri** düğmesini tıklayın.

   ![ayrıntılar sekmesi seçili yeni destek isteği ekranının ekran görüntüsü. Bilgi alanları 'Sorun ayrıntıları', 'Destek yöntemi' ve 'İletişim bilgileri' kategorilerinde düzenlenir.](media/ticket-details.png)

1. Son sayfadaki bilgileri gözden geçirin ve **Oluştur'a**tıklayın. E-posta adresinize bir onay ve bilet numarası gönderilecektir ve bir destek personeli sizinle irtibata geçecektir.

## <a name="request-a-quota-increase"></a>Kota artışı isteğinde bulunun

Azure için Avere vFXT dağıtmak için hangi bileşenlerin gerekli olduğunu öğrenmek [için vFXT kümesi](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) için Kota'yı okuyun. Azure portalından [kota artışı talep](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) edebilirsiniz.
