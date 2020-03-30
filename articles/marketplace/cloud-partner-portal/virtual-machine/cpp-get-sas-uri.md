---
title: Microsoft Azure tabanlı VM resminiz için paylaşılan erişim imzası URI'yi alın | Azure Marketi
description: VM görüntünüz için paylaşılan erişim imzası (SAS) URI'yi nasıl alacağınızı açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: dsindona
ms.openlocfilehash: 6fe15fb18d8865911363a4696e44dd7fe1d90c09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80277812"
---
# <a name="get-shared-access-signature-uri-for-your-vm-image"></a>VM resminiz için paylaşılan erişim imzası URI'yi alın

Yayımlama işlemi sırasında, SUK'larınizle ilişkili her sanal sabit disk (VHD) için tek tip bir kaynak tanımlayıcısı (URI) sağlamanız gerekir. Microsoft'un sertifika işlemi sırasında bu VHD'lere erişmesi gerekir. Bu makalede, her VHD için paylaşılan erişim imzası (SAS) URI nasıl oluşturacağı açıklanmaktadır. Bu URI'yi Bulut İş Ortağı Portalı'ndaki **SKUs** sekmesine gireceksiniz.

VHD'leriniz için SAS URI'leri oluştururken aşağıdaki gereksinimlere uyun:

- Yalnızca yönetilmeyen VHD'ler desteklenir.
- `List`ve `Read` izinler yeterlidir. *Sağlamayın* `Write` `Delete` veya erişin.
- Erişim süresi *(son kullanma tarihi)* SAS URI oluşturulduğundan itibaren en az üç hafta olmalıdır.
- UTC saat değişimlerine karşı korunmak için başlangıç tarihini geçerli tarihten bir gün öncesine ayarlayın. Örneğin, geçerli tarih 6 Ekim 2014 ise, 10/5/2014'ü seçin.

## <a name="generate-the-sas-url"></a>SAS URL'sini oluşturma

SAS URL'si aşağıdaki araçları kullanarak iki yaygın şekilde oluşturulabilir:

- Microsoft Storage Explorer - Windows, macOS ve Linux için kullanılabilen grafik selami
- Microsoft Azure CLI - Windows'a ait olmayan işletim sistemi ve otomatik veya sürekli tümleştirme ortamları için önerilir

### <a name="azure-cli"></a>Azure CLI

Azure CLI ile bir SAS URI oluşturmak için aşağıdaki adımları kullanın.

1. Microsoft Azure [CLI'yi](https://azure.microsoft.com/documentation/articles/xplat-cli-install/)indirin ve yükleyin. Windows, macOS ve Linux'un çeşitli dağıtımları için sürümler mevcuttur.
2. PowerShell dosyası (dosya`.ps1` uzantısı) oluşturun, aşağıdaki kodu kopyalayın ve yerel olarak kaydedin.

   ```azurecli-interactive
   az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' --name <vhd-name> --permissions rl --start '<start-date>' --expiry '<expiry-date>'
   ```

3. Aşağıdaki parametre değerlerini sağlamak için dosyayı edin.  Tarihler UTC tarih saati biçiminde sağlanmalıdır, örneğin. `2016-10-25T00:00:00Z`
   - `<account-name>`- Azure depolama hesap adınız
   - `<account-key>`- Azure depolama hesap anahtarınız
   - `<vhd-name>`- VHD adınız
   - `<start-date>`- VHD erişimi için izin başlangıç tarihi. Geçerli tarihten bir gün önce bir tarih sağlayın.
   - `<expiry-date>`- VHD erişimi için izin son kullanma tarihi.  Geçerli tarihten en az üç hafta sonra bir tarih sağlayın.

   Aşağıdaki örnekte uygun parametre değerleri (bu yazının yazıldığı sırada) gösterilmektedir.

   ```azurecli-interactive
   az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ONc+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net' --name vhds --permissions rl --start '2017-11-06T00:00:00Z' --expiry '2018-08-20T00:00:00Z'
   ```

4. Değişiklikleri bu PowerShell komut dosyasına kaydedin.
5. Kapsayıcı düzeyi erişimi için bir *SAS bağlantı dizesi* oluşturmak için yönetim ayrıcalıklarını kullanarak bu komut dosyasını çalıştırın.  İki temel yaklaşım kullanabilirsiniz:
   - Komut dosyasını konsoldan çalıştırın.  Örneğin, Windows'da komut dosyasına yazma-tıklayın ve **yönetici olarak çalıştır'ı**seçin.
   - Komut dosyasını, yönetim ayrıcalıklarını kullanarak [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/components/ise/introducing-the-windows-powershell-ise)gibi bir PowerShell komut dosyası düzenleyicisinden çalıştırın.
     Aşağıda, bu düzenleyici içinde oluşturulan bir SAS bağlantı dizesi gösterilmektedir.

     ![PowerShell ISE'de SAS URI üretimi](./media/publishvm_032.png)

6. Ortaya çıkan SAS bağlantı dizesini kopyalayın ve güvenli bir konumda bir metin dosyasına kaydedin.  Son SAS URI'yi oluşturmak için ilişkili VHD konum bilgilerini eklemek için bu dizeyi düzenlenecektir.
7. Azure portalında, yeni oluşturulan URI ile ilişkili VHD içeren blob depolama alanına gidin.
8. Aşağıda gösterildiği gibi **Blob hizmet bitiş noktasının**URL değerini kopyalayın.

    ![Azure portalında Blob hizmet bitiş noktası](./media/publishvm_033.png)

9. Metin dosyasını 6.  Aşağıdaki biçimi kullanarak tam SAS URI oluşturacaktır:

    `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`

    Örneğin, VDH adı `TestRGVM2.vhd`ise, o zaman ortaya çıkan SAS URI olacaktır:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Yayınlamayı planladığınız SNU'larda her VHD için bu adımları yineleyin.

### <a name="microsoft-storage-explorer"></a>Microsoft Depolama Gezgini

Microsoft Azure Depolama Gezgini ile bir SAS URI oluşturmak için aşağıdaki adımları kullanın.

1. [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)'ni indirip yükleme.
2. Gezgini açın ve sol menü çubuğunda Hesap **Ekle** simgesine tıklayın.  **Azure Depolamasına Bağlan** iletişim kutusu görüntülenir.
3. **Azure Hesabı Ekle'yi** seçin ve **Oturum Aç'ı**tıklatın.  Azure hesabınızda oturum açabilmek için gerekli adımları sürdürün.
4. Sol daki **Explorer** bölmesinde, Depolama **Hesaplarınıza** gidin ve bu düğümü genişletin.
5. VHD'nize sağ tıklayın ve bağlam menüsünden **Paylaş Erişimini Al'ı** seçin.

    ![Azure Gezgini'nde SAS öğesi alma](./media/publishvm_034.png)

6. **Paylaşılan Erişim İmzası** iletişim kutusu görüntülenir. Aşağıdaki alanlar için değerleri girin:
   - **Başlangıç saati** - VHD erişimi için izin başlangıç tarihi. Geçerli tarihten bir gün önce bir tarih sağlayın.
   - **Son kullanma tarihi** - VHD erişimi için izin son kullanma tarihi.  Geçerli tarihten en az üç hafta sonra bir tarih sağlayın.
   - **İzinler** - `Read` İzinleri ve `List` izinleri seçin.

     ![Azure Gezgini'nde SAS iletişim kutusu](./media/publishvm_035.png)

7. Bu VHD için ilişkili SAS URI'yi oluşturmak için **Oluştur'u** tıklatın.  İletişim şimdi bu işlemle ilgili ayrıntıları görüntüler.
8. **URL** değerini kopyalayın ve güvenli bir konumda bir metin dosyasına kaydedin.

    ![Azure Explorer'da SAS URI oluşturma](./media/publishvm_036.png)

    Oluşturulan bu SAS URL, kapsayıcı düzeyinde erişim içindir.  Özel yapmak için, ilişkili VHD adı eklenmelidir.

9. Metin dosyasını edin. SAS URL'deki dizeden `vhds` sonra VHD adınızı ekleyin (önde gelen bir ileri eğik çizgi ekleyin).  Son SAS URI biçimi olmalıdır:

    `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`

    Örneğin, VDH adı `TestRGVM2.vhd`ise, o zaman ortaya çıkan SAS URI olacaktır:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Yayınlamayı planladığınız SNU'larda her VHD için bu adımları yineleyin.

## <a name="verify-the-sas-uri"></a>SAS URI'yi doğrulayın

Oluşturulan her SAS URI'yi aşağıdaki denetim listesini kullanarak gözden geçirin ve doğrulayın.  Bunu doğrulayın:

- URI şeklindedir: `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` +`<sas-connection-string>`
- URI, ".vhd" dosya adı uzantısı da dahil olmak üzere VHD görüntü dosya adınızı içerir.
- URI'nin ortasına `sp=rl` doğru, görünür. Bu dize `Read` `List` bunu gösterir ve erişim belirtilir.
- Bu noktadan `sr=c` sonra, aynı zamanda görünür. Bu dize, kapsayıcı düzeyinde erişimin belirtildiğini gösterir.
- Uri'yi kopyalayıp yapıştırarak ilişkili blob'u indirmeye başlayın.  (İndirme tamamlanmadan işlemi iptal edebilirsiniz.)

## <a name="next-steps"></a>Sonraki adımlar

Bir SAS URI oluştururken güçlük yaşıyorsanız, genel [SAS URL sorunlarına](./cpp-common-sas-url-issues.md)bakın.  Aksi takdirde, SAS URI(ler)'i daha sonra kullanmak üzere güvenli bir yere kaydedin. [VM teklifinizi](./cpp-publish-offer.md) Bulut İş Ortağı Portalı'nda yayınlamanız gerekecektir.
