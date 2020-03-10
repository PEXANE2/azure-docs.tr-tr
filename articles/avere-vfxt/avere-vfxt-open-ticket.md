---
title: Azure için avere vFXT desteği alma
description: Azure için avere vFXT hakkında Destek biletleri açma açıklaması
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: b3c604003f78a150067875fb2064ab5eca0600db
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78372122"
---
# <a name="get-help-with-your-system"></a>Sisteminizle ilgili yardım alın

Azure sistem için avere vFXT 'niz hakkında yardım almak için şu adımları uygulayın:

* **Avere vfxt sorunu** - [aşağıda](#open-a-support-ticket-for-your-avere-vfxt)açıklanan şekilde avere vfxt 'niz için bir destek bileti açmak üzere Azure Portal kullanın.
* **Kota** -kotayla ilgili bir sorununuz varsa, [Kota artışı isteyin](#request-a-quota-increase)
* **Belgeler ve örnekler** -bu belge veya örneklerle ilgili sorunları buldıysanız, sorun ile sayfanın en altına gidin ve var olan sorunları aramak ve gerekirse yeni bir dosya eklemek Için **geri bildirim** bölümünü kullanın.

## <a name="open-a-support-ticket-for-your-avere-vfxt"></a>Avere vFXT 'niz için bir destek bileti açın

Avere vFXT dağıtma veya kullanma sırasında sorunlarla karşılaşırsanız Azure portal aracılığıyla yardım isteyin.

Destek biletinin kümenizdeki bir kaynakla etiketlendiğinden emin olmak için aşağıdaki adımları izleyin. Bilet etiketleme, doğru destek kaynağına yönlendirmemize yardımcı olur.

1. [https://portal.azure.com](https://portal.azure.com), **kaynak grupları**' nı seçin. Sorunun gerçekleştiği vFXT kümesini içeren kaynak grubuna gidin ve avere kümesi sanal makinelerinden birine tıklayın.

    ![belirli bir VM 'ye sahip Azure portal kaynak grubu "genel bakış" panelinin ekran görüntüsü](media/avere-vfxt-ticket-vm.png)

1. VM sayfasında, sol bölmenin alt kısmına gidin ve **Yeni destek isteği**' ne tıklayın.

    ![Önceki ekran görüntüsünden VM için Azure portal VM sayfasının ekran görüntüsü. Sol menü, en alta kaydırıldı ve ' yeni destek isteği ' bir daire içinde.](media/avere-vfxt-ticket-request.png)

1. Destek isteğinin ilk sayfasında, sorun türünü seçin ve doğru aboneliğin seçildiğinden emin olun.

   **Hizmet**altında, **tüm hizmetler** ' e tıklayın ve **depolama alanına** bakarak **avere vfxt**' yi seçin.

   Kısa bir Özet ekleyin ve sorun türünü seçin.

    ![Azure portal yeni bir destek isteği ekranının ekran görüntüsü. Temel bilgiler sekmesi seçilidir. Ekran öğeleri, sorun türünü, aboneliği, hizmeti, Özeti ve sorun türünü içerir.](media/ticket-basics.png)

   Devam etmek için **İleri**’ye tıklayın.

1. Destek formunun ikinci sayfası, Özet açıklamasına göre sorunu düzeltmeye yönelik öneriler içerir. Hala bir destek bileti oluşturmanız gerekiyorsa alt kısımdaki **İleri** düğmesine tıklayın.

   ![Yeni destek isteği ekranının, çözümler sekmesi seçiliyken ekran görüntüsü. Ortadaki bir metin alanı ' önerilen çözüm ' başlığına sahiptir ve olası düzeltmeleri açıklar.](media/ticket-solutions.png)

1. Üçüncü sayfada ayrıntılar sağlayın. Bu, kümeniz hakkında bilgi, sorunun oluştuğu zaman, önem derecesi ve sizinle iletişim kurma bilgilerini içerir. Bilgileri girin ve alttaki **İleri** düğmesine tıklayın.

   ![Ayrıntılar sekmesi seçili olan yeni destek isteği ekranının ekran görüntüsü. Bilgi alanları, ' sorun ayrıntıları ', ' destek yöntemi ' ve "kişi bilgileri" kategorilerine düzenlenir.](media/ticket-details.png)

1. Son sayfadaki bilgileri gözden geçirin ve **Oluştur**' a tıklayın. E-posta adresinize bir onaylama ve bilet numarası gönderilir ve bir destek personeli üyesi sizinle iletişim kuracaktır.

## <a name="request-a-quota-increase"></a>Kota artışı iste

Azure için avere vFXT 'yi dağıtmak için gereken bileşenleri öğrenmek üzere [vfxt kümesine yönelik kotayı](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) okuyun. Azure portal [bir kota artışı isteyebilirsiniz](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) .
