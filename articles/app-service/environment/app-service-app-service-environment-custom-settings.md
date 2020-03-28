---
title: Özel ayarları yapılandırma
description: Tüm Azure Uygulama Hizmeti ortamına uygulanan ayarları yapılandırın. Azure Kaynak Yöneticisi şablonları ile nasıl yapacağınızı öğrenin.
author: stefsch
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.topic: tutorial
ms.date: 12/19/2019
ms.author: stefsch
ms.custom: mvc, seodec18
ms.openlocfilehash: 25393007a3cc878737ea5927cb65bcf7ef945313
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80057558"
---
# <a name="custom-configuration-settings-for-app-service-environments"></a>Uygulama Hizmet Ortamları için özel yapılandırma ayarları
## <a name="overview"></a>Genel Bakış
Uygulama Hizmet Ortamları (ADE'ler) tek bir müşteriye yalıtıldığı için, yalnızca Uygulama Hizmeti Ortamlarına uygulanabilecek belirli yapılandırma ayarları vardır. Bu makalede, Uygulama Hizmet Ortamları için kullanılabilen çeşitli özel özelleştirmeler belgelenmektedir.

Uygulama Hizmet Ortamınız yoksa, [Uygulama Hizmeti Ortamı Nasıl Oluşturulur'a](app-service-web-how-to-create-an-app-service-environment.md)bakın.

Yeni **kümeAyarlar** özniteliğinde bir dizi kullanarak App Service Environment özelleştirmelerini depolayabilirsiniz. Bu *öznitelik, barındırma Ortamları* Azure Kaynak Yöneticisi kuruluşunun "Özellikler" sözlüğünde bulunur.

Aşağıdaki kısaltılmış Kaynak Yöneticisi şablonu **kümesiAyarlar** özniteliğini gösterir:

    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

**clusterAyarlar** özniteliği, Uygulama Hizmeti Ortamını güncelleştirmek için Kaynak Yöneticisi şablonuna eklenebilir.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Uygulama Hizmeti Ortamını güncelleştirmek için Azure Kaynak Gezgini'ni kullanma
Alternatif olarak, [Azure Kaynak Gezgini'ni](https://resources.azure.com)kullanarak Uygulama Hizmet Ortamını güncelleştirebilirsiniz.  

1. Kaynak Gezgini'nde, App Service Environment **(abonelik** > **kaynaklarıGrup** > **sağlayıcıları** > **Microsoft.Web** > **hostingOrtamları)** için düğüme gidin. Ardından güncelleştirmek istediğiniz belirli Uygulama Hizmeti Ortamı'nı tıklatın.
2. Sağ bölmede, Kaynak Gezgini'nde etkileşimli düzenlemeye izin vermek için üst araç çubuğunda **Oku/Yaz'ı** tıklatın.  
3. Kaynak Yöneticisi şablonu değiştirilebilir yapmak için mavi **Edit** düğmesini tıklatın.
4. Sağ bölmenin altına kaydırın. **clusterSettings** özniteliği, değerini girebileceğiniz veya güncelleştirebileceğiniz en alttadır.
5. **ClusterSettings** özniteliğine istediğiniz yapılandırma değerleri dizisini yazın (veya kopyalayın ve yapıştırın).  
6. Değişikliği Uygulama Hizmeti Ortamı'na işlemek için sağ bölmenin üst kısmında bulunan yeşil **PUT** düğmesini tıklatın.

Değişikliği ancak gönderirseniz, değişikliğin yürürlüğe girmesi, Uygulama Hizmet Ortamındaki ön uç sayısıyla çarpılarak yaklaşık 30 dakika sürer.
Örneğin, bir Uygulama Servis Ortamının dört ön ucu varsa, yapılandırma güncelleştirmesinin tamamlanması yaklaşık iki saat sürer. Yapılandırma değişikliği kullanıma alınırken, Uygulama Hizmet Ortamı'nda başka ölçekleme işlemleri veya yapılandırma değişikliği işlemleri gerçekleşemez.

## <a name="enable-internal-encryption"></a>Dahili Şifrelemeyi Etkinleştir

App Service Environment, sistem içindeki dahili bileşenleri veya iletişimi göremeyeceğiniz bir kara kutu sistemi olarak çalışır. Daha yüksek iş elde edilmesini sağlamak için, iç bileşenler arasında varsayılan olarak şifreleme etkinleştirilir. Trafik izlenmeveya erişileme konusunda tamamen erişilememektedir gibi sistem güvenlidir. Veri yolunun uçtan uca tam şifrelemesini gerektiren bir uyumluluk gereksiniminvarsa, bunu bir clusterSetting ile etkinleştirmek için bir yol vardır.  

        "clusterSettings": [
            {
                "name": "InternalEncryption",
                "value": "1"
            }
        ],
 
InternalEncryption clusterSetting etkinleştirildikten sonra, sistem performansınız üzerinde bir etki olabilir. InternalEncryption'i etkinleştirmek için değişikliği yaptığınızda, değişiklik tamamen yayılana kadar ASE'niz kararsız bir durumda olacaktır. Değişikliğin tam olarak yayılması, ASE'nizde kaç örnek olduğunuza bağlı olarak tamamlanması birkaç saat sürebilir. Kullanımdayken bunu bir ASE'de etkinleştirmememenizi şiddetle öneririz. Bunu etkin olarak kullanılan bir ASE'de etkinleştirmeniz gerekiyorsa, işlem tamamlanana kadar trafiği yedek bir ortama aktarmanızı öneririz. 

## <a name="disable-tls-10-and-tls-11"></a>TLS 1.0 ve TLS 1.1 devre dışı

TLS ayarlarını uygulama bazında yönetmek istiyorsanız, [TLS ayarlarını zorlayın](../configure-ssl-bindings.md#enforce-tls-versions) belgeleriyle sağlanan kılavuzu kullanabilirsiniz. 

Bir ASE'deki tüm uygulamalar için gelen tüm TLS 1.0 ve TLS 1.1 trafiğini devre dışı kullanabilirsiniz, aşağıdaki **clusterAyarlar** girişini ayarlayabilirsiniz:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

Ayarın adı 1.0 diyor ancak yapılandırıldığınızda hem TLS 1.0 hem de TLS 1.1'i devre dışı bkalmıyor.

## <a name="change-tls-cipher-suite-order"></a>TLS şifre paketi siparişini değiştir
Müşterilerden gelen bir diğer soru da, sunucuları tarafından görüşülen şifrelerin listesini değiştirip değiştiremedikleridir ve bu da aşağıda gösterildiği gibi **clusterSettings** değiştirilerek elde edilebilir. Kullanılabilir şifre paketlerinin listesi [bu MSDN makalesinden](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx)alınabilir.

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> SChannel'ın anlayamayacağı şifreleme paketi için yanlış değerler ayarlanmışsa, sunucunuzdaki tüm TLS iletişimi çalışmayı durdurabilir. Böyle bir durumda, *FrontEndSSLCipherSuiteOrder* girişini **clusterSettings'ten** kaldırmanız ve varsayılan şifreleme paketi ayarlarına geri dönmek için güncelleştirilmiş Kaynak Yöneticisi şablonunu göndermeniz gerekir.  Lütfen bu işlevi dikkatli kullanın.
> 
> 

## <a name="get-started"></a>Kullanmaya başlayın
Azure Quickstart Kaynak Yöneticisi şablon sitesi, Uygulama [Hizmeti Ortamı oluşturmak](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/)için temel tanımı içeren bir şablon içerir.

<!-- LINKS -->

<!-- IMAGES -->
