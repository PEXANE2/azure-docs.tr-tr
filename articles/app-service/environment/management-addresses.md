---
title: Yönetim adresleri
description: Bir Uygulama Hizmet Ortamını denetlemek için kullanılan yönetim adreslerini bulun. Asimetrik yönlendirme sorunlarını önlemek için bunları bir rota tablosunda yapılandırıldı.
author: ccompy
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.topic: article
ms.date: 11/13/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 7d7f97552e8faadee1af928a9ce4e1eea2df476e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687107"
---
# <a name="app-service-environment-management-addresses"></a>Uygulama Hizmeti Ortamı yönetimi adresleri

Uygulama Hizmet Ortamı (ASE), Azure Sanal Ağınızda (VNet) çalışan Azure Uygulama Hizmeti'nin tek bir kiracı dağıtımıdır.  ASE VNet'inizde çalışırken, hizmeti yönetmek için Azure Uygulama Hizmeti tarafından kullanılan bir dizi özel IP adresinden erişilebiliyor olması gerekir.  ASE durumunda, yönetim trafiği kullanıcı tarafından denetedilen ağa geçer. Bu trafik engellenirse veya yanlış yönlendirilirse, ASE askıya alınır. ASE ağ bağımlılıkları hakkında ayrıntılı bilgi için [Ağ la ilgili hususları ve Uygulama Hizmet Ortamı'nı][networking]okuyun. ASE hakkında genel bilgi [için, Uygulama Hizmet Ortamına Giriş][intro]ile başlayabilirsiniz.

Tüm ASEs yönetim trafiği içine girer bir kamu VIP var. Bu adreslerden gelen yönetim trafiği, ASE'nizin genel VIP'si üzerindeki 454 ve 455 bağlantı noktalarından gelir. Bu belge, ASE'ye yönelik yönetim trafiği için Uygulama Hizmeti kaynak adreslerini listeler. Bu adresler AppServiceManagement adlı IP Hizmet Etiketinde de yer alıyor.

Aşağıda belirtilen adresler, yönetim trafiğiyle ilgili asimetrik yönlendirme sorunlarını önlemek için bir rota tablosunda yapılandırılabilir. Rotalar IP düzeyinde ki trafikte hareket eder ve trafik yönü veya trafiğin TCP yanıt iletisinin bir parçası olduğu konusunda bir farkındalığa sahip değildir. TCP isteğinin yanıt adresi gönderildiği adresten farklıysa, asimetrik yönlendirme sorununuz var demektir. ASE yönetim trafiğinizle ilgili asimetrik yönlendirme sorunlarını önlemek için, yanıtların gönderildikleri adresten geri gönderildiğinden emin olmanız gerekir. AsE'nizi, giden trafiğin şirket içinde gönderildiği bir ortamda çalışacak şekilde nasıl yapılandıracağınız hakkında ayrıntılı bilgi için, [ZORUNLU tünelleme ile ASE'nizi Yapılandır'ı okuyun][forcedtunnel]

## <a name="list-of-management-addresses"></a>Yönetim adresleri listesi ##

| Bölge | Adresler |
|--------|-----------|
| Tüm genel bölgeler | 13.64.115.203, 13.66.140.0, 13.67.8.128, 13.69.64.128, 13.69.227.128, 13.70.73.128, 13.71.170.64, 13.71.194.129, 13.75.34.192, 13.75.127.117, 13.77.50.128, 13.78.109.0, 13.89.171.0, 13.94.141.115, 13.94.143.126, 13.94.149.179, 20.36.106.128, 20.36.114.64, 23.100.226.236, 23.102.135.246, 23.102.188.65, 40.69.106.128, 40.70.146.128, 40.71.13.64, 40.74.100.64, 40.78.194.128, 40.79.130.64, 40.79.178.128, 40.83.120.64, 40.83.121.56, 40.83.125.161, 40.90.240.166, 40.91.126.196, 40.112.242.192, 40.119.4.111, 40.124.47.188, 51.140.146.64, 51.140.210.128, 52.151.25.45, 52.162.80.89, 52.162.106.192, 52.165.152.214, 52.165.153.122, 52.165.154.193, 52.165.158.140, 52.174.22.21, 52.178.177.147, 52.178.184.149, 52.178.190.65, 52.178.195.197, 52.187.56.50, 52.187.59.251, 52.187.63.19, 52.187.63.37, 52.224.105.172, 52.225.177.153, 52.231.18.64, 52.231.146.128, 65.52.14.230, 65.52.172.237, 65.52.193.203, 70.37.57.58, 70.37.89.222, 104.43.242.137, 104.44.129.141, 104.44.129.243, 104.44.129.255, 104.44.134.255, 104.208.54.11, 104.211.81.64, 104.211.146.128, 104.214.49.0, 157.55.176.93, 157.55.208.185, 191.233.203.64, 191.236.154.88 |
| Microsoft Azure Yönetimi | 23.97.29.209, 13.72.53.37, 13.72.180.105, 23.97.0.17, 23.97.16.184 |

## <a name="configuring-a-network-security-group"></a>Ağ Güvenlik Grubu Yapılandırma

Ağ Güvenlik Grupları ile, tek tek adresler veya kendi yapılandırmanızı korumak konusunda endişelenmenize gerek yoktur. AppServiceManagement adlı bir IP hizmet etiketi vardır ve tüm adreslerle güncel tutulur. Bu IP hizmet etiketini NSG'nizde kullanmak için portala gidin, Ağ Güvenlik Grupları UI'nizi açın ve Gelen güvenlik kurallarını seçin. Gelen yönetim trafiği için önceden varolan bir kuralınız varsa, kuralınızı güncellenin. Bu NSG ASE ile oluşturulmadıysa veya tamamen **yeniyse, Ekle'yi**seçin. Kaynak açılır gürüntin altında **Hizmet Etiketi'ni**seçin.  Kaynak hizmet etiketi altında **AppServiceManagement'ı**seçin. Kaynak \*bağlantı noktası aralıklarını , Hedef Any, Hedef bağlantı noktası aralıklarını **454-455,** Protokolden **TCP'ye**ve Eyleme **İzin Verecek'** olarak ayarlayın. **Any** Kuralı siz yapıyorsanız, Önceliği belirlemeniz gerekir. 

![hizmet etiketiyle bir NSG oluşturma][1]

## <a name="configuring-a-route-table"></a>Rota tablosunu yapılandırma

Yönetim adresleri, tüm gelen yönetim trafiğinin aynı yoldan geri dönebilmesini sağlamak için bir sonraki internet atlamalı bir rota tablosuna yerleştirilebilir. Zorunlu tünel yapılandırma sı için bu yollara ihtiyaç vardır. Rota tablosunu oluşturmak için portal, PowerShell veya Azure CLI portalını kullanabilirsiniz.  PowerShell komutundan Azure CLI'yi kullanarak bir rota tablosu oluşturmak için komutlar aşağıda verilmiştir. 

    $rg = "resource group name"
    $rt = "route table name"
    $location = "azure location"
    $managementAddresses = "13.64.115.203", "13.66.140.0", "13.67.8.128", "13.69.64.128", "13.69.227.128", "13.70.73.128", "13.71.170.64", "13.71.194.129", "13.75.34.192", "13.75.127.117", "13.77.50.128", "13.78.109.0", "13.89.171.0", "13.94.141.115", "13.94.143.126", "13.94.149.179", "20.36.106.128", "20.36.114.64", "23.100.226.236", "23.102.135.246", "23.102.188.65", "40.69.106.128", "40.70.146.128", "40.71.13.64", "40.74.100.64", "40.78.194.128", "40.79.130.64", "40.79.178.128", "40.83.120.64", "40.83.121.56", "40.83.125.161", "40.90.240.166", "40.91.126.196", "40.112.242.192", "40.119.4.111", "40.124.47.188", "51.140.146.64", "51.140.210.128", "52.151.25.45", "52.162.80.89", "52.162.106.192", "52.165.152.214", "52.165.153.122", "52.165.154.193", "52.165.158.140", "52.174.22.21", "52.178.177.147", "52.178.184.149", "52.178.190.65", "52.178.195.197", "52.187.56.50", "52.187.59.251", "52.187.63.19", "52.187.63.37", "52.224.105.172", "52.225.177.153", "52.231.18.64", "52.231.146.128", "65.52.14.230", "65.52.172.237", "65.52.193.203", "70.37.57.58", "70.37.89.222", "104.43.242.137", "104.44.129.141", "104.44.129.243", "104.44.129.255", "104.44.134.255", "104.208.54.11", "104.211.81.64", "104.211.146.128", "104.214.49.0", "157.55.176.93", "157.55.208.185", "191.233.203.64", "191.236.154.88"

    az network route-table create --name $rt --resource-group $rg --location $location
    foreach ($ip in $managementAddresses) {
        az network route-table route create -g $rg --route-table-name $rt -n $ip --next-hop-type Internet --address-prefix ($ip + "/32")
    }

Rota tablonuz oluşturulduktan sonra, bunu ASE alt ağınızda ayarlamanız gerekir.  

## <a name="get-your-management-addresses-from-api"></a>Yönetim adreslerinizi API'den alın ##

AsE'nize uyan yönetim adreslerini aşağıdaki API çağrısıyla listeleyebilirsiniz.

    get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01

API, ASE'nizin tüm gelen adreslerini içeren bir JSON belgesini döndürür. Adresler listesi yönetim adreslerini, ASE'niz tarafından kullanılan VIP'yi ve ASE alt net adres aralığını içerir.  

ApI'yi [armclient](https://github.com/projectkudu/ARMClient) ile aramak için aşağıdaki komutları kullanın, ancak abonelik kimliğinizi, kaynak grubunuzda ve ASE adınızı değiştirin.  

    armclient login
    armclient get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01


<!--IMAGES-->
[1]: ./media/management-addresses/managementaddr-nsg.png

<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
[forcedtunnel]: ./forced-tunnel-support.md
