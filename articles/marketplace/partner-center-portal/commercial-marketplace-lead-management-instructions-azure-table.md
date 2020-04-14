---
title: Azure Tablo | Azure Marketi
description: Azure Tablosu için müşteri adayı yönetimini yapılandırın.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: dsindona
ms.openlocfilehash: a36c411b9ababc42adb51d82a316df4252c01e24
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81252187"
---
# <a name="configure-lead-management-using-an-azure-table"></a>Azure Tablosu'nu kullanarak müşteri adayı yönetimini yapılandırma

Müşteri İlişkileri Yönetimi (CRM) sisteminiz Azure Marketi ve AppSource müşteri adaylarını almak için İş Ortağı Merkezi'nde açıkça desteklenmiyorsa, bu müşteri adaylarını işlemek için bir Azure Tablosu kullanabilirsiniz. Daha sonra verileri dışa aktarmayı ve CRM sisteminize aktarmayı seçebilirsiniz. Bu makaledeki yönergeler, bir Azure Depolama hesabı ve bu hesabın altında bir Azure Tablosu oluşturma sürecinde size yol açacaktır. Ayrıca, teklifiniz bir müşteri adayı aldığında e-posta bildirimi göndermek için Microsoft Flow'u kullanarak yeni bir akış oluşturabilirsiniz.

## <a name="configure-azure-table"></a>Azure Tablosunu Yapılandırma

1. Azure hesabınız yoksa, [ücretsiz bir deneme hesabı oluşturabilirsiniz.](https://azure.microsoft.com/pricing/free-trial/)
2. Azure hesabınız etkin olduktan sonra [Azure portalında](https://portal.azure.com)oturum açın.
3. Azure portalında, aşağıdaki yordamı kullanarak bir depolama hesabı oluşturun.  
    1. Sol menü çubuğunda **+Kaynak Oluştur'u** seçin.  **Yeni** bölme (bıçak) sağda görüntülenir.
    2. **Yeni** bölmede **Depolama'yı** seçin.  **Öne Çıkanlar** listesi sağda görüntülenir.
    3. Hesap oluşturmaya başlamak için **Depolama Hesabı'nı** seçin.  Makaledeki yönergeleri izleyin [Bir depolama hesabı oluşturun.](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)

        ![Azure depolama hesabı oluşturma adımları](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-storage-create.png)

        Depolama hesapları hakkında daha fazla bilgi için [Hızlı Başlangıç öğreticisi'ni](https://docs.microsoft.com/azure/storage/)seçin.  Depolama fiyatlandırması hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/pricing/details/storage/)

4. Depolama hesabınız sağlanana kadar bekleyin, bu işlem genellikle birkaç dakika sürer.  Ardından, **tüm kaynaklarınızı Gör'ü** seçerek veya Azure portalının sol navigasyon menüsü çubuğundan **tüm kaynakları** seçerek Azure portalının **Giriş** sayfasından depolama hesabınıza erişin.

    ![Azure depolama hesabınıza erişin](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-storage-access.png)

5. Depolama hesabı bölmenizden **Access tuşlarını** seçin ve anahtar için *Bağlantı dize* değerini kopyalayın. Pazar yeri teklifiniz için müşteri adaylarını almak için yayımlama portalında sağlamanız gereken *Depolama Hesabı Bağlantı String* değeri olduğu için bu değeri kaydedin. 

    Bir bağlantı sokması bir örnek:

    ```sql
    DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
    ```

    ![Azure depolama anahtarı](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-storage-keys.png)

6. Depolama hesabı bölmenizden **Tablolar'ı** seçin ve tablo oluşturmak için **+Tablo'yu** seçin. Tablonuz için bir ad girin ve **Tamam'ı**seçin. Müşteri adayları geldiğinde e-posta bildirimleri almak için bir MS Akışını yapılandırmak istiyorsanız, bu değeri ihtiyacınız olduğu şekilde kaydedin.

    ![Azure tablolar](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-tables.png)

    Depolama tablonuzdaki verileri görmek için [Azure depolama gezginini](https://archive.codeplex.com/?p=azurestorageexplorer) veya başka bir aracı kullanabilirsiniz. Verileri Azure Tablosu'nda da dışa aktarabilirsiniz. 

## <a name="optional-use-microsoft-flow-with-an-azure-table"></a>(İsteğe bağlı) Azure Tablosuyla Microsoft Akışını Kullanma  

Azure tablosuna her müşteri adayı ekilen bildirimleri otomatikleştirmek için [Microsoft Akışı'nı](https://docs.microsoft.com/flow/) kullanabilirsiniz. Hesabınız yoksa, ücretsiz bir hesap [için kaydolabilirsiniz.](https://flow.microsoft.com/)

### <a name="lead-notification-example"></a>Müşteri adayı bildirimi örneği

Azure tablosuna yeni bir müşteri adayı eklendiğinde otomatik olarak e-posta bildirimi gönderen basit bir akış oluşturmak için bu örneği kılavuz olarak kullanın. Bu örnek, tablo depolama alanı güncelleştirilmişse, her saat başı bilgileri göndermek için bir yineleme ayarlar.

1. Microsoft Flow hesabınızda oturum açın.
2. Soldaki gezinti çubuğunda **akışlarım'ı**seçin.
3. Üstteki gezinti çubuğunda **+ Yeni'yi**seçin.  
4. Açılan listede , **+ Zamanlanmış - boş seçin**

   ![Akışlarım **+ Zamanlanmış - boştan**](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-scheduled-from-blank.png)

5.    Yapı'da, aralık için "1" ve sıklık için "saat" için her seçiyi *yinele'nin* altında *zamanlanmış bir akış* penceresi oluşturun. Ayrıca, isterseniz akışı bir isim verin. **Oluştur'u**seçin.

>[!Note]
>Bu örnek1 saatlik bir aralık kullansa da, iş gereksinimleriniz için en iyi aralığı ve sıklığı seçebilirsiniz.

![Zamanlanmış bir akış oluşturun.](./media/commercial-marketplace-lead-management-instructions-azure-table/build-scheduled-flow.png)

6. **+ Yeni adım**’ı seçin.
7. "Geçmiş zamanı al" için *bir eylem* penceresi seçin ve ardından Eylemler altında geçmiş **zamanı al'ı** seçin.

   ![Bir eylem seçin.](./media/commercial-marketplace-lead-management-instructions-azure-table/choose-an-action.png)

8. Geçmiş **Zaman** Penceresinde, **Aralığı** 1 olarak ayarlayın. Saat **birimi** açılır listesinden **Saat'i**seçin.

    >[!Important]
    >Bu Aralık ve Zaman biriminin, adım 5'te Yineleme için yapılandırdığınız Aralık ve Sıklık ile eşleştiğinden emin olun.

    ![Geçmiş zaman aralığını ayarlama](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-getpast-time.png)

>[!TIP] 
>Her adımın doğru şekilde yapılandırıldığından doğrulamak için akışınızı istediğiniz zaman kontrol edebilirsiniz. Akışınızı kontrol etmek için Akış menüsü çubuğundan **Akış denetleyicisini** seçin.

Sonraki adım kümesinde, Azure tablonuza bağlanır ve yeni müşteri adaylarını işlemek için işleme mantığını ayarlarsınız.

9. Geçmiş zaman adımını al'dan sonra **+ Yeni adımını**seçin ve ardından eylem penceresi *seç'te* "Varlıkları al" seçeneğini arayın.
10. **Eylemler**altında, **varlıkları al (Azure Tablo Depolama) seçeneğini**belirleyin.
11.    Azure **Tablo Depolama** penceresinde, aşağıdaki alanlar için bilgi sağlayın ve **Oluştur:'ı**seçin:
* *Bağlantı Adı* - bu akış ve Azure Tablosu arasında kurduğunuz bağlantı için anlamlı bir ad sağlayın.
* *Depolama Hesabı Adı* - Azure tablonuz için depolama hesabının adını sağlayın. Bunu depolama hesabının **Erişim anahtarları** sayfasında bulabilirsiniz.
* *Paylaşılan Depolama Anahtarı* - Azure tablonuz için mağaza hesabınız için anahtar değerini sağlayın. Bunu depolama hesabının **Erişim anahtarları** sayfasında bulabilirsiniz.
    ![Azure Tablo depolama.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-storage.png)

Oluştur'u tıklattıktan sonra *varlıkları al* penceresi görürsünüz. Burada **gelişmiş seçenekleri göster'i** seçin ve aşağıdaki alanlar için bilgi sağlayın:
* *Tablo* - Azure Tablo Depolama alanınızın adını seçin (Azure tablosunu nasıl yapılandıracağınıza ilişkin yönergelerin 6. adımından). Bir sonraki ekran yakalama, bu örnek için "marketplaceleads" tablosu seçildiğinde istemi gösterir.
    ![Azure Tablo varlıkları olsun.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-get-entities.png)

* *Filtre Sorgusu* - Bu alanı seçin ve bu `Timestamp gt datetime'@{body('Get_past_time')}'` ![işlevi alana yapıştırın: Azure Tablosu varlıkları olsun - Filtre Sorgusu.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-get-entities-filter-query.png)

12. Azure tablosuna bağlantıyı ayarlamayı tamamladığınızda, Azure tablosunu yeni müşteri adayları için tarayabilmek için bir koşul eklemek için **Yeni adım'ı** seçin. 

13. Eylem penceresi **seç'te** **Eylemler'i**seçin ve ardından **Durum denetimini**seçin.

    ![Azure Tablosu - Bir eylem seçin.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-choose-an-action.png)

14. **Durum** penceresinde, değer alanı **seç'i** seçin ve ardından açılan pencerede **İfade'yi** seçin.

15. Fx `length(body('Get_entities')?['value'])` alanına ***fx*** yapıştırın. Bu işlevi eklemek için **Tamam'ı** seçin. 

16. Koşulu ayarlamayı bitirmek için:
    1. Açılan listeden "büyüktür" seçeneğini belirleyin.
    2. Değer olarak 0 girin

        ![Azure Tablo - Durum.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition.png)

Sonraki birkaç adımda, koşulun sonucuna bağlı olarak eylemi yapacak şekilde ayarlarsınız.

* Eğer durum eğer **hayır**olarak çözülürse, hiçbir şey yapmayın.
* Koşul **evet**ise olarak çözülürse, e-posta göndermek için Office 365 hesabınıza bağlanan bir eylemi tetikler. 

17. **Evet'in**altında **eylem ekle'yi** seçin.

    ![Azure Tablo - Koşul, **Evet ise**.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition-if-yes.png)

18. **E-posta gönder'i (Office 365 Outlook) seçin.**

    ![Azure Tablo - Koşul, **Evet**, e-posta gönderin.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition-if-yes-send-email.png)

    >[!Note]
    >Farklı bir e-posta sağlayıcısı aramak istiyorsanız ve eylem olarak e-posta bildirimi (Posta) gönder'i seçin. Yönergeler, Office 365 Outlook'u kullanarak nasıl yapılandıracağınızı gösterir, ancak talimatlar farklı bir e-posta sağlayıcısı için benzerdir.

19. Office **365 Outlook** penceresinde, aşağıdaki alanlar için bilgi sağlayın:

    1. **To** - Bu bildirimi alacak herkes için bir e-posta adresi girin.
    1. **Konu** - E-posta için bir konu sağlayın. Örneğin: Yeni müşteri adayları!
    1. **Gövde** - Her e-postaya eklemek istediğiniz metni ekleyin (isteğe `body('Get_entities')?['value']`bağlı) ve ardından gövdeye yapıştırın.

    >[!Note]
    >Bu e-postanın gövdesine ek statik veya dinamik veri noktaları ekleyebilirsiniz.

    ![Azure Tablo - Koşul, **Evet ise**, Office 365 Outlook penceresi.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition-if-yes-outlook.png)

20. Akışı kaydetmek için **Kaydet**’i seçin. Microsoft Flow, akışı otomatik olarak hatalar için sınar. Herhangi bir hata yoksa, akışınız kaydedildikten sonra çalışmaya başlar.

Bir sonraki ekran yakalama, son akışın nasıl görünmesi gerektiğine bir örnek gösterir.

![Son akış bir örnek.](./media/commercial-marketplace-lead-management-instructions-azure-table/final-flow.png)

### <a name="manage-your-flow"></a>Akışınızı yönetin

Akışınızı çalıştırdıktan sonra yönetmek kolaydır. Akışınızı tam olarak kontrol ete sahipsiniz. Örneğin, bunu durdurabilir, edinebilir, bir çalışma geçmişi görebilir ve analitik elde edebilirsiniz. Sonraki ekran yakalama, akışı yönetmek için kullanılabilen seçenekleri gösterir. 

 ![Akışı yönetme](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-manage-completed.png)

**Akış, Turn akışını kapatma** seçeneğini kullanarak siz durduruna kadar çalışmaya devam eder.

Müşteri adayı e-posta bildirimleri almıyorsanız, bu yeni müşteri adaylarının Azure tablosuna eklenmediğiniz anlamına gelir. Akış hataları varsa, bir sonraki ekran yakalamaörneğinde olduğu gibi bir e-posta alırsınız.

 ![Akış hatası e-posta bildirimi](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-failure-note.png)

## <a name="configure-your-offer-to-send-leads-to-the-azure-table"></a>Müşteri adaylarını Azure Tablosuna gönderecek şekilde teklifinizi yapılandırın

Yayımlama portalında teklifiniz için müşteri adayı yönetimi bilgilerini yapılandırmaya hazır olduğunuzda aşağıdaki adımları izleyin:

1. **Teklifiniz** için Teklif kurulum sayfasına gidin.
2. Müşteri Adayı Yönetimi bölümü altında **Bağlan'ı** seçin.
3. Bağlantı ayrıntıları açılır penceresinde, **Müşteri Adayı Hedefi**için Azure **Tablosu'nu** seçin ve Depolama hesabı **bağlantı dizesi** alanına daha önceki adımları izleyerek oluşturduğunuz Azure depolama hesabından bağlantı dizesine yapıştırın.
4. **Kişi e-postası** - Şirketinizde yeni bir müşteri adayı geldiğinde e-posta bildirimleri alması gereken kişiler için e-posta sağlayın. Birden çok e-postayı yarı kolon ile ayırarak sağlayabilirsiniz.
5. **Tamam**’ı seçin.

Bir müşteri adayı hedefine başarıyla bağlandığınızdan emin olmak için doğrulama düğmesini tıklatın. Başarılı olursa, müşteri adayı hedefinde bir test müşteri adayınız olur.

>[!Note]
>Teklifin geri kalanını yapılandırmayı bitirmeniz ve teklifiçin müşteri adayı alabilmek için yayımlamanız gerekir.

Müşteri adayları oluşturulduğunda, Microsoft müşteri adaylarını Azure Tablosu'na gönderir. Bir akışı yapılandırırsanız, yapılandırdığınız e-posta adresine de bir e-posta gönderilir.

![Müşteri adayı yönetimi](./media/commercial-marketplace-lead-management-instructions-azure-table/lead-management.png)

![Müşteri adayı yönetimi - bağlantı ayrıntıları](./media/commercial-marketplace-lead-management-instructions-azure-table/connection-details.png)

![Müşteri adayı yönetimi - bağlantı ayrıntıları depolama hesabı](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-connection-details.png)

