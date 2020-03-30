---
title: Azure Tablo depolama | Azure Marketi
description: Azure Tablo depolama alanında müşteri adayı yönetimini yapılandırın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: dsindona
ms.openlocfilehash: cee28c65b97d4ac163f78b746dc7be9ee210843c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280363"
---
# <a name="lead-management-instructions-for-table-storage"></a>Tablo depolama için müşteri adayı yönetimi yönergeleri

Bu makalede, satış müşteri adaylarını yönetmek için Azure Table depolama alanının nasıl yapılandırılabildiğini açıklanmaktadır. Tablo depolama, müşteri bilgilerini depolamanıza ve değiştirmenize yardımcı olur.

## <a name="configure-table-storage"></a>Tablo depolamayı yapılandır

1. Azure hesabınız yoksa, [ücretsiz bir deneme hesabı oluşturun.](https://azure.microsoft.com/pricing/free-trial/)
1. Hesabınız etkin olduktan sonra Azure [portalında](https://portal.azure.com)oturum açın.
1. Azure portalında aşağıdaki adımları izleyin:  
    1. Sol taraftaki bölmede **+Kaynak Oluştur'u** seçin. **Yeni** bölme açılacak.
    1. **Yeni** bölmede **Depolama'yı**seçin. Sağ tarafta **öne çıkan** bir liste açılır.
    1. **Depolama hesabı'nı**seçin. Ardından, [bir depolama hesabı oluştur'daki](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)yönergeleri izleyin.

    ![Azure Storage hesabı oluşturma](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

    Depolama hesapları hakkında daha fazla bilgi [için, Quickstart öğreticiler](https://docs.microsoft.com/azure/storage/)bakın. Fiyatlandırma bilgileri için Azure [depolama fiyatlandırması'na](https://azure.microsoft.com/pricing/details/storage/)bakın.

1. Depolama hesabınız sağlanana kadar bekleyin, bu genellikle birkaç dakika sürer. Ardından, Azure portalının ana sayfasından hesaba erişin: Gezinti bölmesindeki tüm kaynaklarınızı veya **tüm kaynaklarınızı** **görün'ü** seçin.

    ![Azure depolama hesabınıza erişin](./media/cloud-partner-portal-lead-management-instructions-azure-table/azure-storage-access.png)

1. Depolama hesabı bölmenizden, anahtar için depolama hesabı bağlantı dizesini kopyalayın. Bulut İş Ortağı Portalı'ndaki depolama hesabı için **Bağlantı Dize** alanına yapıştırın.

    Örnek bağlantı dizesi:

    ```sql
    DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
    ```

      ![Azure depolama anahtarı](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

Tablo depolama alanınızdaki verileri görmek için [Azure Depolama Gezgini'ni](https://azurestorageexplorer.codeplex.com/) veya benzer bir aracı kullanabilirsiniz. Ayrıca, ondan veri dışa aktarabilirsiniz.

## <a name="use-microsoft-flow-with-table-storage-optional"></a>Tablo depolama ile Microsoft Akışını Kullanma (*isteğe bağlı*)

Tablo depolama alanınıza bir müşteri adayı eklendiğinde bildirimleri otomatik olarak göndermek için [Microsoft Akışını](https://docs.microsoft.com/flow/) kullanabilirsiniz. Microsoft Flow hesabınız yoksa, ücretsiz [bir hesap için kaydolun.](https://flow.microsoft.com/)

### <a name="lead-notification-example"></a>Müşteri adayı bildirimi örneği

Bu örnek, temel bir akış oluşturmak için nasıl gösterir. Akış, tablo depolama alanınıza yeni müşteri adayları eklendiğinde saatlik olarak otomatik olarak bir e-posta bildirimi gönderir.

1. Microsoft Flow hesabınızda oturum açın.
1. Sol taraftaki gezinti bölmesinde **akışlarım'ı**seçin.
1. Üst teki gezinti çubuğunda **+Yeni'yi**seçin.  
1. Açılan listeden **boştan +Oluştur'u**seçin.
1. **Boştan akış oluştur**altında, **boştan Oluştur'u**seçin.

   ![Boştan yeni bir akış oluşturma](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-create-from-blank.png)

1. Bağlayıcılar ve tetikleyiciler arama sayfasında **Tetikleyiciler'i**seçin.
1. **Tetikleyiciler** **altında, Yineleme'yi**seçin.
1. **Yineleme** penceresinde, **Aralık**için **varsayılan 1** ayarını tutun. **Sıklık** açılır listesinden **Saat'i**seçin.

   >[!NOTE] 
   >Bu örnekte bir saatlik aralık kullanılır. Ancak, işletme gereksinimlerinize en uygun bir aralık ve sıklık seçebilirsiniz.

   ![Yineleme için 1 saatlik bir frekans ayarlama](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-recurrence-dropdown.png)

1. **+Yeni adım'ı**seçin.
1. Geçmiş **zamanı bul'u**arayın ve ardından **bir eylem seçin**altında geçmiş zamanı **al'ı** seçin.

    !["Geçmiş zamanı al" eylemini bulun ve seçin](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-search-getpasttime.png)

1. Geçmiş **Zaman** Al penceresinde, **Aralığı** **1**olarak ayarlayın.  Saat **birimi** açılır listesinden **Saat'i**seçin.
    >[!IMPORTANT] 
    >**Aralık** ve Zaman **biriminin** yineleme için yapılandırdığınız aralık ve sıklıkla eşleştirdiğinden emin olun (adım 8).

    ![Geçmiş zaman aralığını ayarlama](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getpast-time.png)

    >[!TIP] 
    >Her adımın doğru yapılandırıldığından doğrulamak için akışınızı istediğiniz zaman kontrol edebilirsiniz: Akış menüsü çubuğundan **Akış denetleyicisini** seçin.

Sonraki adım kümesinde, depolama tablonuza bağlanır ve yeni müşteri adaylarını işlemek için işleme mantığını ayarlarsınız.

1. Geçmiş **zaman** adımını al'dan sonra **+Yeni adım'ı**seçin ve ardından **varlıkları al'ı**arayın.
1. **Eylemler**altında, **varlıkları al'ı**seçin ve gelişmiş seçenekleri **göster'i**seçin.
1. Varlıkları **Al** penceresinde, aşağıdaki alanları doldurun:

   - **Tablo**: tablo depolamanızın adı. Aşağıdaki resimde girilen "MarketPlaceLeads" gösterilmektedir:

     ![Azure tablo adı için özel bir değer seçme](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-table-name.png)

   - **Filtre Sorgusu**: Bu alanı seçtiğinizde, açılan pencerede **Geçmiş Zamanı Al** simgesi görüntülenir. Sorguyu filtrelemek için bu değeri zaman damgası olarak kullanmak için **Geçmiş Zamanı'nı** seçin. Veya, aşağıdaki işlevi alana yapıştırabilirsiniz:
   
      `CreatedTime Timestamp gt datetime'@{body('Get_past_time')}'` 

     ![Filtre sorgu işlevini ayarlama](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-filterquery.png)

1. Tablo depolama alanınızı yeni müşteri adayları için tarayan bir koşul eklemek için **Yeni adım'ı** seçin.

   ![Tablo depolama alanını tarayabilmek için koşul eklemek için "Yeni adım"ı kullanın](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-add-filterquery-new-step.png)

1. Eylem penceresi **seç'te** **Eylemler'i**seçin ve ardından **Koşul Denetimi'ni**seçin.

     ![Koşul denetimi ekleme](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-action-condition-control.png)

1. **Durum** penceresinde, **bir değer seç'i**ve ardından açılan pencerede **İfade'yi** seçin' i seçin.
1. Fx `length(body('Get_entities')?['value'])` alanına ***fx*** yapıştırın. Bu işlevi eklemek için **Tamam'ı** seçin. 



     ![Koşula bir işlev ekleme](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-fx0.png)

1. Koşulun sonucuna göre eylemi yapacak şekilde ayarlayın.

    1. Select, açılan listeden **daha büyüktür.**
   1. Değer olarak **0** girin.

     ![Koşul sonuçlarına göre bir eylem ayarlama](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-pick-action.png)

1. Eğer durum "Hayırsa" olarak çözülürse, hiçbir şey yapmayın.

    Koşul "Evet' olarak giderılırsa, Office 365 hesabınızı e-posta göndermek için bağlayan bir eylemi tetikler:
   1. **Eylem ekle**'yi seçin.
   1. **E-posta gönder**'i seçin.
   1. **E-posta gönder** penceresinde aşağıdaki alanlara bilgi girin:

      - **Kime**: bildirimi alacak herkes için bir e-posta adresi.
      - **Konu**: e-posta için bir konu. Örneğin: *Yeni müşteri adayları!*
      - **Gövde**: her e-postaya eklemek istediğiniz metin (isteğe bağlı). Ayrıca müşteri `body('Get_entities')?['value']` adayı bilgilerini eklemek için bir işlev olarak yapıştırın.

        >[!NOTE] 
        >E-postanın gövdesine ek statik veya dinamik veri noktaları ekleyebilirsiniz.

      ![Müşteri adayı bildirimi için e-posta ayarlama](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-emailbody-fx.png)

1. Akışı kaydetmek için **Kaydet**’i seçin. Microsoft Flow hataları için otomatik olarak sınayacak. Herhangi bir hata yoksa, akışınız kaydedildikten sonra çalışmaya başlar.

    Aşağıdaki resim, son akışın nasıl görünmesi gerektiğine bir örnek gösterilmektedir.

    [![Son akış sırası](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end-thmb.png)](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end.png)

    (*Büyütmek için resmi seçin.*)

### <a name="manage-your-flow"></a>Akışınızı yönetin

Akış çalışmasını n için ardından kolayca yönetebilirsiniz. Akışınızı tam olarak kontrol ete sahipsiniz. Örneğin, bunu durdurabilir, edinebilir, bir çalışma geçmişi görebilir ve analitik elde edebilirsiniz. Aşağıdaki resimde akış yönetimi seçenekleri gösterilmektedir.

 ![Akış yönetimi seçenekleri](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-manage-completed.png)

**Akışı kapat'ı**seçene kadar akış çalışmaya devam eder.

Müşteri adayı e-posta bildirimleri almıyorsanız, tablo depolama alanınıza yeni müşteri adayları eklenmedi.
Akış hatası oluşursa aşağıdaki örnek gibi bir e-posta alırsınız:

 ![Akış hatası e-posta bildirimi](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-failure-note.png)

## <a name="next-steps"></a>Sonraki adımlar

[Müşteri adaylarını yapılandırma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)
