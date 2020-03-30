---
title: REST arabirimini kullanmak için Azure Veri Kutusu sorun giderme| Microsoft Dokümanlar
description: Veri kopyalama, REST arabirimi üzerinden yken Azure Veri Kutusu'nda görülen sorunları nasıl gidereceğinizi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: 7fe5afbc4984c430cbf393e4e2b44122bdd43983
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297135"
---
# <a name="troubleshoot-issues-related-to-azure-data-box-blob-storage"></a>Azure Veri Kutusu Blob depolama yla ilgili sorun giderme sorunları

Bu makalede, verileri kopyalamak için Veri Kutusu'ndaki REST arabirimi üzerinden Veri Kutusu Blob depolamasını kullanırken görebileceğiniz sorunları nasıl giderebileceğiniz hakkında ayrıntılar. Bu sorunlar, Python için Azure Depolama Gezgini, AzCopy veya Azure Depolama kitaplığı gibi diğer uygulamalarla veya istemci kitaplıklarıyla Birlikte Veri Kutusu Blob depolama alanını kullanırken ortaya çıkar.

## <a name="errors-seen-in-azure-storage-explorer"></a>Azure Depolama Gezgini'nde görülen hatalar

Bu bölümde, Veri Kutusu Blob depolama alanı ile Azure Depolama Gezgini kullanılırken karşılaşılan bazı sorunlar ayrıntıları.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|Alt kaynaklar alınamıyor. HTTP üstbilgilerinden birinin değeri doğru biçimde değildir.|**Edit** menüsünden **Hedef Azure Yığını API'lerini**seçin. <br>Azure Depolama Gezgini'ni yeniden başlatın.|
|`getaddrinfo ENOTFOUND <accountname>.blob.<serialnumber>.microsoftdatabox.com` |Bu yolda ana `<accountname>.blob.<serialnumber>.microsoftdatabox.com` bilgisayar dosyasına uç nokta adının eklenmediğini denetleyin: <li>`C:\Windows\System32\drivers\etc\hosts`Windows'da veya </li><li> `/etc/hosts`Linux üzerinde.</li>|
|Alt kaynaklar alınamıyor. <br>Ayrıntılar: kendi imzalı sertifika |Cihazınızın TLS/SSL sertifikasını Azure Depolama Gezgini'ne taşıyın: <li>Sertifikayı Azure portalından indirin. Daha fazla bilgi için [sertifikayı karşıdan yükleyin.](data-box-deploy-copy-data-via-rest.md#download-certificate)</li><li>**Edit** menüsünden **SSL Sertifikalarını** seçin ve ardından **Sertifikaları İçe Aktar'ı**seçin.</li>|

## <a name="errors-seen-in-azcopy-for-windows"></a>Windows için AzCopy'de görülen hatalar

Bu bölümde, Veri Kutusu Blob depolama alanı ile Windows için AzCopy kullanırken karşılaşılan bazı sorunlar ayrıntıları.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|AzCopy komutu bu hatayı görüntülemeden önce bir dakika asılı görünüyor: <br>Https:// dizin leri sayısala uğratmakta başarısız oldu... Uzak ad çözülemedi`<accountname>.blob.<serialnumber>.microsoftdatabox.com`|Son nokta adının `<accountname>.blob.<serialnumber>.microsoftdatabox.com` ana bilgisayar dosyasına şu `C:\Windows\System32\drivers\etc\hosts`anda eklenmediğini denetleyin: .|
|AzCopy komutu bu hatayı görüntülemeden önce bir dakika asılı görünüyor: <br>Kaynak konumunu ayrışdırma hatası. Altta yatan bağlantı kapatıldı: SSL/TLS güvenli kanal için güven ilişkisi kurulamadı.|Cihazınızın TLS/SSL sertifikasını sistemin sertifika deposuna aktarın. Daha fazla bilgi için [sertifikayı karşıdan yükleyin.](data-box-deploy-copy-data-via-rest.md#download-certificate)|


## <a name="errors-seen-in-azcopy-for-linux"></a>Linux için AzCopy'de görülen hatalar

Bu bölümde, Veri Kutusu Blob depolama ile Linux için AzCopy kullanırken karşılaşılan bazı sorunlar ayrıntıları.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|AzCopy komutu bu hatayı görüntülemeden önce 20 dakika asılı görünüyor: <br>Hata ayrışdırma `https://<accountname>.blob.<serialnumber>.microsoftdatabox.com/<cntnr>`kaynak konumu . Böyle bir cihaz veya adres yok|Son nokta adının `<accountname>.blob.<serialnumber>.microsoftdatabox.com` ana bilgisayar dosyasına şu `/etc/hosts`anda eklenmediğini denetleyin: .|
|AzCopy komutu bu hatayı görüntülemeden önce 20 dakika asılı görünüyor: <br>Hata ayrışdırma kaynak konumu... SSL bağlantısı kurulamadı.|Cihazınızın TLS/SSL sertifikasını sistemin sertifika deposuna aktarın. Daha fazla bilgi için [sertifikayı karşıdan yükleyin.](data-box-deploy-copy-data-via-rest.md#download-certificate)|

## <a name="errors-seen-in-azure-storage-library-for-python"></a>Python için Azure Depolama kitaplığında görülen hatalar

Bu bölümde, veri kopyalama için bir Linux istemcisi kullanılırken Veri Kutusu Diskdağıtımı sırasında karşılaşılan en önemli sorunlardan bazıları ayrıntılı olarak açıklanmaz.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|HTTP üstbilgilerinden birinin değeri doğru biçimde değildir. |Python için Microsoft Azure Depolama Kitaplığı'nın yüklenen sürümü Veri Kutusu tarafından desteklenmez. Desteklenen sürümler için Azure Veri Kutusu Blob depolama gereksinimlerine bakın.|
|… [SSL: CERTIFICATE_VERIFY_FAILED] ...|Python'u çalıştırmadan önce, REQUESTS_CA_BUNDLE ortamı değişkenini Base64 kodlanmış TLS sertifika dosyasının yoluna ayarlayın [(sertifikayı nasıl karşıdan yükleyin'](data-box-deploy-copy-data-via-rest.md#download-certificate)e bakın). <br>Örnek:<br>`export REQUESTS_CA_BUNDLE=/tmp/mycert.cer` <br>`python` <br>Alternatif olarak, sertifikayı sistemin sertifika deposuna ekleyin ve bu ortam değişkenini bu mağazanın yoluna ayarlayın. <br> Örneğin, Ubuntu üzerinde: <br>`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt` <br>`python`|


## <a name="common-errors"></a>Sık karşılaşılan hatalar

Bu hatalar herhangi bir uygulamaya özgü değildir.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|Bağlantı zamanları doldu. |Veri Kutusu aygıtında oturum açın ve kilidiolup olmadığını kontrol edin. Cihaz yeniden başlatılsa, birisi gelene kadar kilitli kalır.|

## <a name="next-steps"></a>Sonraki adımlar

- [Veri Kutusu Blob depolama sistemi gereksinimleri](data-box-system-requirements-rest.md)hakkında bilgi edinin.
