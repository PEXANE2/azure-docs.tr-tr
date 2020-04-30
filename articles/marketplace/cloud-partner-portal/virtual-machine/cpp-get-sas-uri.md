---
title: Microsoft Azure tabanlı VM Görüntünüz için paylaşılan erişim imzası URI 'SI alın | Azure Marketi
description: VM Görüntünüz için paylaşılan erişim imzası (SAS) URI 'sinin nasıl alınacağını açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: dsindona
ms.openlocfilehash: 46b3764fe2a1e09875f2b92a461591b6ff08540f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82147897"
---
# <a name="get-shared-access-signature-uri-for-your-vm-image"></a>VM Görüntünüz için paylaşılan erişim imzası URI 'SI alın

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [VM Görüntünüz için paylaşılan erişim imzası URI 'Si al](https://docs.microsoft.com/azure/marketplace/partner-center-portal/common-sas-uri-issues) bölümündeki yönergeleri izleyin.

Yayımlama işlemi sırasında, SKU 'larınız ile ilişkili her bir sanal sabit disk (VHD) için bir Tekdüzen Kaynak tanımlayıcısı (URI) sağlamanız gerekir. Sertifika işlemi sırasında Microsoft 'un bu VHD 'lere erişmesi gerekir. Bu makalede, her VHD için paylaşılan erişim imzası (SAS) URI 'SI oluşturma açıklanmaktadır. Bu URI 'yi Bulut İş Ortağı Portalı **SKU 'ları** sekmesine girersiniz.

VHD 'niz için SAS URI 'Leri oluştururken aşağıdaki gereksinimlere uyar:

- Yalnızca yönetilmeyen VHD 'ler desteklenir.
- `List`ve `Read` izinler yeterlidir. Sağlama *not* `Write` veya `Delete` erişim.
- SAS URI 'sinin oluşturulduğu erişim süresi (*bitiş tarihi*), en az üç hafta olmalıdır.
- UTC saat çeşitlemelerine karşı korumak için, başlangıç tarihini geçerli tarihten bir güne ayarlayın. Örneğin, geçerli tarih 6 Ekim 2014 ise 10/5/2014 ' i seçin.

## <a name="generate-the-sas-url"></a>SAS URL 'sini oluşturma

SAS URL 'SI aşağıdaki araçları kullanarak iki ortak şekilde oluşturulabilir:

- Microsoft Depolama Gezgini-grafik aracı Windows, macOS ve Linux için kullanılabilir
- Microsoft Azure CLı-Windows olmayan OSs ve otomatikleştirilmiş veya sürekli tümleştirme ortamları için önerilir

### <a name="azure-cli"></a>Azure CLI

Azure CLı ile SAS URI 'SI oluşturmak için aşağıdaki adımları kullanın.

1. [MICROSOFT Azure CLI](https://azure.microsoft.com/documentation/articles/xplat-cli-install/)'yi indirin ve yükleyin. Sürümler Windows, macOS ve çeşitli Linux 'lar için kullanılabilir.
2. Bir PowerShell dosyası (`.ps1` dosya uzantısı) oluşturun, aşağıdaki kodu kopyalayın ve yerel olarak kaydedin.

   ```azurecli-interactive
   az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' --name <vhd-name> --permissions rl --start '<start-date>' --expiry '<expiry-date>'
   ```

3. Aşağıdaki parametre değerlerini sağlamak için dosyayı düzenleyin.  Örneğin `2016-10-25T00:00:00Z`, UTC Tarih saat biçiminde tarih sağlanmalıdır.
   - `<account-name>`-Azure depolama hesabınızın adı
   - `<account-key>`-Azure depolama hesabı anahtarınız
   - `<vhd-name>`-VHD adınız
   - `<start-date>`-VHD erişimi için izin başlangıç tarihi. Geçerli tarihten bir gün önce bir tarih girin.
   - `<expiry-date>`-VHD erişimi için izin sona erme tarihi.  Geçerli tarihin ötesinde en az üç hafta sonra bir tarih girin.

   Aşağıdaki örnek, doğru parametre değerlerini gösterir (Bu yazma sırasında).

   ```azurecli-interactive
   az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ONc+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net' --name vhds --permissions rl --start '2017-11-06T00:00:00Z' --expiry '2018-08-20T00:00:00Z'
   ```

4. Bu PowerShell betiğine yapılan değişiklikleri kaydedin.
5. Kapsayıcı düzeyinde erişim için bir *SAS bağlantı dizesi* oluşturmak üzere yönetim ayrıcalıklarını kullanarak bu betiği çalıştırın.  İki temel yaklaşımdan yararlanabilirsiniz:
   - Betiği konsolundan çalıştırın.  Örneğin, Windows 'ta betiğe yazın ve **yönetici olarak çalıştır**' ı seçin.
   - Betiği, yönetici ayrıcalıklarını kullanarak [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/components/ise/introducing-the-windows-powershell-ise)gibi bir PowerShell betik düzenleyicisinden çalıştırın.
     Aşağıda, bu Düzenleyici içinde oluşturulan SAS bağlantı dizesi gösterilmektedir.

     ![PowerShell ıSE 'de SAS URI 'SI oluşturma](./media/publishvm_032.png)

6. Elde edilen SAS bağlantı dizesini kopyalayın ve güvenli bir konumdaki bir metin dosyasına kaydedin.  Bu dizeyi, ilişkili VHD konum bilgilerini eklemek için, son SAS URI 'sini oluşturmak için düzenleyin.
7. Azure portal, yeni oluşturulan URI ile ilişkili VHD 'YI içeren BLOB depolama alanına gidin.
8. **BLOB hizmeti uç NOKTASıNıN**URL değerini aşağıda gösterildiği gibi kopyalayın.

    ![Azure portal blob hizmeti uç noktası](./media/publishvm_033.png)

9. Metin dosyasını 6. adımdaki SAS bağlantı dizesiyle düzenleyin.  Aşağıdaki biçimi kullanarak, tüm SAS URI 'sini oluşturacaksınız:

    `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`

    Örneğin, VDH 'nin adı ise `TestRGVM2.vhd`, elde edilen SAS URI 'si şöyle olacaktır:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Yayımlamayı planladığınız SKU 'lardaki her VHD için bu adımları tekrarlayın.

### <a name="microsoft-storage-explorer"></a>Microsoft Depolama Gezgini

Microsoft Azure Depolama Gezgini bir SAS URI 'SI oluşturmak için aşağıdaki adımları kullanın.

1. [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)'ni indirip yükleme.
2. Gezginini açın ve sol taraftaki menü çubuğunda **Hesap Ekle** simgesine tıklayın.  **Azure depolama 'Ya Bağlan** iletişim kutusu görüntülenir.
3. **Azure hesabı ekle** ' yi seçin ve **oturum aç**' a tıklayın.  Azure hesabınızda oturum açmak için gerekli adımları devam edin.
4. Sol **Gezgin** bölmesinde, **depolama hesaplarınıza** gidin ve bu düğümü genişletin.
5. VHD 'nize sağ tıklayın ve bağlam menüsünden **paylaşma erişim Imzasını al** ' ı seçin.

    ![Azure Explorer 'da SAS öğesi al](./media/publishvm_034.png)

6. **Paylaşılan erişim imzası** iletişim kutusu görüntülenir. Aşağıdaki alanlar için değerler girin:
   - VHD erişimi için **Başlangıç zamanı** -izin başlangıç tarihi. Geçerli tarihten bir gün önce bir tarih girin.
   - VHD erişimi için süre sonu **zamanı** -izin sona erme tarihi.  Geçerli tarihin ötesinde en az üç hafta sonra bir tarih girin.
   - **İzinler** - `Read` ve `List` izinlerini seçin.

     ![Azure Explorer 'da SAS iletişim kutusu](./media/publishvm_035.png)

7. Bu VHD için ilişkili SAS URI 'sini oluşturmak için **Oluştur** ' a tıklayın.  İletişim kutusu artık bu işlemle ilgili ayrıntıları görüntüler.
8. **URL** değerini kopyalayın ve güvenli bir konumdaki bir metin dosyasına kaydedin.

    ![Azure Explorer 'da SAS URI 'SI oluşturma](./media/publishvm_036.png)

    Bu oluşturulan SAS URL 'SI kapsayıcı düzeyinde erişime yöneliktir.  Özel hale getirmek için ilişkili VHD adı buna eklenmelidir.

9. Metin dosyasını düzenleyin. SAS URL 'sindeki `vhds` DIZEDEN sonra VHD adınızı ekleyin (baştaki eğik çizgi ekleyin).  Son SAS URI 'SI şu biçimde olmalıdır:

    `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`

    Örneğin, VDH 'nin adı ise `TestRGVM2.vhd`, elde edilen SAS URI 'si şöyle olacaktır:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Yayımlamayı planladığınız SKU 'lardaki her VHD için bu adımları tekrarlayın.

## <a name="verify-the-sas-uri"></a>SAS URI 'sini doğrulama

Aşağıdaki denetim listesini kullanarak oluşturulan her SAS URI 'sini gözden geçirin ve doğrulayın.  Şunları doğrulayın:

- `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` URI şu biçimdedir: +`<sas-connection-string>`
- URI, ". vhd" dosya adı uzantısı da dahil olmak üzere VHD görüntü dosya adını içerir.
- URI 'nin ortasına doğru bir şekilde `sp=rl` görünür. Bu dize, ve `Read` `List` erişiminin belirtildiğini gösterir.
- Bu noktadan sonra `sr=c` da görüntülenir. Bu dize, kapsayıcı düzeyi erişimin belirtildiğini gösterir.
- İlişkili blobu indirmeye başlamak için URI 'yi kopyalayıp tarayıcıya yapıştırın.  (İndirme tamamlanmadan önce işlemi iptal edebilirsiniz.)

## <a name="next-steps"></a>Sonraki adımlar

SAS URI 'SI oluşturma konusunda zorluklarla karşılaşıyorsanız bkz. [genel SAS URL 'si sorunları](./cpp-common-sas-url-issues.md).  Aksi takdirde, SAS URI 'leri daha sonra kullanmak üzere güvenli bir konuma kaydedin. Bulut İş Ortağı Portalı [sanal makine teklifinizi yayımlamanız](./cpp-publish-offer.md) gerekecektir.
