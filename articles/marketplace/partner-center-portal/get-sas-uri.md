---
title: VM görüntüleri için paylaşılan erişim imzası URI 'SI-Azure Marketi
description: Azure Marketi 'nde sanal sabit diskleriniz (VHD) için bir paylaşılan erişim imzası (SAS) URI 'SI oluşturun.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 07/29/2020
ms.openlocfilehash: 2bc129fc37347bd108ad62409490c5ce31b7728f
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87538940"
---
# <a name="get-shared-access-signature-uri-for-your-vm-image"></a>VM Görüntünüz için paylaşılan erişim imzası URI 'SI alın

Bu makalede, her bir sanal sabit disk (VHD) için paylaşılan erişim imzası (SAS) Tekdüzen Kaynak tanımlayıcısı (URI) oluşturma açıklanır.

Yayımlama işlemi sırasında, planlarınızla ilişkili her VHD için bir URI sağlamanız gerekir (daha önce SKU 'Lar adı verilir). Sertifika işlemi sırasında Microsoft 'un bu VHD 'lere erişmesi gerekir. Bu URI 'yi Iş Ortağı Merkezi 'ndeki **planlar** sekmesinde girersiniz.

VHD 'niz için SAS URI 'Leri oluştururken şu gereksinimleri izleyin:

* Yalnızca yönetilmeyen VHD 'ler desteklenir.
* Yalnızca `List` ve `Read` izinleri gereklidir. Yazma veya silme erişimi sağlamaz.
* SAS URI 'sinin oluşturulduğu erişim süresi (bitiş tarihi), en az üç hafta olmalıdır.
* UTC saat değişikliklerine karşı koruma sağlamak için, başlangıç tarihini geçerli tarihten bir güne ayarlayın. Örneğin, geçerli tarih 6 Ekim 2019 ise 10/5/2019 ' i seçin.

## <a name="generate-the-sas-address"></a>SAS adresini oluşturma

SAS adresi (URL) oluşturmak için kullanılan iki ortak araç vardır:

* **Microsoft Azure Depolama Gezgini** -grafik araç Azure Portal kullanılabilir.
* **MICROSOFT Azure CLI** : Windows dışı işletim sistemleri ve otomatikleştirilmiş veya sürekli tümleştirme ortamları için önerilir.

### <a name="use-microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini kullan

1. Azure portal depolama hesabınıza gidin.
2. Soldaki gezgin bölmesinde **Depolama Gezgini** (Önizleme) aracını açın.
3. VHD 'nize sağ tıklayın ve **paylaşılan erişim Imzası al**' ı seçin.
4. **Paylaşılan erişim imzası** iletişim kutusu görüntülenir. Aşağıdaki alanları doldurun:

    * **Başlangıç zamanı** – VHD erişimi için izin başlangıç tarihi. Geçerli tarihten bir gün önce bir tarih girin.
    * **Süre sonu** – VHD erişimi için izin sona erme tarihi. Geçerli tarihin ötesinde en az üç haftalık bir tarih girin.
    * **İzinler** – okuma ve listeleme izinlerini seçin.
    * **Kapsayıcı düzeyi** – **kapsayıcı düzeyinde paylaşılan erişim Imzası URI 'si oluştur** onay kutusunu işaretleyin.

        :::image type="content" source="media/create-sas-uri-storage-explorer.png" alt-text="Paylaşılan erişim Imzası iletişim kutusunu gösterir":::

5. Bu VHD için ilişkili SAS URI 'SI oluşturmak için **Oluştur**' u seçin. İletişim kutusu yenilenir ve bu işlemle ilgili ayrıntıları gösterir.
6. **URI** 'yi kopyalayın ve güvenli bir konumdaki bir metin dosyasına kaydedin.

    :::image type="content" source="media/create-sas-uri-shared-access-signature-details.png" alt-text="Paylaşılan erişim Imzası ayrıntıları kutusunu gösterir":::
7. Yayımlayacağınız planlardaki her VHD için bu adımları tekrarlayın.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

1. [MICROSOFT Azure CLI](https://azure.microsoft.com/documentation/articles/xplat-cli-install/)indirin ve yükleyin. Sürümler Windows, macOS ve çeşitli Linux 'lar için kullanılabilir.
2. Bir PowerShell dosyası (. ps1 dosyası uzantısı) oluşturun, aşağıdaki kodu kopyalayın ve yerel olarak kaydedin.

    ```PowerShell
    az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' --name <vhd-name> --permissions rl --start '<start-date>' --expiry '<expiry-date>'
    ```

3. Aşağıdaki parametre değerlerini kullanmak için dosyayı düzenleyin. Tarihleri UTC Tarih saat biçiminde girin, örneğin `2020-04-01T00:00:00Z` .

    * `<account-name>`– Azure depolama hesabınızın adı
    * `<account-key>`– Azure depolama hesabı anahtarınız
    * `<vhd-name>`– VHD adınız
    * `<start-date>`– VHD erişimi için izin başlangıç tarihi. Geçerli tarihten bir gün önce bir tarih girin.
    * `<expiry-date>`– VHD erişimi için izin sona erme tarihi. Geçerli tarihten en az üç hafta sonra bir tarih girin.

    Bu örnek, doğru parametre değerlerini gösterir (Bu yazma sırasında):

    ```PowerShell
    az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ONc+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net' --name vhds --permissions rl --start '2020-04-01T00:00:00Z' --expiry '2021-04-01T00:00:00Z'
    ```

4. Değişiklikleri kaydedin.
5. Aşağıdaki yöntemlerden birini kullanarak, kapsayıcı düzeyinde erişim için bir **SAS bağlantı dizesi** oluşturmak üzere bu betiği yönetici ayrıcalıklarıyla çalıştırın:

    * Betiği konsolundan çalıştırın. Windows 'ta, betiğe sağ tıklayıp **yönetici olarak çalıştır**' ı seçin.
    * Betiği [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/components/ise/introducing-the-windows-powershell-ise)gibi bir PowerShell betik düzenleyicisinden çalıştırın. Bu ekranda, Bu düzenleyicide SAS bağlantı dizesinin oluşturulması gösterilmektedir:

     :::image type="content" source="media/create-sas-uri-power-shell-ise.png" alt-text="Windows PowerShell ISE bir SAS bağlantı dizesinin oluşturulmasını gösterir":::

6. SAS bağlantı dizesini kopyalayın ve güvenli bir konumdaki bir metin dosyasına kaydedin. Son SAS URI 'sini oluşturmak için VHD konum bilgilerini eklemek üzere bu dizeyi düzenleyin.
7. Azure portal, yeni URI ile ilişkili VHD 'YI içeren BLOB depolama alanına gidin.
8. **BLOB hizmeti uç NOKTASıNıN**URL 'sini aşağıdaki ekran görüntüsünde gösterildiği gibi kopyalayın

    :::image type="content" source="media/create-sas-uri-blob-endpoint.png" alt-text="Blob hizmeti uç noktasını gösterir":::

9. Metin dosyasını 6. adımdaki SAS bağlantı dizesiyle düzenleyin. Şu biçimi kullanarak tüm SAS URI 'sini oluşturun:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`

    Örneğin, VHD 'nin adı ise `TestRGVM2.vhd` SAS URI 'si şu şekilde olur:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Yayımlayacağınız planlardaki her VHD için bu adımları tekrarlayın.

## <a name="verify-the-sas-uri"></a>SAS URI 'sini doğrulama

Aşağıdakileri doğrulamak için aşağıdaki denetim listesini kullanarak oluşturulan her SAS URI 'sini gözden geçirin:

* URI şöyle görünür:`<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`
* URI, ". vhd" dosya adı uzantısı da dahil olmak üzere VHD görüntü dosya adını içerir.
* `sp=rl`URI 'nizin ortasına yakın görünür. Bu dize, `Read` ve `List` erişiminin belirtilme olduğunu gösterir.
* `sr=c`Göründüğünde, bu, kapsayıcı düzeyi erişimin belirtildiği anlamına gelir.
* Blob 'u test etmek için bir tarayıcıya kopyalayıp yapıştırın (indirme tamamlanmadan önce işlemi iptal edebilirsiniz).

## <a name="next-step"></a>Sonraki adım

SAS URI 'SI oluşturma zorluklarına sahipseniz bkz. [genel SAS URL 'si sorunları](common-sas-uri-issues.md). Aksi takdirde, SAS URI 'leri daha sonra kullanmak üzere güvenli bir konuma kaydedin. VM teklifinizi Iş Ortağı Merkezi 'nde yayımlamak için ihtiyacınız olacak.

* [Azure sanal makine teklifi oluşturma](azure-vm-create-offer.md)
