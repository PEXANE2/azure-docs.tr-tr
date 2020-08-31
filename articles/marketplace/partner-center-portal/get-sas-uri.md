---
title: VM Görüntünüz için SAS URI 'SI edinme-Azure Marketi
description: Azure Marketi 'nde sanal sabit diskleriniz (VHD) için bir paylaşılan erişim imzası (SAS) URI 'SI oluşturun.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 08/14/2020
ms.openlocfilehash: a84f287c6303e093d68dd462ccc5cecc34b463cd
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89144546"
---
# <a name="get-a-sas-uri-for-your-vm-image"></a>VM Görüntünüz için SAS URI 'SI alın

Yayımlama işlemi sırasında, planlarınızla ilişkili her VHD için bir SAS (paylaşılan erişim Imzası) URI 'SI sağlamanız gerekir (daha önce SKU 'Lar adı verilir). Sertifika işlemi sırasında Microsoft 'un bu VHD 'lere erişmesi gerekir. Bu URI 'yi Iş Ortağı Merkezi 'ndeki **planlar** sekmesinde girersiniz.

VHD 'niz için SAS URI 'Lerinin oluşturulması şu gereksinimlere sahiptir:

- Yalnızca yönetilmeyen VHD 'leri destekler.
- Yalnızca liste ve okuma izinleri gereklidir. Yazma veya silme erişimi sağlamaz.
- SAS URI 'sinin oluşturulduğu erişim süresi (bitiş tarihi), en az üç hafta olmalıdır.
- UTC saat değişikliklerine karşı koruma sağlamak için, başlangıç tarihini geçerli tarihten bir güne ayarlayın. Örneğin, geçerli tarih 16 Haziran 2020, 6/15/2020 ' i seçin.

## <a name="generate-the-sas-address"></a>SAS adresini oluşturma

SAS adresi (URL) oluşturmak için kullanılan iki ortak araç vardır:

1. **Microsoft Depolama Gezgini** : Windows, MacOS ve Linux için kullanılabilen grafik araç.
2. **MICROSOFT Azure CLI** : Windows dışı işletim sistemleri ve otomatikleştirilmiş veya sürekli tümleştirme ortamları için önerilir.

### <a name="using-tool-1-microsoft-storage-explorer"></a>Araç 1 kullanma: Microsoft Depolama Gezgini

1. [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)indirin ve yükleyin.
2. Gezginini açın ve sol menüden **Hesap Ekle**' yi seçin.
3. **Azure depolama 'Ya Bağlan** Iletişim kutusunda **Azure hesabı ekle** ' yi seçin ve Azure hesabınızda oturum açın.
4. Sol gezgin bölmesinde, **depolama hesapları** düğümünü genişletin.
5. VHD 'nize sağ tıklayın ve **paylaşma erişimi Imzasını al**' ı seçin.
6. **Paylaşılan erişim imzası** iletişim kutusunda aşağıdaki alanları doldurun:

    1. Başlangıç zamanı – VHD erişimi için Izin başlangıç tarihi. Geçerli tarihten bir gün önce bir tarih girin.
    2. Süre sonu – VHD erişimi için Izin sona erme tarihi. Geçerli tarihin ötesinde en az üç hafta sonra bir tarih girin.
    3. İzinler – okuma ve listeleme izinlerini seçin.
    4. Kapsayıcı düzeyi – kapsayıcı düzeyinde paylaşılan erişim imzası URI 'SI oluştur onay kutusunu Işaretleyin.

    ![Paylaşılan erişim imzası iletişim kutusu.](media/vm/create-sas-uri-storage-explorer.png)

7. Bu VHD için ilişkili SAS URI 'SI oluşturmak için **Oluştur**' u seçin. İletişim kutusu yenilenir ve bu işlemle ilgili ayrıntıları gösterir.

8. URI 'yi kopyalayın ve güvenli bir konumdaki bir metin dosyasına kaydedin.

    ![URI kopyalanıyor.](media/vm/create-sas-uri-shared-access-signature-details.png)

    Bu oluşturulan SAS URI 'SI kapsayıcı düzeyinde erişime yöneliktir. Özel hale getirmek için, VHD adını eklemek üzere metin dosyasını düzenleyin.

9. SAS URI 'sindeki VHD dizesinden sonra VHD adınızı ekleyin (eğik çizgi ekleyin). Son SAS URI 'SI şöyle görünmelidir:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`

1. Yayımlayacağınız planlardaki her VHD için bu adımları tekrarlayın.

### <a name="using-tool-2-azure-cli"></a>Araç 2 kullanma: Azure CLı

1. [MICROSOFT Azure CL](https://azure.microsoft.com/documentation/articles/xplat-cli-install/)I indirin ve yükleyin. Sürümler Windows, macOS ve çeşitli Linux 'lar için kullanılabilir.
2. Bir PowerShell dosyası (. ps1 dosyası uzantısı) oluşturun, aşağıdaki kodu kopyalayın ve yerel olarak kaydedin.

    ```JSON
    az storage container generate-sas --connection-string ‘DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net’ --name <vhd-name> --permissions rl --start ‘<start-date>’ --expiry ‘<expiry-date>’
    ```

3. Aşağıdaki parametre değerlerini kullanmak için dosyayı düzenleyin. Tarihi UTC-04-01T00:00:00Z gibi UTC Tarih saat biçiminde girin.

    - hesap-adı – Azure depolama hesabınızın adı.
    - hesap-anahtar – Azure depolama hesabı anahtarınız.
    - VHD-adı: VHD adınız.
    - başlangıç-tarih – VHD erişimi için Izin başlangıç tarihi. Geçerli tarihten bir gün önce bir tarih girin.
    - Son Tarih – VHD erişimi için Izin sona erme tarihi. Geçerli tarihten en az üç hafta sonra bir tarih girin.

    Aşağıda, doğru parametre değerleri örneği (Bu yazma sırasında) verilmiştir:

    `az storage container generate-sas --connection-string ‘DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ON c+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net’ --name vhds -- permissions rl --start ‘2020-04-01T00:00:00Z’ --expiry ‘2021-04-01T00:00:00Z’`

1. Değişiklikleri kaydedin.
2. Aşağıdaki yöntemlerden birini kullanarak, kapsayıcı düzeyinde erişim için bir SAS bağlantı dizesi oluşturmak üzere bu betiği yönetici ayrıcalıklarıyla çalıştırın:

    - Betiği konsolundan çalıştırın. Windows 'ta, betiğe sağ tıklayıp **yönetici olarak çalıştır**' ı seçin.
    - Betiği [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/components/ise/introducing-the-windows-powershell-ise)gibi bir PowerShell betik düzenleyicisinden çalıştırın. Bu ekranda, Bu düzenleyicide SAS bağlantı dizesinin oluşturulması gösterilmektedir:

    [![PowerShell Düzenleyicisi içinde SAS bağlantı dizesi oluşturma](media/vm/create-sas-uri-power-shell-ise.png)](media/vm/create-sas-uri-power-shell-ise.png#lightbox)

6. SAS bağlantı dizesini kopyalayın ve güvenli bir konumdaki bir metin dosyasına kaydedin. Son SAS URI 'sini oluşturmak için VHD konum bilgilerini eklemek üzere bu dizeyi düzenleyin.
7. Azure portal, yeni URI ile ilişkili VHD 'YI içeren BLOB depolama alanına gidin.
8. Bblob hizmeti uç noktasının URL 'sini kopyalayın:

    ![Blob hizmeti uç noktasının URL 'SI kopyalanıyor.](media/vm/create-sas-uri-blob-endpoint.png)

9. Metin dosyasını 6. adımdaki SAS bağlantı dizesiyle düzenleyin. Şu biçimi kullanarak tüm SAS URI 'sini oluşturun:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`

## <a name="verify-the-sas-uri"></a>SAS URI 'sini doğrulama

İsteğin SAS URI 'SI gönderme ile ilgili herhangi bir sorunu önlemek için Iş Ortağı Merkezi 'nde yayımlamadan önce SAS URI 'sini denetleyin. Bu işlem isteğe bağlıdır, ancak önerilir.

- URI, dosya adı uzantısı da dahil olmak üzere VHD görüntü dosya adını içerir `.vhd` .
- `Sp=rl` URI 'nizin ortasına yakın görünür. Bu dize okuma ve listeleme erişiminin belirtilir.
- `sr=c`Göründüğünde, bu, kapsayıcı düzeyi erişimin belirtildiği anlamına gelir.
- Blob 'u test etmek için bir tarayıcıya kopyalayıp yapıştırın (indirme tamamlanmadan önce işlemi iptal edebilirsiniz).

## <a name="next-step"></a>Sonraki adım

- [Azure sanal makine oluşturma teklifini](azure-vm-create-offer.md)okuyun.
