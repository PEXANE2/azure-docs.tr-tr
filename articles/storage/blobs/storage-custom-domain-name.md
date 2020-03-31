---
title: Azure Blob Depolama bitiş noktasıyla özel bir etki alanı haritası
titleSuffix: Azure Storage
description: Azure depolama hesabındaki blob depolama alanı veya web bitiş noktasıyla özel bir etki alanı haritası belirleyin.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: normesta
ms.reviewer: dineshm
ms.subservice: blobs
ms.openlocfilehash: 9d05677ec47851557594ef47499da653accad141
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370483"
---
# <a name="map-a-custom-domain-to-an-azure-blob-storage-endpoint"></a>Azure Blob Depolama bitiş noktasıyla özel bir etki alanı haritası

Özel bir etki alanını bir blob hizmeti bitiş noktası veya statik bir [web sitesi](storage-blob-static-website.md) bitiş noktasıyla eşleyebilirsiniz. 

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

> [!NOTE] 
> Bu eşleme yalnızca alt etki alanları `www.contoso.com`için çalışır (örneğin: ). Web bitiş noktanızın kök etki alanında kullanılabilir olmasını istiyorsanız `contoso.com`(örneğin: ), Azure CDN'yi kullanmanız gerekir. Kılavuz için, bu makalenin [HTTPS etkin bölümüne sahip özel bir etki alanı haritasına](#enable-https) bakın. Özel etki alanınızın kök etki alanını etkinleştirmek için bu makalenin o bölümüne gitmeniz nedeniyle, bu bölümdeki HTTPS'yi etkinleştirme adımı isteğe bağlıdır. 

<a id="enable-http" />

## <a name="map-a-custom-domain-with-only-http-enabled"></a>Yalnızca HTTP etkinleştirilmiş özel bir etki alanını eşle

Bu yaklaşım daha kolaydır, ancak yalnızca HTTP erişimi sağlar. Depolama hesabı HTTPS üzerinden [güvenli aktarım gerektirecek](../common/storage-require-secure-transfer.md) şekilde yapılandırıldıysa, özel etki alanınız için HTTPS erişimini etkinleştirmeniz gerekir. 

HTTPS erişimini etkinleştirmek için, bu makalenin [HTTPS etkin bölümüne sahip özel bir etki alanı haritasına](#enable-https) bakın. 

<a id="map-a-domain" />

### <a name="map-a-custom-domain"></a>Özel bir etki alanı yla haritala

> [!IMPORTANT]
> Yapılandırmayı tamamlarken özel etki alanınız kısa bir süre kullanıcılar tarafından kullanılamaz. Etki alanınız şu anda sıfır kapalı kalma süresi gerektiren hizmet düzeyi sözleşmesi (SLA) olan bir uygulamayı destekliyorsa, DNS eşleme gerçekleşirken kullanıcıların etki alanınıza erişebilmesini sağlamak için bu makalenin sıfır kesinti süresi bölümüne [sahip özel bir etki alanı yla Eşle'deki](#zero-down-time) adımları izleyin.

Etki alanının kısa bir süre kullanıcılarınızın kullanımına açık olmadığını umursamazsanız, aşağıdaki adımları izleyin.

:heavy_check_mark: Adım 1: Depolama bitiş noktanızın ana bilgisayar adını alın.

:heavy_check_mark: Adım 2: Etki alanı sağlayıcınızla bir kanonik ad (CNAME) kaydı oluşturun.

:heavy_check_mark: Adım 3: Azure ile özel etki alanını kaydedin. 

:heavy_check_mark: Adım 4: Özel etki alanınızı test edin.

<a id="endpoint" />

#### <a name="step-1-get-the-host-name-of-your-storage-endpoint"></a>Adım 1: Depolama bitiş noktanızın ana bilgisayar adını alma 

Ana bilgisayar adı, protokol tanımlayıcısı ve son çizgi olmadan depolama bitiş noktası URL'sidir. 

1. Azure [portalında](https://portal.azure.com)depolama hesabınıza gidin.

2. Menü bölmesinde **Ayarlar'ın**altında **Özellikler'i**seçin.  

3. **Birincil Blob Hizmet Bitiş Noktası** veya **Birincil statik web sitesi bitiş noktasının** değerini bir metin dosyasına kopyalayın. 

4. Protokol tanımlayıcısını *(örn.* HTTPS) ve bu dizeden son çizgiyi kaldırın. Aşağıdaki tablo örnekleri içerir.

   | Bitiş noktası türü |  endpoint | ev sahibi adı |
   |------------|-----------------|-------------------|
   |blob servisi  | `https://mystorageaccount.blob.core.windows.net/` | `mystorageaccount.blob.core.windows.net` |
   |statik web sitesi  | `https://mystorageaccount.z5.web.core.windows.net/` | `mystorageaccount.z5.web.core.windows.net` |
  
   Bu değeri daha sonra için bir kenara koyun.

<a id="create-cname-record" />

#### <a name="step-2-create-a-canonical-name-cname-record-with-your-domain-provider"></a>Adım 2: Etki alanı sağlayıcınızla bir kanonik ad (CNAME) kaydı oluşturma

Ev sahibi adınızı işaret edecek bir CNAME kaydı oluşturun. CNAME kaydı, bir kaynak etki alanı adını hedef etki alanı adına eşleyen bir DNS kaydı türüdür.

1. Etki alanı kayıt şirketinizin web sitesinde oturum açın ve ardından DNS ayarını yönetmek için sayfaya gidin.

   Sayfayı **Alan Adı**, **DNS**veya Ad **Sunucusu Yönetimi**adlı bir bölümde bulabilirsiniz.

2. CNAME kayıtlarını yönetmek için bölümü bulun. 

   Gelişmiş bir ayarlar sayfasına gidip **CNAME,** **Diğer Ad**veya Alt Etki **Alanlarını aramanız**gerekebilir.

3. CNAME kaydı oluşturun. Bu kaydın bir parçası olarak, aşağıdaki öğeleri sağlayın: 

   - Alt etki alanı diğer `www` `photos`adı veya . Alt etki alanı gereklidir, kök etki alanları desteklenmez. 
      
   - Bu makalede, [depolama bitiş noktası bölümünün ana bilgisayar adını al'dan](#endpoint) elde ettiğiniz ana bilgisayar adı. 

<a id="register" />

#### <a name="step-3-register-your-custom-domain-with-azure"></a>Adım 3: Azure ile özel etki alanınızı kaydedin

1. Azure [portalında](https://portal.azure.com)depolama hesabınıza gidin.

2. Menü bölmesinde, **Blob Service**altında, **Özel etki alanını**seçin.  

   ![özel etki alanı seçeneği](./media/storage-custom-domain-name/custom-domain-button.png "özel etki alanı")

   **Özel etki alanı** bölmesi açılır.

3. Etki **Alanı adı** metin kutusuna, alt etki alanı da dahil olmak üzere özel etki alanınızın adını girin  
   
   Örneğin, etki alanınız *contoso.com* ve alt etki alanı `www.contoso.com`takma adınız *www*ise, girin. Alt etki alanınız *fotoğraflarsa,* girin. `photos.contoso.com`

4. Özel etki alanını kaydetmek için **Kaydet** düğmesini seçin.

   CNAME kaydı Etki Alanı Adı Sunucuları (DNS) üzerinden yayıldıktan ve kullanıcılarınız uygun izinlere sahipse, özel etki alanını kullanarak blob verilerini görüntüleyebilir.

#### <a name="step-4-test-your-custom-domain"></a>Adım 4: Özel etki alanınızı test edin

Özel etki alanınızın blob hizmeti bitiş noktanıza eşlenindiğini doğrulamak için, depolama hesabınızdaki ortak bir kapsayıcıda bir blob oluşturun. Daha sonra, bir web tarayıcısında, aşağıdaki biçimde bir URI kullanarak blob erişin:`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Örneğin, *photos.contoso.com* özel alt etki alanında *myforms* kapsayıcısındaki bir web formuna erişmek için aşağıdaki URI'yi kullanabilirsiniz:`http://photos.contoso.com/myforms/applicationform.htm`

<a id="zero-down-time" />

### <a name="map-a-custom-domain-with-zero-downtime"></a>Sıfır kapalı kalma süresiyle özel bir etki alanını eşle

> [!NOTE]
> Etki alanının kısa bir süre için kullanıcılarınız açısından kullanılamadığını dikkate almadığınızda, Harita' daki bu makalenin [çağrısı](#map-a-domain) büyük adı büyüyü/ Daha az adımla daha basit bir yaklaşım.  

Etki alanınız şu anda sıfır kapalı kalma süresi gerektiren hizmet düzeyi sözleşmesi (SLA) olan bir uygulamayı destekliyorsa, DNS eşleme gerçekleşirken kullanıcıların etki alanınıza erişebilmesini sağlamak için aşağıdaki adımları izleyin. 

:heavy_check_mark: Adım 1: Depolama bitiş noktanızın ana bilgisayar adını alın.

:heavy_check_mark: Adım 2: Etki alanı sağlayıcınızla bir ara ad (CNAME) kaydı oluşturun.

:heavy_check_mark: Adım 3: Azure'a özel etki alanını önceden kaydedin.

:heavy_check_mark: Adım 4: Etki alanı sağlayıcınızla bir CNAME kaydı oluşturun.

:heavy_check_mark: Adım 5: Özel etki alanınızı test edin.

<a id="endpoint-2" />

#### <a name="step-1-get-the-host-name-of-your-storage-endpoint"></a>Adım 1: Depolama bitiş noktanızın ana bilgisayar adını alma 

Ana bilgisayar adı, protokol tanımlayıcısı ve son çizgi olmadan depolama bitiş noktası URL'sidir. 

1. Azure [portalında](https://portal.azure.com)depolama hesabınıza gidin.

2. Menü bölmesinde **Ayarlar'ın**altında **Özellikler'i**seçin.  

3. **Birincil Blob Hizmet Bitiş Noktası** veya **Birincil statik web sitesi bitiş noktasının** değerini bir metin dosyasına kopyalayın. 

4. Protokol tanımlayıcısını *(örn.* HTTPS) ve bu dizeden son çizgiyi kaldırın. Aşağıdaki tablo örnekleri içerir.

   | Bitiş noktası türü |  endpoint | ev sahibi adı |
   |------------|-----------------|-------------------|
   |blob servisi  | `https://mystorageaccount.blob.core.windows.net/` | `mystorageaccount.blob.core.windows.net` |
   |statik web sitesi  | `https://mystorageaccount.z5.web.core.windows.net/` | `mystorageaccount.z5.web.core.windows.net` |
  
   Bu değeri daha sonra için bir kenara koyun.

#### <a name="step-2-create-a-intermediary-canonical-name-cname-record-with-your-domain-provider"></a>Adım 2: Etki alanı sağlayıcınızla bir ara kanonik ad (CNAME) kaydı oluşturma

Ana bilgisayar adınızı işaret etmek için geçici bir CNAME kaydı oluşturun. CNAME kaydı, bir kaynak etki alanı adını hedef etki alanı adına eşleyen bir DNS kaydı türüdür.

1. Etki alanı kayıt şirketinizin web sitesinde oturum açın ve ardından DNS ayarını yönetmek için sayfaya gidin.

   Sayfayı **Alan Adı**, **DNS**veya Ad **Sunucusu Yönetimi**adlı bir bölümde bulabilirsiniz.

2. CNAME kayıtlarını yönetmek için bölümü bulun. 

   Gelişmiş bir ayarlar sayfasına gidip **CNAME,** **Diğer Ad**veya Alt Etki **Alanlarını aramanız**gerekebilir.

3. CNAME kaydı oluşturun. Bu kaydın bir parçası olarak, aşağıdaki öğeleri sağlayın: 

   - Alt etki alanı diğer `www` `photos`adı veya . Alt etki alanı gereklidir, kök etki alanları desteklenmez.

     Diğer `asverify` ada alt etki alanını ekleyin. Örneğin: `asverify.www` veya `asverify.photos`.
       
   - Bu makalede, [depolama bitiş noktası bölümünün ana bilgisayar adını al'dan](#endpoint) elde ettiğiniz ana bilgisayar adı. 

     Alt etki `asverify` alanını ana bilgisayar adına ekleyin. Örneğin: `asverify.mystorageaccount.blob.core.windows.net`.

4. Özel etki alanını kaydetmek için **Kaydet** düğmesini seçin.

   Kayıt başarılı olursa, portal depolama hesabınızın başarıyla güncelleştirdiğini size haber vetir. Özel etki alanınız Azure tarafından doğrulandı, ancak etki alanınızın trafiği henüz depolama hesabınıza yönlendirilemedi.

#### <a name="step-3-pre-register-your-custom-domain-with-azure"></a>Adım 3: Azure ile özel etki alanınızı önceden kaydettirin

Özel etki alanınızı Azure'a ön kaydettirdiğinizde, azure'un etki alanı için DNS kaydını değiştirmek zorunda kalmadan özel etki alanınızı tanımasına izin verirsiniz. Bu şekilde, etki alanı için DNS kaydını değiştirdiğinizde, hiçbir kesinti olmadan blob bitiş noktasına eşlenir.

1. Azure [portalında](https://portal.azure.com)depolama hesabınıza gidin.

2. Menü bölmesinde, **Blob Service**altında, **Özel etki alanını**seçin.  

   ![özel etki alanı seçeneği](./media/storage-custom-domain-name/custom-domain-button.png "özel etki alanı")

   **Özel etki alanı** bölmesi açılır.

3. Etki **Alanı adı** metin kutusuna, alt etki alanı da dahil olmak üzere özel etki alanınızın adını girin  
   
   Örneğin, etki alanınız *contoso.com* ve alt etki alanı `www.contoso.com`takma adınız *www*ise, girin. Alt etki alanınız *fotoğraflarsa,* girin. `photos.contoso.com`

4. Dolaylı **CNAME doğrulama onay** kutusunu kullan'ı seçin.

5. Özel etki alanını kaydetmek için **Kaydet** düğmesini seçin.
  
   CNAME kaydı Etki Alanı Adı Sunucuları (DNS) üzerinden yayıldıktan ve kullanıcılarınız uygun izinlere sahipse, özel etki alanını kullanarak blob verilerini görüntüleyebilir.

#### <a name="step-4-create-a-cname-record-with-your-domain-provider"></a>Adım 4: Etki alanı sağlayıcınızla bir CNAME kaydı oluşturma

Ana bilgisayar adınızı işaret etmek için geçici bir CNAME kaydı oluşturun.

1. Etki alanı kayıt şirketinizin web sitesinde oturum açın ve ardından DNS ayarını yönetmek için sayfaya gidin.

   Sayfayı **Alan Adı**, **DNS**veya Ad **Sunucusu Yönetimi**adlı bir bölümde bulabilirsiniz.

2. CNAME kayıtlarını yönetmek için bölümü bulun. 

   Gelişmiş bir ayarlar sayfasına gidip **CNAME,** **Diğer Ad**veya Alt Etki **Alanlarını aramanız**gerekebilir.

3. CNAME kaydı oluşturun. Bu kaydın bir parçası olarak, aşağıdaki öğeleri sağlayın: 

   - Alt etki alanı diğer `www` `photos`adı veya . Alt etki alanı gereklidir, kök etki alanları desteklenmez.
      
   - Bu makalede, [depolama bitiş noktası bölümünün ana bilgisayar adını al'dan](#endpoint-2) elde ettiğiniz ana bilgisayar adı. 

#### <a name="step-5-test-your-custom-domain"></a>Adım 5: Özel etki alanınızı test edin

Özel etki alanınızın blob hizmeti bitiş noktanıza eşlenindiğini doğrulamak için, depolama hesabınızdaki ortak bir kapsayıcıda bir blob oluşturun. Daha sonra, bir web tarayıcısında, aşağıdaki biçimde bir URI kullanarak blob erişin:`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Örneğin, *photos.contoso.com* özel alt etki alanında *myforms* kapsayıcısındaki bir web formuna erişmek için aşağıdaki URI'yi kullanabilirsiniz:`http://photos.contoso.com/myforms/applicationform.htm`

### <a name="remove-a-custom-domain-mapping"></a>Özel etki alanı eşleme

Özel etki alanı eşlemi kaldırmak için özel etki alanının kaydını silin. Aşağıdaki yordamlardan birini kullanın.

#### <a name="portal"></a>[Portal](#tab/azure-portal)

Özel etki alanı ayarını kaldırmak için aşağıdakileri yapın:

1. Azure [portalında](https://portal.azure.com)depolama hesabınıza gidin.

2. Menü bölmesinde, **Blob Service**altında, **Özel etki alanını**seçin.  
   **Özel etki alanı** bölmesi açılır.

3. Özel etki alanı adınızı içeren metin kutusunun içeriğini temizleyin.

4. **Kaydet** düğmesini seçin.

Özel etki alanı başarıyla kaldırıldıktan sonra, depolama hesabınızın başarıyla güncelleştirildiğini belirten bir portal bildirimi görürsünüz

#### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Özel bir etki alanı kaydını kaldırmak için [az depolama hesabı güncelleştirme](https://docs.microsoft.com/cli/azure/storage/account) CLI komutunu kullanın ve ardından bağımsız değişken değeri için boş bir dize ()`""`belirtin. `--custom-domain`

* Komut biçimi:

  ```azurecli
  az storage account update \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --custom-domain ""
  ```

* Komut örneği:

  ```azurecli
  az storage account update \
      --name mystorageaccount \
      --resource-group myresourcegroup \
      --custom-domain ""
  ```

#### <a name="powershell"></a>[Powershell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Özel bir etki alanı kaydını kaldırmak için [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) PowerShell cmdlet'i kullanın ve ardından bağımsız değişken değeri için boş bir dize (`""`) belirtin. `-CustomDomainName`

* Komut biçimi:

  ```powershell
  Set-AzStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Komut örneği:

  ```powershell
  Set-AzStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```
---

<a id="enable-https" />

## <a name="map-a-custom-domain-with-https-enabled"></a>HTTPS etkinken özel bir etki alanı haritası

Bu yaklaşım daha fazla adım içerir, ancak HTTPS erişimi sağlar. 

Kullanıcıların HTTPS'yi kullanarak blob veya web içeriğinize erişmelerine gerek yoksa, haritayı bu makalenin [yalnızca HTTP etkin bölümüyle özel bir etki alanı](#enable-http) olarak görün. 

Özel bir etki alanının haritasını çıkarmak ve HTTPS erişimini etkinleştirmek için aşağıdakileri yapın:

1. Blob veya web bitiş noktanızda [Azure CDN'yi](../../cdn/cdn-overview.md) etkinleştirin. 

   Blob Depolama bitiş noktası için [bkz.](../../cdn/cdn-create-a-storage-account-with-cdn.md) 

   Statik bir web sitesi bitiş noktası [için](static-website-content-delivery-network.md)bkz.

2. [Azure CDN içeriğini özel bir etki alanına](../../cdn/cdn-map-content-to-custom-domain.md)eşleştirin.

3. [Azure CDN özel etki alanında HTTPS'yi etkinleştirin.](../../cdn/cdn-custom-ssl.md)

   > [!NOTE] 
   > Statik web sitenizi güncellediğinizde, CDN kenar ı sunucularında önbelleğe alınmış içeriği CDN bitiş noktasını temizleyerek temizlediğinizden emin olun. Daha fazla bilgi için bkz. [Azure CDN uç noktasını temizleme](../../cdn/cdn-purge-endpoint.md).

4. (İsteğe bağlı) Aşağıdaki kılavuzu gözden geçirin:

   * [Azure CDN ile paylaşılan erişim imzası (SAS) belirteçleri.](https://docs.microsoft.com/azure/cdn/cdn-storage-custom-domain-https#shared-access-signatures)

   * [Azure CDN ile HTTP-to-HTTPS yeniden yönlendirme.](https://docs.microsoft.com/azure/cdn/cdn-storage-custom-domain-https#http-to-https-redirection)

   * [Azure CDN ile Blob Depolama kullanırken fiyatlandırma ve faturalandırma.](https://docs.microsoft.com/azure/cdn/cdn-storage-custom-domain-https#http-to-https-redirection)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Blob depolama alanında statik web sitesi barındırma hakkında bilgi edinin](storage-blob-static-website.md)
