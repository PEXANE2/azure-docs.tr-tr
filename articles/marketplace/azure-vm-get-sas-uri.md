---
title: VM görüntüsü için SAS URI 'SI oluşturma-Azure Marketi
description: Azure Marketi 'nde bir sanal sabit disk (VHD) için paylaşılan erişim imzası (SAS) URI 'SI oluşturun.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: iqshahmicrosoft
ms.author: krsh
ms.date: 10/19/2020
ms.openlocfilehash: b927e90c60110d6922649b75b6549528a2a9a40f
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92284772"
---
# <a name="how-to-generate-a-sas-uri-for-a-vm-image"></a>VM görüntüsü için SAS URI 'SI oluşturma

Yayımlama işlemi sırasında, planlarınızla ilişkili her VHD için bir SAS (paylaşılan erişim Imzası) URI 'SI sağlamanız gerekir (daha önce SKU 'Lar adı verilir). Sertifika işlemi sırasında Microsoft 'un bu VHD 'lere erişmesi gerekir. Bu URI 'yi Iş Ortağı Merkezi 'ndeki **planlar** sekmesinde girersiniz.

VHD 'niz için SAS URI 'Lerinin oluşturulması şu gereksinimlere sahiptir:

- Yalnızca yönetilmeyen VHD 'leri destekler.
- Yalnızca liste ve okuma izinleri gereklidir. Yazma veya silme erişimi sağlamaz.
- SAS URI 'sinin oluşturulduğu erişim süresi (bitiş tarihi), en az üç hafta olmalıdır.
- UTC saat değişikliklerine karşı koruma sağlamak için, başlangıç tarihini geçerli tarihten bir güne ayarlayın. Örneğin, geçerli tarih 16 Haziran 2020, 6/15/2020 ' i seçin.

## <a name="generate-the-sas-address"></a>SAS adresini oluşturma

SAS adresi (URL) oluşturmak için kullanılan iki ortak araç vardır:

1. **Azure Depolama Gezgini** : Azure Portal kullanılabilir.
2. **Azure CLI** : Windows dışı işletim sistemleri ve otomatikleştirilmiş veya sürekli tümleştirme ortamları için önerilir.

### <a name="using-tool-1-azure-storage-explorer"></a>Araç 1 kullanma: Azure Depolama Gezgini

1. **Depolama hesabınıza**gidin.
1. **Depolama Gezgini**açın.

    :::image type="content" source="media/create-vm/storge-account-explorer.png" alt-text="Depolama hesabı penceresi.":::

3. **Kapsayıcıda**, VHD dosyasına sağ tıklayın ve **paylaşma erişimi imzasını al**' ı seçin.
4. **Paylaşılan erişim imzası** iletişim kutusunda aşağıdaki alanları doldurun:

    1. Başlangıç zamanı – VHD erişimi için Izin başlangıç tarihi. Geçerli tarihten bir gün önce bir tarih girin.
    2. Süre sonu – VHD erişimi için Izin sona erme tarihi. Geçerli tarihin ötesinde en az üç hafta sonra bir tarih girin.
    3. İzinler – okuma ve listeleme izinlerini seçin.
    4. Kapsayıcı düzeyi – kapsayıcı düzeyinde paylaşılan erişim imzası URI 'SI oluştur onay kutusunu Işaretleyin.

    ![Paylaşılan erişim imzası iletişim kutusu.](media/vm/create-sas-uri-storage-explorer.png)

5. Bu VHD için ilişkili SAS URI 'SI oluşturmak için **Oluştur**' u seçin.
6. URI 'yi kopyalayın ve güvenli bir konumdaki bir metin dosyasına kaydedin. Bu oluşturulan SAS URI 'SI kapsayıcı düzeyinde erişime yöneliktir. Özel hale getirmek için, VHD adını eklemek üzere metin dosyasını düzenleyin.
7. SAS URI 'sindeki VHD dizesinden sonra VHD adınızı ekleyin (eğik çizgi ekleyin). Son SAS URI 'SI şöyle görünmelidir:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`

8. Yayımlayacağınız planlardaki her VHD için bu adımları tekrarlayın.

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

## <a name="next-steps"></a>Sonraki adımlar

- Sorunlarla karşılaşırsanız, bkz. [VM SAS hata iletileri](azure-vm-sas-failure-messages.md).
- [Iş Ortağı Merkezi 'nde oturum açın](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership)
- [Azure Marketi 'nde sanal makine teklifi oluşturma](azure-vm-create.md)
