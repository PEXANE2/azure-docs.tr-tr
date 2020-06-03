---
title: REST arabirimini kullanarak sorun giderme Azure Data Box | Microsoft Docs
description: Veri kopyalama işlemi REST arabirimi aracılığıyla olduğunda Azure Data Box görülen sorunların nasıl giderileceği açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: ccb1a31761298d9dc4376684380702cf7cbd9a1f
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310079"
---
# <a name="troubleshoot-issues-related-to-azure-data-box-blob-storage"></a>Azure Data Box blob depolamayla ilgili sorunları giderme

Bu makalede, verileri kopyalamak için Data Box REST arabirimi aracılığıyla Data Box blob depolamayı kullanırken görebileceğiniz sorunları giderme hakkında bilgi bulabilirsiniz. Bu sorunlar, diğer uygulamalarla Data Box BLOB depolama alanı veya Python için Azure Depolama Gezgini, AzCopy veya Azure depolama kitaplığı gibi istemci kitaplıkları kullanırken ortaya çıkan bir yüzey vardır.

## <a name="errors-seen-in-azure-storage-explorer"></a>Azure Depolama Gezgini görülen hatalar

Bu bölümde Data Box blob depolamayla Azure Depolama Gezgini kullanırken ortaya çıkan sorunlardan bazıları ayrıntılı olarak ele verilebilir.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|Alt kaynaklar alınamıyor. HTTP başlıklarındaki bir değer doğru biçimde değil.|**Düzenle** menüsünde **hedef Azure Stack API 'leri**' ni seçin. <br>Azure Depolama Gezgini yeniden başlatın.|
|`getaddrinfo ENOTFOUND <accountname>.blob.<serialnumber>.microsoftdatabox.com` |Uç nokta adının `<accountname>.blob.<serialnumber>.microsoftdatabox.com` Bu yoldaki Hosts dosyasına eklendiğinden emin olun: <li>`C:\Windows\System32\drivers\etc\hosts`Windows veya </li><li> `/etc/hosts`Linux üzerinde.</li>|
|Alt kaynaklar alınamıyor. <br>Ayrıntılar: otomatik olarak imzalanan sertifika |Cihazınız için TLS/SSL sertifikasını Azure Depolama Gezgini içine aktarın: <li>Azure portal sertifikayı indirin. Daha fazla bilgi için [sertifikayı indirme](data-box-deploy-copy-data-via-rest.md#download-certificate)bölümüne gidin.</li><li>**Düzenle** menüsünde **SSL sertifikaları** ' nı seçin ve ardından **sertifikaları içeri aktar**' ı seçin.</li>|

## <a name="errors-seen-in-azcopy-for-windows"></a>Windows için AzCopy 'da görülen hatalar

Bu bölümde, Windows için AzCopy Data Box blob depolamayla kullanılırken ortaya çıkan sorunlardan bazıları ayrıntılı olarak ele verilebilir.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|AzCopy komutu, bu hatayı görüntülemeden önce bir dakika boyunca yanıt vermeyi durdurmuş gibi görünüyor: <br>Dizin https://numaralandırılamadı... Uzak ad çözümlenemedi`<accountname>.blob.<serialnumber>.microsoftdatabox.com`|Uç nokta adının `<accountname>.blob.<serialnumber>.microsoftdatabox.com` Şu adreste bulunan Hosts dosyasına eklendiğinden emin olun: `C:\Windows\System32\drivers\etc\hosts` .|
|AzCopy komutu, bu hatayı görüntülemeden önce bir dakika boyunca yanıt vermeyi durdurmuş gibi görünüyor: <br>Kaynak konumu ayrıştırılırken hata oluştu. Temel alınan bağlantı kapatıldı: SSL/TLS güvenli kanalı için güven ilişkisi kurulamadı.|Cihazınızın TLS/SSL sertifikasını sistemin sertifika deposuna aktarın. Daha fazla bilgi için [sertifikayı indirme](data-box-deploy-copy-data-via-rest.md#download-certificate)bölümüne gidin.|


## <a name="errors-seen-in-azcopy-for-linux"></a>Linux için AzCopy 'da görülen hatalar

Bu bölümde, Data Box BLOB depolama ile Linux için AzCopy kullanılırken ortaya çıkan sorunlardan bazıları ayrıntılı olarak ele verilebilir.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|AzCopy komutu, bu hatayı görüntülemeden önce 20 dakika boyunca yanıt vermeyi durdurmuş gibi görünüyor: <br>Kaynak konumu ayrıştırılırken hata oluştu `https://<accountname>.blob.<serialnumber>.microsoftdatabox.com/<cntnr>` . Böyle bir cihaz veya adres yok|Uç nokta adının `<accountname>.blob.<serialnumber>.microsoftdatabox.com` Şu adreste bulunan Hosts dosyasına eklendiğinden emin olun: `/etc/hosts` .|
|AzCopy komutu, bu hatayı görüntülemeden önce 20 dakika boyunca yanıt vermeyi durdurmuş gibi görünüyor: <br>Kaynak konumu ayrıştırılırken hata oluştu... SSL bağlantısı kurulamadı.|Cihazınızın TLS/SSL sertifikasını sistemin sertifika deposuna aktarın. Daha fazla bilgi için [sertifikayı indirme](data-box-deploy-copy-data-via-rest.md#download-certificate)bölümüne gidin.|

## <a name="errors-seen-in-azure-storage-library-for-python"></a>Python için Azure depolama kitaplığı 'nda görülen hatalar

Bu bölümde, veri kopyalama için bir Linux istemcisi kullanılırken Data Box Disk dağıtımı sırasında ortaya çıkan başlıca sorunlardan bazıları ayrıntılı olarak ele verilebilir.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|HTTP başlıklarındaki bir değer doğru biçimde değil. |Python için Microsoft Azure Depolama kitaplığının yüklü sürümü Data Box tarafından desteklenmiyor. Desteklenen sürümler için Azure Data Box BLOB depolama gereksinimleri bölümüne bakın.|
|… [SSL: CERTIFICATE_VERIFY_FAILED]...|Python 'u çalıştırmadan önce, REQUESTS_CA_BUNDLE ortam değişkenini Base64 kodlamalı TLS sertifika dosyasının yoluna ayarlayın (bkz. [sertifikayı indirme](data-box-deploy-copy-data-via-rest.md#download-certificate)). <br>Örneğin:<br>`export REQUESTS_CA_BUNDLE=/tmp/mycert.cer` <br>`python` <br>Alternatif olarak, sertifikayı sistemin sertifika deposuna ekleyin ve bu ortam değişkenini bu deponun yoluna ayarlayın. <br> Örneğin, Ubuntu üzerinde: <br>`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt` <br>`python`|


## <a name="common-errors"></a>Sık karşılaşılan hatalar

Bu hatalar herhangi bir uygulamaya özgü değildir.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|Bağlantı zaman aşımına uğrar. |Data Box cihazda oturum açın ve kilidinin açık olup olmadığını denetleyin. Cihaz her yeniden başlatıldığında, birisi oturum açana kadar kilitli kalır.|

## <a name="next-steps"></a>Sonraki adımlar

- [Data Box BLOB depolama sistem gereksinimleri](data-box-system-requirements-rest.md)hakkında bilgi edinin.
