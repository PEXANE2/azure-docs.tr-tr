---
title: Azure Data Box Disk geri göndermeye yönelik öğretici | Microsoft Docs
description: Azure Data Box Disk'inizi Microsoft'a nasıl gönderebileceğinizi öğrenmek için bu öğreticiyi kullanın
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 9e3159a083d21e4a7a81df2fcf7a7d5db882b757
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976926"
---
::: zone target="docs"

# <a name="tutorial-return-azure-data-box-disk-and-verify-data-upload-to-azure"></a>Öğretici: Azure Data Box Disk döndürün ve verileri Azure 'a yükleyin

Bu, serinin son öğreticisidir: Azure Data Box Disk dağıtın. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Data Box Disk'i Microsoft'a gönderme
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box Disk'ten verileri silinme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce [öğreticiyi tamamladığınızdan emin olun: Azure Data Box Disk verileri kopyalayın ve doğrulayın](data-box-disk-deploy-copy-data.md).


## <a name="ship-data-box-disk-back"></a>Data Box Disk'i geri gönderme

1. Veri doğrulama tamamlandıktan sonra diskleri çıkarın. Bağlantı kablolarını çıkarın.
2. Tüm diskleri ve bağlantı kablolarını kabarcıklı naylona sarın ve bunları sevkiyat kutusuna yerleştirin. Aksesuarlar yoksa ücretler uygulanabilir.
    - Ambalajını ilk sevkıyatla yeniden kullanın.  
    - İyi güvenli bir kabarcıklanmasını sarması kullanarak disk paketetmenizi öneririz.
    - Kutunun içindeki tüm hareketleri azaltmak için Sığdırma 'nin Snug olduğundan emin olun.

Sonraki adımlar, cihazı döndürmekte olduğunuz konuma göre belirlenir.

### <a name="pick-up-in-us-canada"></a>ABD, Kanada 'da seçin

Cihazı ABD veya Kanada 'Ya döndürürken aşağıdaki adımları uygulayın.

1. Kutuya yapıştırılmış şeffaf plastik kılıftaki iade sevkiyat etiketini kullanın. Etiket hasar görmüşse veya kaybolursa:
    - **Genel bakış > Sevkiyat etiketi indir** bölümüne gidin.

        ![Sevkiyat etiketini indirme](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        Bu eylem aşağıda gösterildiği gibi bir sevkiyat etiketi indirir.

        ![Örnek sevkiyat etiketi](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Cihazdaki etiketi afden düzeltir.

2. Sevkiyat kutusunu mühürleyin ve iade sevkiyat etiketinin görünür olduğundan emin olun.
3. KESINTISIZ bir toplama zamanlayın. Bir toplama zamanlamak için:

    - Yerel UPS (ülkeye/bölgeye özel ücretsiz numara) çağırın.
    - Çağrınızda, basılı etiketinizde gösterildiği gibi ters Sevkiyat izleme numarasını teklif edin.
    - İzleme numarası tırnak işareti yoksa, UPS toplama sırasında ek ücret ödemeniz gerekir.
    - Toplama zamanlamak yerine, en yakın bırakma konumundaki Data Box Disk de bırakabilirsiniz.


### <a name="pick-up-in-europe"></a>Avrupa 'da seçin

Cihazı Avrupa 'ya döndürürken aşağıdaki adımları uygulayın.

1. Kutuya yapıştırılmış şeffaf plastik kılıftaki iade sevkiyat etiketini kullanın. Etiket hasar görmüşse veya kaybolursa:
    - **Genel bakış > Sevkiyat etiketi indir** bölümüne gidin.

        ![Sevkiyat etiketini indirme](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        Bu eylem aşağıda gösterildiği gibi bir sevkiyat etiketi indirir.

        ![Örnek sevkiyat etiketi](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Cihazdaki etiketi afden düzeltir.

2. Sevkiyat kutusunu mühürleyin ve iade sevkiyat etiketinin görünür olduğundan emin olun.
3. Cihazı Avrupa'da DHL ile iade ediyorsanız, DHL'in web sitesini ziyaret edip bir havayolu fatura numarası belirterek toplama isteğinde bulunun.
4. DHL Express web sitesine ülke/bölge ' ye gidin ve **bir Courier Collection > eReturn sevkıyatı**seçin.

    ![DHL iade sevk irsaliyesi](media/data-box-disk-deploy-picked-up/dhl-ship-1.png)
    
3. Konşimento numarasını belirtin ve toplama ayarlaması yapmak için **Toplama Zamanlama**'ya tıklayın.

      ![Toplamayı zamanla](media/data-box-disk-deploy-picked-up/dhl-ship-2.png)

### <a name="pick-up-in-asia-pacific-region"></a>Asya Pasifik bölgesinde seçin

Bu bölge, Japonya, Kore, Avustralya ve Singapur 'da toplama yönergelerini içerir.

#### <a name="pick-up-in-australia"></a>Avustralya 'da seçin

Avustralya 'daki Azure veri merkezlerinde ek bir güvenlik bildirimi vardır. Tüm gelen sevkiyatlar gelişmiş bir bildirime sahip olmalıdır. Avustralya 'da toplama için aşağıdaki adımları uygulayın.

1. Benzersiz `adbops@microsoft.com` gelen kimliği veya Tau kodu olan istek sevkiyat etiketine e-posta gönderin. Etiketi zaman içinde almak için planlı sevk tarihinden en az 3 gün önce, isteği en az 3 gün önce koyun.
2. E-posta konusu, *Tau kodlu ters Sevkiyat Etiketi Için istek*olmalıdır. E-postaya aşağıdaki ayrıntıları eklediğinizden emin olun: 

    - Sipariş adı
    - Adres
    - Kişi adı

#### <a name="pick-up-in-japan"></a>Japonya 'da seçin

1. Şirket adı ve adres bilgilerinizi, gönderici bilgileriniz olarak konsinye notuna yazın.
2. Aşağıdaki e-posta şablonunu kullanarak e-posta Quantium çözümü.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Microsoft Azure Data Box Disk｜Job Name： 
    Body: 
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
        a. 08：00-13：00 
        b. 13：00-15：00 
        c. 15：00-17：00 
        d. 17：00-19：00 
    ```
    - **Osaka 'da**yaşıyorsanız, e-posta şablonundaki konuyu şu şekilde değiştirin: `Pickup request for Microsoft Azure OSA`.
    - Japon Chakubarai konsinye veya eksik olduğunda, bu e-postada bu göz önünde olduğunu unutmayın. Quantium çözümleri Japonya, toplama sırasında konsinye dekontunu getirmek için Japon gönderi ister.
    - Birden çok siparişiniz varsa, bireysel toplama sağlamak için e-posta gönderin.

3. Bir toplama zamanladıktan sonra, Quantium çözümlerinden bir e-posta onayı alın. E-posta onayı, Chakubarai konsinye notuna ilişkin bilgileri de içerir.

Gerekirse, aşağıdaki bilgileri izleyerek Quantium çözüm desteği (Japonca dil) ile iletişim kurabilmeniz gerekir: 

- E-posta:Customerservice.JP@quantiumsolutions.com 
- Telefon: 03-5755-0150 

#### <a name="pick-up-in-korea"></a>Kore 'de Seç

1. Dönüş konsinye notuna dahil ettiğinizden emin olun.
2. Konsinye senedi mevcut olduğunda çekme isteği istemek için:
    1. Ofis saatlerinde 070-8231-1418:00 ' dan 5 ' e kadar, Pazartesi 'Den Cuma 'ya kadar, *Quantium çözümlerini Uluslararası* yardım hattı çağrısı yapın. Teklif *Microsoft Azure* toplama ve bir koleksiyon için düzenleme yapılacak hizmet isteği numarası.  
    2. Yardım hattı meşgul ise, e- `microsoft@rocketparcel.com`posta konusu *Microsoft Azure toplama* ve hizmet isteği numarası başvuru olarak e-posta.
    3. Courier koleksiyon için ulaşmazsa, diğer düzenlemeler için *Quantium çözümlerini Uluslararası* yardım hattını çağırın. 
    4. Çekme zamanlaması için bir e-posta onayı alırsınız.
3. Bu adımı yalnızca konsinye notunun mevcut olmaması durumunda yapın. Toplama istemek için:
    1. Ofis saatlerinde 070-8231-1418:00 ' dan 5 ' e kadar, Pazartesi 'Den Cuma 'ya kadar, *Quantium çözümlerini Uluslararası* yardım hattı çağrısı yapın. Teklif *Microsoft Azure* toplama ve bir koleksiyon için düzenleme yapılacak hizmet isteği numarası. Bir koleksiyon için düzenlemek üzere yeni bir konsinye notuna ihtiyacınız olduğunu belirtin. Gönderici (müşteri), alıcı bilgileri (Azure veri merkezi) ve başvuru numarası (hizmet isteği numarası) sağlayın. 
    2. Yardım hattı meşgul ise, e- `microsoft@rocketparcel.com`posta konusu *Microsoft Azure toplama* ve hizmet isteği numarası başvuru olarak e-posta.
    3. Courier koleksiyon için ulaşmazsa, diğer düzenlemeler için *Quantium çözümlerini Uluslararası* yardım hattını çağırın. 
    4. İstek telefon aracılığıyla yapılırsa bir onaylama onayı alırsınız.

### <a name="pick-up-in-singapore"></a>Singapur 'ta seçin

1. Sevkiyat etiketini yazdırın ve kutunun üzerine ekleyin. Etiket hasar görmüşse veya kaybolursa:
    - **Genel bakış > Sevkiyat etiketi indir** bölümüne gidin.

        ![Sevkiyat etiketini indirme](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        Bu eylem aşağıda gösterildiği gibi bir sevkiyat etiketi indirir.

        ![Örnek sevkiyat etiketi](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Cihazdaki etiketi afden düzeltir. Etiketin görünür olduğundan emin olun.

2. Toplama istemek için:
    - Office saatlerinde (00-17:00, Pazartesi 'Den Cuma 'ya kadar) bir **Mapost** **6845 6485** hattı çağrısı yapın.  
    - Teklif *Microsoft Azure* toplama ve hizmet istek numarası (iade sevkiyat etiketindeki izleme numarası) bir koleksiyon için düzenleme yapılır. 
    - Çekme zamanlaması için bir onaylama onayı alacaksınız. 
    - Courier Collection 'a ulaşmazsa, alternatif düzenlemeler için **6845 6485** adresindeki **singpost** 'u çağırın. 
3. Courier 'e el ile. 


## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

Diskler nakliyeciniz tarafından toplandıktan sonra, portaldaki sipariş durumu **Toplandı** olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.

![Diskler toplandı](media/data-box-disk-deploy-picked-up/data-box-portal-pickedup.png)

Microsoft diski alıp taradığında, iş durumu **Alındı** olarak güncelleştirilir. 

![Diskler alındı](media/data-box-disk-deploy-picked-up/data-box-portal-received.png)

Diskler Azure veri merkezindeki bir sunucuya bağlandıktan sonra veriler otomatik olarak kopyalanır. Verilerin boyutuna bağlı olarak, kopyalama işleminin tamamlanması birkaç saatten bir güne kadar sürebilir. Kopyalama işinin ilerleme durumunu portalda izleyebilirsiniz.

Kopyalama tamamlandıktan sonra, sipariş durumu **Tamamlandı** olarak güncelleştirilir.

![Veri kopyalama tamamlandı](media/data-box-disk-deploy-picked-up/data-box-portal-completed.png)

Kopyalama hatalarla tamamlanırsa, bkz. [karşıya yükleme hatalarını giderme](data-box-disk-troubleshoot-upload.md).

Kaynaktan silmeden önce verilerinizin depolama hesaplarında olduğundan emin olun. Verileriniz şu durumda olabilir:

- Azure depolama hesabınız. Data Box'a veri kopyaladığınızda, türlerine bağlı olarak bu veriler Azure Depolama hesabınızda aşağıdaki yollardan birine yüklenir.

  - Blok blobları ve sayfa blobları için: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
  - Azure Dosyaları için: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

    Alternatif olarak Azure portalda Azure depolama hesabınıza gidip oradan ilerleyebilirsiniz.

- Yönetilen disk kaynak grubunuz. Yönetilen diskler oluştururken, VHD 'ler sayfa Blobları olarak yüklenir ve ardından yönetilen disklere dönüştürülür. Yönetilen diskler, sipariş oluşturma sırasında belirtilen kaynak gruplarına iliştirilir.

  - Azure 'da yönetilen disklere kopyalama işlemi başarılı olduysa, Azure portal **sipariş ayrıntılarına** gidebilir ve yönetilen diskler için belirtilen kaynak grubunu bir yere göz önünde yapabilirsiniz.

      ![Sipariş ayrıntılarını görüntüle](media/data-box-disk-deploy-picked-up/order-details-resource-group.png)

    Belirtilen kaynak grubuna gidin ve yönetilen disklerinizi bulun.

      ![Yönetilen diskler için kaynak grubu](media/data-box-disk-deploy-picked-up/resource-group-attached-managed-disk.png)

  - Bir VHDX veya dinamik/fark kayıt VHD 'SI kopyaladıysanız, VHDX/VHD, bir Blok Blobu olarak hazırlama depolama hesabına yüklenir. **Blob > hazırlama depolama hesabınıza** gidin ve ardından uygun kapsayıcıyı seçin-Standardssd, standardhdd veya PremiumSSD. VHDX/VHD 'ler, hazırlama depolama hesabınızda blok Blobları olarak gösterilmelidir.

Verilerin Azure'a yüklendiğini doğrulamak için aşağıdaki adımları izleyin:

1. Disk siparişinizle ilişkilendirilmiş depolama hesabına gidin.
2. **Blob hizmeti > Bloblara göz atın** seçeneğine gidin. Kapsayıcı listesi gösterilir. *BlockBlob* ve *PageBlob* klasörlerinin altında oluşturduğunuz alt klasöre karşılık olarak, depolama hesabınızda aynı adlı kapsayıcılar oluşturulur.
    Klasör adları Azure adlandırma kurallarına uymuyorsa, Azure'a veri yükleme işlemi başarısız olur.

4. Veri kümesinin tamamının yüklendiğini doğrulamak için, Microsoft Azure Depolama Gezgini'ni kullanın. Disk kiralama siparişine karşılık gelen depolama hesabını ekleyin ve ardından blob kapsayıcılarının listesine bakın. Bir kapsayıcı seçin, **…Diğer** düğmesine tıklayın ve sonra da **Klasör istatistikleri**'ne tıklayın. **Etkinlikler** bölmesinde, blob sayısı ve toplam blob boyutu gibi o klasöre özgü istatistikler görüntülenir. Bayt cinsinden toplam blob boyutu, veri kümesinin boyutuyla eşleşmelidir.

    ![Depolama Gezgini'nde klasör istatistikleri](media/data-box-disk-deploy-picked-up/folder-statistics-storage-explorer.png)

## <a name="erasure-of-data-from-data-box-disk"></a>Data Box Disk'ten verileri silinme

Kopyalama tamamlandıktan ve siz de verilerin Azure depolama hesabında olduğunu doğruladıktan sonra, diskler NIST standardına uygun olarak güvenle silinir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box Disk konularını öğrendiniz:

> [!div class="checklist"]
> * Data Box Disk'i Microsoft'a gönderme
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box Disk'ten verileri silinme


Azure portalı yoluyla Data Box Disk'i yönetmeyi öğrenmek için bir sonraki nasıl yapılır makalesine geçin.

> [!div class="nextstepaction"]
> [Azure portalını kullanarak Azure Data Box Disk'i yönetme](./data-box-portal-ui-admin.md)

::: zone-end


::: zone target="chromeless"

# <a name="return-the-disk-and-verify-upload-to-azure"></a>Diski döndürün ve Azure 'a yüklemeyi doğrulayın

## <a name="ship-to-azure"></a>Azure'a gönderme

1. Veri doğrulama işlemi tamamlandıktan sonra diskleri çıkarın. Bağlantı kablolarını çıkarın.
2. Tüm diskleri ve bağlantı kablolarını kabarcıklı naylona sarın ve bunları sevkiyat kutusuna yerleştirin. Aksesuarlar yoksa ücretler uygulanabilir.
    - Ambalajını ilk sevkıyatla yeniden kullanın.  
    - İyi güvenli bir kabarcıklanmasını sarması kullanarak disk paketetmenizi öneririz.
    - Kutunun içindeki tüm hareketleri azaltmak için Sığdırma 'nin Snug olduğundan emin olun.
3. Sonraki adımlar, cihazı döndürmekte olduğunuz konuma göre belirlenir.
    - [CIHAZı ABD ve Kanada 'ya DÖNDÜRÜRKEN UPS ile bir toplama zamanlayın](data-box-disk-deploy-picked-up.md#pick-up-in-us-canada).
    - [Avrupa için DHL ile birlikte bir toplama zamanlayın](data-box-disk-deploy-picked-up.md#pick-up-in-europe) ve Web sitesini ziyaret edin ve Airway fatura numarasını belirtin.
    - Avustralya, Japonya, Kore ve Singapur gibi [Avustralya-Pasifik bölgesindeki ülkeler için bir çekme planı zamanlayın](data-box-disk-deploy-picked-up.md#pick-up-in-asia-pacific-region) .
4. Bu diskler, taşıyıcınızın üzerinden çekildikten sonra, Portal güncelleştirmelerinde sipariş durumu ve izleme KIMLIĞI görüntülenir.

## <a name="verify-upload-to-azure"></a>Azure 'a yüklemeyi doğrulama

Veriler Azure 'a yüklendikten sonra, kaynaktan silmeden önce verilerinizin depolama hesaplarına ait olduğundan emin olun. Verileriniz şu durumda olabilir:

- Azure depolama hesabınız. Data Box'a veri kopyaladığınızda, türlerine bağlı olarak bu veriler Azure Depolama hesabınızda aşağıdaki yollardan birine yüklenir.

    - **Blok Blobları ve sayfa Blobları için**: https://< storage_account_name >. blob. Core. Windows.<containername>NET//Files/a.txt

    - **Azure dosyaları için**: https://< storage_account_name >. File. Core. Windows. net/<sharename>/Files/a.txt

    Alternatif olarak Azure portalda Azure depolama hesabınıza gidip oradan ilerleyebilirsiniz.

- Yönetilen disk kaynak grubunuz. Yönetilen diskler oluştururken, VHD 'ler sayfa Blobları olarak yüklenir ve ardından yönetilen disklere dönüştürülür. Yönetilen diskler, sipariş oluşturma sırasında belirtilen kaynak gruplarına iliştirilir.

::: zone-end


