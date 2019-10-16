---
title: Azure Lab Services sınıf laboratuvarlarında kullanım ayarlarını yapılandırma | Microsoft Docs
description: Laboratuvar için Kullanıcı sayısını yapılandırmayı, laboratuvara kaydolmalarını ve VM 'yi kullanbilecekleri saat sayısını denetlemeyi ve daha fazlasını öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2019
ms.author: spelluru
ms.openlocfilehash: 691907d1c221283f99ba59f0937cfbaf673f427a
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72324532"
---
# <a name="add-and-manage-lab-users"></a>Laboratuvar kullanıcılarını ekleme ve yönetme
Bu makalede, laboratuvara Kullanıcı ekleme, laboratuvarda nasıl kayıt yapılacağı, VM 'nin kullanabileceği saat sayısını denetleme ve daha fazla bilgi edinebilirsiniz. 


## <a name="add-users-to-the-lab"></a>Laboratuvara Kullanıcı ekleme

1. Sol menüdeki **Kullanıcılar** ' ı seçin. Varsayılan olarak, **erişimi kısıtla** seçeneği etkinleştirilmiştir. Bu ayar açık olduğunda, Kullanıcı kullanıcılar listesinde olmadığı takdirde Kullanıcı kayıt bağlantısına sahip olsa bile, Kullanıcı laboratuvara kayıt yapamıyor. Yalnızca listedeki kullanıcılar, göndereceğiniz kayıt bağlantısını kullanarak laboratuvara kayıt yapabilir. Bu yordamda, kullanıcıları listeye eklersiniz. Alternatif olarak, kayıt bağlantısı olduğu sürece kullanıcıların laboratuvara kaydolmaları için **erişimi kısıtla**' yı devre dışı bırakabilirsiniz. 
2. Araç çubuğundan **Kullanıcı Ekle** ' yi seçin ve ardından **e-posta adreslerine göre Ekle**' yi seçin. 

    ![Kullanıcı Ekle düğmesi](../media/how-to-configure-student-usage/add-users-button.png)
1. **Kullanıcı Ekle** sayfasında, kullanıcıların e-posta adreslerini ayrı satırlarda veya noktalı virgülle ayrılmış tek bir satıra girin. 

    ![Kullanıcı e-posta adresleri Ekle](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. **Kaydet**’i seçin. Kullanıcıların e-posta adreslerini ve bunların durumlarını (kayıtlı veya değil) listede görürsünüz. 

    ![Kullanıcı listesi](../media/how-to-configure-student-usage/users-list-new.png)

### <a name="add-users-by-uploading-a-csv-file"></a>CSV dosyası karşıya yükleyerek Kullanıcı ekleme
Kullanıcıları, kullanıcıların e-posta adreslerine sahip bir CSV dosyası karşıya yükleyerek da ekleyebilirsiniz.

1. Tek bir sütundaki kullanıcıların e-posta adreslerini içeren bir CSV dosyası oluşturun.

    ![Kullanıcılar ile CSV dosyası](../media/how-to-configure-student-usage/csv-file-with-users.png)
2. Laboratuvarın **Kullanıcılar** sayfasında, araç çubuğunda **Kullanıcı Ekle** ' yı seçin ve ardından **CSV 'yi karşıya yükle**' yi seçin.

    ![CSV 'yi Karşıya Yükle düğmesi](../media/how-to-configure-student-usage/upload-csv-button.png)
3. Kullanıcı e-posta adreslerini içeren CSV dosyasını seçin. CSV dosyasını seçtikten sonra **Aç** ' ı seçtiğinizde, aşağıdaki **Kullanıcı Ekle** penceresini görürsünüz. E-posta adresi listesi CSV dosyasındaki e-posta adresleriyle doldurulur. 

    ![CSV dosyasından e-posta adresleriyle doldurulmuş Kullanıcı ekleme penceresi](../media/how-to-configure-student-usage/add-users-window.png)
4. **Kullanıcı Ekle** penceresinde **Kaydet** ' i seçin. 
5. Kullanıcılar listesinde kullanıcıları görtığınızdan emin olun. 

    ![Eklenen kullanıcıların listesi](../media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="send-invitations-to-users"></a>Kullanıcılara davet gönder
Öğrenciye kayıt bağlantısını göndermek için aşağıdaki yöntemlerden birini kullanın. İlk yöntem, öğrenciler için kayıt bağlantısı ve isteğe bağlı bir ileti ile e-posta gönderme işlemlerinin nasıl yapılacağını gösterir. İkinci yöntemde, başkalarıyla dilediğiniz şekilde paylaşabileceğiniz kayıt bağlantısını nasıl alacağınız gösterilmektedir. 

Laboratuvar için **erişimi kısıtla** etkinleştirilirse, yalnızca Kullanıcı listesindeki kullanıcılar, laboratuvara kaydolmak için kayıt bağlantısını kullanabilir. Bu seçenek varsayılan olarak etkindir. 

### <a name="invite-all-users"></a>Tüm kullanıcıları davet et

1. Laboratuvarın **Kullanıcılar** sayfasına geçin. 
2. Araç çubuğundan **Tümünü davet et** ' i seçin. 
3. Kullanıcılara bir **ileti** girin. Bu, isteğe bağlı bir adımdır.
4. Sonra **Gönder**' i seçin.

    ![Tüm kullanıcıları davet et](../media/how-to-configure-student-usage/invite-all.png)

    Bu işlemin durumunu **Kullanıcılar** listesinin **davet** sütununda görürsünüz. Davet e-postası, kullanıcıların laboratuvara kaydolmak için kullanabileceği kayıt bağlantısını içerir. 

### <a name="invite-selected-users"></a>Seçili kullanıcıları davet et

1. Listeden bir kullanıcı veya birden çok kullanıcı seçin. 
2. Ardından, seçili satırda gördüğünüz **zarf** simgesini seçin (veya) araç çubuğunda **davet** ' i seçin. 

    ![Seçili kullanıcıları davet et](../media/how-to-configure-student-usage/invite-selected-users.png)
3. **E-posta ile davet gönder** penceresinde, isteğe bağlı bir **ileti**girin ve ardından **Gönder**' i seçin. 

    ![Seçili kullanıcılara e-posta gönder](../media/how-to-configure-student-usage/send-invitation-to-selected-users.png)

    Bu işlemin durumunu **Kullanıcılar** listesinin **davet** sütununda görürsünüz. Davet e-postası, kullanıcıların laboratuvara kaydolmak için kullanabileceği kayıt bağlantısını içerir.

1. Zaten sayfada değilseniz, **Kullanıcılar** görünümüne geçin. 

## <a name="get-registration-link"></a>Kayıt bağlantısını al
Ayrıca, portaldan kayıt bağlantısını alabilir ve kendi e-posta istemci uygulamanızı kullanarak gönderebilirsiniz. 

1. Sol menüdeki **Kullanıcılar** ' a tıklayarak **Kullanıcılar** görünümüne geçin. 
2. Seç **... (üç nokta)** araç çubuğunda **kayıt bağlantısı**' nı seçin.

    ![Öğrenci kaydı bağlantısı](../media/how-to-configure-student-usage/registration-link-button.png)
1. **Kullanıcı kaydı** Iletişim kutusunda **Kopyala** düğmesini seçin. Bağlantı, panoya kopyalanır. Bağlantıyı bir e-posta düzenleyiciye yapıştırın ve öğrenciye e-posta gönderin. 

    ![Öğrenci kaydı bağlantısı](../media/how-to-configure-student-usage/registration-link.png)
2. **Kullanıcı kaydı** Iletişim kutusunda **bitti**' yi seçin. 
4. Öğrencinin sınıfa kaydolabilmesi için **kayıt bağlantısını** bir öğrenciye gönderin. 

## <a name="view-users-registered-with-the-lab"></a>Laboratuvara kayıtlı kullanıcıları görüntüleme

Laboratuvara kayıtlı kullanıcıların listesini görmek için sol menüdeki **Kullanıcılar** ' ı seçin. 

![Laboratuvara kayıtlı kullanıcıların listesi](../media/how-to-configure-student-usage/students-registered.png)

## <a name="set-quotas-for-users"></a>Kullanıcılar için kota ayarlama
Aşağıdaki adımları kullanarak, Kullanıcı başına kota ayarlayabilirsiniz: 

1. Sayfa zaten etkin değilse Sol menüdeki **Kullanıcılar** ' ı seçin. 
2. **Kullanıcı başına kota seçin: araç çubuğunda &lt;sayı @ no__t-2 saat** . 
3. **Kullanıcı başına kota** sayfasında, zamanlanan sınıf süresi dışında her kullanıcıya (öğrenci) vermek istediğiniz saat sayısını belirtin ve ardından **Kaydet**' i seçin.

    ![Kullanıcı başına kota](../media/how-to-configure-student-usage/quota-per-user.png)    
5. Değiştirilen değerleri şimdi araç çubuğunda görürsünüz: **Kullanıcı başına kota: &lt; saat @ no__t-2**. 

    ![Kullanıcı başına kota-sonra](../media/how-to-configure-student-usage/quot-per-user-after.png)

    > [!IMPORTANT]
    > [VM 'lerin zamanlanan çalışma süresi](how-to-create-schedules.md) , bir kullanıcıya ayrılan kotaya göre sayılmaz. Kota, bir öğrencinin VM 'lerde harcadığı zamanlama saatleri dışında bir süredir. 

## <a name="set-additional-quota-for-a-specific-user"></a>Belirli bir kullanıcı için ek kota ayarlama
Bir kullanıcı için ayrı bir kota ayarlayabilirsiniz. Bunu yapmak için şu adımları izleyin:

1. **Kullanıcılar** sayfasındaki kullanıcılar listesinden bir Kullanıcı (öğrenci) seçin.
2. Ardından araç çubuğundan **kotayı ayarla** ' yı seçin. 

    ![Kotayı ayarla düğmesi](../media/how-to-configure-student-usage/adjust-quota-button.png)
3. Seçili kullanıcı veya kullanıcılar için **ek saat** sayısını girin ve ardından **Uygula**' yı seçin. 

    ![Bir kullanıcı için ek kota](../media/how-to-configure-student-usage/additional-quota.png)
4. **Kullanım** sütununda kullanıcının güncelleştirilmiş kullanımını görürsünüz. 

    ![Kullanıcı için yeni kullanım](../media/how-to-configure-student-usage/new-usage-hours.png)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Yönetici olarak, laboratuvar hesapları oluşturun ve yönetin](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak, Labs oluşturma ve yönetme](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak, şablonları ayarlama ve yayımlama](how-to-create-manage-template.md)
- [Laboratuvar kullanıcısı olarak, sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)
