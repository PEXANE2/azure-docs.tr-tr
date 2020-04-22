---
title: VM resminiz için paylaşılan erişim imzası URI'yi alın | Azure Marketi
description: Bu makalede, her sanal sabit disk (VHD) için paylaşılan erişim imzası (SAS) URI nasıl alınır açıklanmaktadır.
author: anbene
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: mingshen
ms.openlocfilehash: c226d35647e4a5a2b1d583dd6328bfb73dae2a1c
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732631"
---
# <a name="get-shared-access-signature-uri-for-your-vm-image"></a>VM resminiz için paylaşılan erişim imzası URI'yi alın

> [!IMPORTANT]
> Azure Sanal Makine tekliflerinizin yönetimini Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşıyoruz. Tekliflerin izlene kadar, tekliflerinizi yönetmek için Bulut İş Ortağı Portalı [için VM resminiz için paylaşılan erişim imzası URI'ye bakın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-get-sas-uri)

Bu makalede, her sanal sabit disk (VHD) için paylaşılan erişim imzası (SAS) tek düzen kaynak tanımlayıcısı (URI) nasıl oluşturacağı açıklanmaktadır.

Yayımlama işlemi sırasında, planlarınız ile ilişkili her VHD için bir URI sağlamanız gerekir. Bu planlar daha önce SK'ler veya stok tutma birimleri olarak anılırdı. Microsoft'un sertifika işlemi sırasında bu VHD'lere erişmesi gerekir. Bu URI'yi Ortak Merkezi'ndeki **Planlar** sekmesine girersiniz.

VH'leriniz için SAS URI'leri oluştururken aşağıdaki gereksinimleri izleyin:

* Yalnızca yönetilmeyen VHD'ler desteklenir.
* Yalnızca `List` `Read` ve izinler gereklidir. Yaz veya Sil erişimi sağlamayın.
* Erişim süresi (son kullanma tarihi) SAS URI oluşturulduğundan itibaren en az üç hafta olmalıdır.
* UTC saat değişikliklerine karşı korunmak için başlangıç tarihini geçerli tarihten bir gün öncesine ayarlayın. Örneğin, geçerli tarih 6 Ekim 2019 ise, 10/5/2019'u seçin.

## <a name="generate-the-sas-address"></a>SAS adresini oluşturma

SAS adresi (URL) oluşturmak için kullanılan iki ortak araç vardır:

* **Microsoft Storage Explorer** – Windows, macOS ve Linux için grafik selami kullanılabilir.
* **Microsoft Azure CLI** – Windows'a uygun olmayan işletim sistemleri ve otomatik veya sürekli tümleştirme ortamları için önerilir.

### <a name="use-microsoft-storage-explorer"></a>Microsoft Storage Explorer'ı kullanma

1. [Microsoft Azure Depolama Gezgini'ni](https://azure.microsoft.com/features/storage-explorer/)indirin ve yükleyin.
2. Gezgini açın ve sol menüde **Hesap Ekle'yi**seçin. **Azure Depolamasına Bağlan** iletişim kutusu görüntülenir.
3. **Azure Hesabı Ekle'yi** seçin ve ardından **oturum açın.** Azure hesabınızda oturum açabilmek için gerekli adımları tamamlayın.
4. Soldaki**Explorer** bölmesinde, Depolama **Hesaplarınıza** gidin ve bu düğümü genişletin.
5. VHD'nize sağ tıklayın ve ardından **Paylaş Erişim İmzası'nı al'ı**seçin.
6. **Paylaşılan Erişim İmzası** iletişim kutusu görüntülenir. Aşağıdaki alanları tamamlayın:

    * **Başlangıç saati** – VHD erişimi için izin başlangıç tarihi. Geçerli tarihten bir gün önce bir tarih sağlayın.
    * **Son kullanma tarihi** – VHD erişimi için izin son kullanma tarihi. Geçerli tarihten en az üç hafta sonra bir tarih sağlayın.
    * **İzinler** – Okuma ve Liste izinlerini seçin.
    * **Kapsayıcı düzeyinde** – **Oluştur kapsayıcı düzeyinde paylaşılan erişim imzası URI** onay kutusunu denetleyin.

        :::image type="content" source="media/create-sas-uri-storage-explorer.png" alt-text="Paylaşılan Erişim İmzası iletişim kutusunu gösterir":::

7. Bu VHD için ilişkili SAS URI'yi oluşturmak için **Oluştur'u**seçin. İletişim kutusu yenilenir ve bu işlemle ilgili ayrıntıları gösterir.
8. **URI'yi** kopyalayın ve güvenli bir konumda bir metin dosyasına kaydedin.

    :::image type="content" source="media/create-sas-uri-shared-access-signature-details.png" alt-text="Paylaşılan Erişim İmzası ayrıntıları kutusunu gösterir":::

    Bu oluşturulan SAS URI konteyner düzeyinde erişim içindir. Özel yapmak için, VHD adını (sonraki adım) eklemek için metin dosyasını değiştirin.

9. SAS URI'deki vhds dizelerinden sonra VHD adınızı ekleyin (ileri eğik çizgi ekleyin). Son SAS URI şu şekilde görünmelidir:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`Örneğin, VDH adı `TestRGVM2.vhd`ise, o zaman ortaya çıkan SAS URI olacaktır:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

10. Yayınlayacağınız planlarda her VHD için bu adımları yineleyin.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

1. [Microsoft Azure CLI'yi](https://azure.microsoft.com/documentation/articles/xplat-cli-install/)indirin ve yükleyin. Windows, macOS ve Linux'un çeşitli dağıtımları için sürümler mevcuttur.
2. PowerShell dosyası (.ps1 dosya uzantısı) oluşturun, aşağıdaki kodu kopyalayın ve yerel olarak kaydedin.

    ```PowerShell
    az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' --name <vhd-name> --permissions rl --start '<start-date>' --expiry '<expiry-date>'
    ```

3. Aşağıdaki parametre değerlerini kullanmak için dosyayı edin. UtC tarih saati biçiminde tarihler `2020-04-01T00:00:00Z`sağlayın, örneğin.

    * `<account-name>`– Azure depolama hesap adınız
    * `<account-key>`– Azure depolama hesap anahtarınız
    * `<vhd-name>`– VHD adınız
    * `<start-date>`– VHD erişimi için izin başlangıç tarihi. Geçerli tarihten bir gün önce bir tarih sağlayın.
    * `<expiry-date>`– VHD erişimi için izin son kullanma tarihi. Geçerli tarihten en az üç hafta sonra bir tarih sağlayın.

    Bu örnek, uygun parametre değerlerini gösterir (bu yazının yazıldığı sırada):

    ```PowerShell
    az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ONc+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net' --name vhds --permissions rl --start '2020-04-01T00:00:00Z' --expiry '2021-04-01T00:00:00Z'
    ```

4. Değişiklikleri kaydedin.
5. Aşağıdaki yöntemlerden birini kullanarak, kapsayıcı düzeyinde erişim için bir **SAS bağlantı dizesi** oluşturmak için bu komut dosyasını yönetim ayrıcalıklarıyla çalıştırın:

    * Komut dosyasını konsoldan çalıştırın. Windows'da, komut dosyasına sağ tıklayın ve **yönetici olarak Çalıştır'ı**seçin.
    * Komut dosyasını [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/components/ise/introducing-the-windows-powershell-ise)gibi bir PowerShell komut dosyası düzenleyicisinden çalıştırın. Bu ekran, bu düzenleyici içinde bir SAS bağlantı dizesi oluşturulmasını gösterir:

     :::image type="content" source="media/create-sas-uri-power-shell-ise.png" alt-text="Windows PowerShell ISE ile bir SAS bağlantı dizesi oluşturulmasını gösterir":::

6. SAS bağlantı dizesini kopyalayın ve güvenli bir konumda bir metin dosyasına kaydedin. Son SAS URI'yi oluşturmak için VHD konum bilgilerini eklemek için bu dizeyi edin.
7. Azure portalında, yeni URI ile ilişkili VHD'yi içeren blob depolama alanına gidin.
8. Aşağıdaki ekran görüntüsünde gösterildiği gibi **Blob hizmet bitiş noktasının**URL'sini kopyalayın

    :::image type="content" source="media/create-sas-uri-blob-endpoint.png" alt-text="Blob hizmet bitiş noktasını gösterir":::

9. Metin dosyasını 6. Bu biçimi kullanarak tam SAS URI oluşturun:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`

    Örneğin, VHD adı ise, `TestRGVM2.vhd`SAS URI olacaktır:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Yayınlamayı planladığınız SNU'larda her VHD için bu adımları yineleyin.

## <a name="verify-the-sas-uri"></a>SAS URI'yi doğrulayın

Bunu doğrulamak için aşağıdaki denetim listesini kullanarak oluşturulan her SAS URI'yi gözden geçirin:

* URI şuna benzer:`<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`
* URI, ".vhd" dosya adı uzantısı da dahil olmak üzere VHD görüntü dosya adınızı içerir.
* `sp=rl`URI'nizin ortasına yakın görünür. Bu dize `Read` `List` bunu gösterir ve erişim belirtilir.
* Görüntülendiğinde, `sr=c` bu kapsayıcı düzeyinde erişim belirtilir anlamına gelir.
* Lekeyi test indirmek için URI'yi bir tarayıcıya kopyalayıp yapıştırın (indirme işlemi tamamlanmadan işlemi iptal edebilirsiniz).

## <a name="next-step"></a>Sonraki adım

SAS URI oluşturmakta güçlük yaşıyorsanız, [Genel SAS URL sorunlarına](https://docs.microsoft.com/azure/marketplace/partner-center-portal/common-sas-uri-issues)bakın. Aksi takdirde, SAS URI(ler)'i daha sonra kullanmak üzere güvenli bir yere kaydedin. VM teklifinizi Partner Center'da yayınlamak için ihtiyacınız olacak.

* [Azure Sanal Makine teklifi oluşturma](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer)
