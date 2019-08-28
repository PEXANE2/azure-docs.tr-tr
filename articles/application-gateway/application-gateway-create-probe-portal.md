---
title: Özel bir araştırma oluşturma-Azure Application Gateway-Azure portalı | Microsoft Docs
description: Portalı kullanarak Application Gateway için özel bir araştırma oluşturmayı öğrenin
services: application-gateway
documentationcenter: na
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: 33fd5564-43a7-4c54-a9ec-b1235f661f97
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: victorh
ms.openlocfilehash: b92b9d953b6dd941b8b5f445ad64059f557c2980
ms.sourcegitcommit: 388c8f24434cc96c990f3819d2f38f46ee72c4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70061798"
---
# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Portalı kullanarak Application Gateway için özel bir araştırma oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Klasik PowerShell](application-gateway-create-probe-classic-ps.md)

Bu makalede, mevcut bir uygulama ağ geçidine Azure portal aracılığıyla özel bir araştırma eklersiniz. Özel yoklamalar, belirli bir sistem durumu denetimi sayfası veya varsayılan Web uygulamasında başarılı bir yanıt sağlamayan uygulamalar için yararlıdır.

## <a name="before-you-begin"></a>Başlamadan önce

Henüz bir uygulama ağ geçidiniz yoksa, birlikte çalışmak üzere bir uygulama ağ geçidi oluşturmak için [Application Gateway oluştur](application-gateway-create-gateway-portal.md) ' u ziyaret edin.

## <a name="createprobe"></a>Araştırma oluşturma

Yoklamalar, Portal üzerinden iki adımlı bir işlemde yapılandırılır. İlk adım araştırmanın oluşturulması. İkinci adımda, uygulama ağ geçidinin arka uç http ayarlarına araştırma eklersiniz.

1. [Azure Portal](https://portal.azure.com)’da oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz deneme sürümü](https://azure.microsoft.com/free) için kaydolabilirsiniz

1. Azure portal sık kullanılanlar bölmesinde, tüm kaynaklar ' a tıklayın. Tüm kaynaklar dikey penceresinde uygulama ağ geçidine tıklayın. Seçtiğiniz abonelikte zaten çeşitli kaynaklar varsa, ada göre filtrele... partners.contoso.net girebilirsiniz. girebilirsiniz.

1. **Yoklamalar** ' a tıklayın ve bir araştırma eklemek için **Ekle** düğmesine tıklayın.

   ![Doldurulmuş bilgiler içeren araştırma dikey penceresi ekleme][1]

1. **Sistem durumu araştırması Ekle** dikey penceresinde, araştırma için gerekli bilgileri doldurun ve tamamlandığında **Tamam**' a tıklayın.

   |**Ayar** | **Değer** | **Ayrıntılar**|
   |---|---|---|
   |**Name**|Customaraştırması|Bu değer, portalda erişilebilen araştırmanın kolay bir adıdır.|
   |**Protokolü**|HTTP veya HTTPS | Sistem durumu araştırmasının kullandığı protokol.|
   |**Ana Bilgisayar**|Yani contoso.com|Bu değer, araştırma için kullanılan ana bilgisayar adıdır. Yalnızca Application Gateway 'de birden çok site yapılandırıldığında uygulanabilir, aksi takdirde ' 127.0.0.1 ' kullanın. Bu değer, VM ana bilgisayar adından farklıdır.|
   |**Yolu**|/veya başka bir yol|Özel araştırma için tam URL 'nin geri kalanı. Geçerli bir yol '/' ile başlar. Http:\//contoso.com ' nin varsayılan yolu için yalnızca '/' kullanın |
   |**Aralık (saniye)**|30|Araştırmanın sistem durumunu denetlemek için ne sıklıkta çalıştırıldığı. 30 saniyeden daha düşük bir değer ayarlamanız önerilmez.|
   |**Zaman aşımı (saniye)**|30|Araştırmanın zaman aşımından önce bekleyeceği süre. Zaman aşımı aralığı, arka uç sistem durumu sayfasının kullanılabilir olduğundan emin olmak için bir http çağrısının yüksek olması gerekir.|
   |**Sağlıksız eşik**|3|Sağlıksız kabul edilecek başarısız deneme sayısı. Eşik 1 veya daha fazlasına ayarlanabilir.|

   > [!IMPORTANT]
   > Ana bilgisayar adı sunucu adı ile aynı değil. Bu değer, uygulama sunucusunda çalışan sanal konağın adıdır. Araştırma http://(ana bilgisayar adı):(bağlantı noktasına httpsetting)/urlPath 'e gönderilir

## <a name="add-probe-to-the-gateway"></a>Ağ geçidine araştırma ekleme

Artık araştırma oluşturuldığına göre, ağ geçidine eklemek zaman alabilir. Araştırma ayarları, uygulama ağ geçidinin arka uç http ayarları üzerinde ayarlanır.

1. Uygulama ağ geçidinde **http ayarları** ' na tıklayın, yapılandırma dikey penceresini açmak için pencerede listelenen geçerli arka uç http ayarları ' na tıklayın.

   ![https ayarları penceresi][2]

1. **Appgatewaybackendhttpsettings** ayarları dikey penceresinde **özel araştırma kullan** onay kutusunu işaretleyin ve **özel araştırma** açılan penceresindeki [araştırma oluştur](#createprobe) bölümünde oluşturulan araştırmayı seçin.
   Tamamlandığında **Kaydet** ' e tıklayın ve ayarlar uygulanır.

Varsayılan araştırma, Web uygulamasına varsayılan erişimi denetler. Özel bir araştırma oluşturuldığına göre, uygulama ağ geçidi, arka uç sunucuları için sistem durumunu izlemek için tanımlanan özel yolu kullanır. Tanımlanan ölçütlere göre Application Gateway, araştırmanın belirtilen yolunu denetler. Ana bilgisayar: bağlantı noktası/yol çağrısı bir HTTP 200-399 durum yanıtı döndürmezse, sağlıksız eşiğe ulaşıldıktan sonra sunucu dönüşten alınır. Algılama, sağlıklı olmayan örnek üzerinde devam eder ve yeniden sağlıklı hale gelir. Örnek, sağlıklı sunucu havuzuna eklendikten sonra trafiği tekrar akışa başlar ve örneği yoklamaya başlar, Kullanıcı tarafından belirtilen aralıkta normal olarak devam eder.

## <a name="next-steps"></a>Sonraki adımlar

Azure Application Gateway ile SSL yük boşaltma yapılandırma hakkında bilgi edinmek için bkz. [SSL yük boşaltma yapılandırma](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png

