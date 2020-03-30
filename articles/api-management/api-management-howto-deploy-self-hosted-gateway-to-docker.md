---
title: Docker'a kendi barındırılan Azure API Yönetimi ağ geçidini dağıtın | Microsoft Dokümanlar
description: Docker'a kendi barındırılan Azure API Yönetimi ağ geçidini nasıl dağıtılayacağımöğrenin
services: api-management
documentationcenter: ''
author: vladvino
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2019
ms.author: apimpm
ms.openlocfilehash: b5acda30f7f4a01e1b7b6ac82652452c3c40e3bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75768512"
---
# <a name="deploy-an-azure-api-management-self-hosted-gateway-to-docker"></a>Azure API Yönetimi'ni Docker'a kendi barındırılan ağ geçidini dağıtma

Bu makalede, kendi barındırılan Azure API Yönetimi ağ geçidini Docker ortamına dağıtma adımları sağlanmaktadır.

> [!NOTE]
> Kendi kendine barındırılan ağ geçidi özelliği önizlemededir. Önizleme sırasında, kendi kendine barındırılan ağ geçidi ek ücret ödemeden yalnızca Geliştirici ve Premium katmanlarında kullanılabilir. Geliştirici katmanı, kendi kendine barındırılan tek bir ağ geçidi dağıtımıyla sınırlıdır.

## <a name="prerequisites"></a>Ön koşullar

- Aşağıdaki hızlı başlatmayı tamamlayın: [Azure API Yönetimi örneği oluşturun](get-started-create-service-instance.md)
- Docker ortamı oluşturun. [Docker for Desktop](https://www.docker.com/products/docker-desktop) geliştirme ve değerlendirme amacıyla iyi bir seçenektir. Tüm Docker sürümleri, özellikleri ve Docker'ın kendisi hakkındaki kapsamlı belgeler hakkında bilgi için [Docker belgelerine](https://docs.docker.com) bakın.
- [API Yönetimi örneğinde ağ geçidi kaynağı sağlama](api-management-howto-provision-self-hosted-gateway.md)

> [!NOTE]
> Kendi kendine barındırılan ağ geçidi x86-64 Linux tabanlı Docker konteynerolarak paketlenmiştir.

## <a name="deploy-the-self-hosted-gateway-to-docker"></a>Docker'a kendi kendine barındırılan ağ geçidini dağıtın

1. **Ayarlar'ın**altından **Ağ Geçitleri'ni** seçin.
2. Dağıtmayı planlamayı planladiğiniz ağ geçidi kaynağını seçin.
3. **Dağıtım'ı**seçin.
4. **Belirteç** metin kutusundaki yeni bir belirteç varsayılan **Son Kullanma ve** Gizli **Anahtar** değerlerini kullanarak sizin için otomatik olarak oluşturulduğunu unutmayın. İstenirseniz ya da her ikisini de ayarlayın ve yeni bir belirteç oluşturmak için **Oluştur'u** seçin.
4. **Docker** **Docker'ın Dağıtım komut dosyaları**altında seçildiğinden emin olun.
5. Dosyayı indirmek için **Çevre'nin** yanındaki **env.conf** dosya bağlantısını seçin.
6. Docker komutunu panoya kaydetmek için **Çalıştır** metin kutusunun sağ ucunda bulunan **kopya** simgesini seçin.
7. Komutu terminal (veya komut) penceresine yapıştırın. Gerektiğinde bağlantı noktası eşlemelerini ve konteyner adını ayarlayın. Komutun indirilen ortam dosyasının geçerli dizinde bulunmasını beklediğini unutmayın.
```
    docker run -d -p 80:8080 -p 443:8081 --name <gateway-name> --env-file env.conf mcr.microsoft.com/azure-api-management/gateway:<tag>
```
8. Komutu yürütün. Komut, Microsoft Konteyner Kayıt Defteri'nden indirilen kendi kendine barındırılan ağ geçidinin görüntüsünü kullanarak ve kapsayıcının HTTP (8080) ve HTTPS (8081) bağlantı noktalarını ana bilgisayardaki 80 ve 443 bağlantı noktalarıyla eşlemek için Docker ortamınıza kapsayıcıyı çalıştırmatalimatı verir.
9. Ağ geçidi bölmesinin çalıştığını denetlemek için aşağıdaki komutu çalıştırın:
```console
docker ps
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS              PORTS                                         NAMES
895ef0ecf13b        mcr.microsoft.com/azure-api-management/gateway:beta   "/bin/sh -c 'dotnet …"   5 seconds ago       Up 3 seconds        0.0.0.0:80->8080/tcp, 0.0.0.0:443->8081/tcp   my-gateway
```
10. Azure portalına geri dön ve az önce dağıttığınız ağ geçidi düğümünin sağlıklı durum bildirdiğini onaylayın.

![ağ geçidi durumu](media/api-management-howto-deploy-self-hosted-gateway-to-docker/status.png)

> [!TIP]
> Kendi <code>console docker container logs <gateway-name></code> kendine barındırılan ağ geçidi günlüğünün anlık görüntüsünü görüntülemek için komutu kullanın.
>
> Tüm <code>docker container logs --help</code> günlük görüntüleme seçeneklerini görmek için komutu kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* Kendi kendine barındırılan ağ geçidi hakkında daha fazla bilgi edinmek için [Azure API Yönetimi'nin kendi barındırılan ağ geçidine genel bakış'a](self-hosted-gateway-overview.md)bakın.
* [Kendi kendine barındırılan ağ geçidi için özel etki alanı adını yapılandırın.](api-management-howto-configure-custom-domain-gateway.md)
