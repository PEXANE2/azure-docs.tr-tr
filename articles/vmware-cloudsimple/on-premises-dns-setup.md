---
title: Azure VMware çözümleri (AVS)-AVS özel bulutu için DNS 'yi yapılandırma
description: Şirket içi iş istasyonlarından bir AVS özel bulutu üzerindeki vCenter Server 'a erişim için DNS ad çözümlemesi ayarlamayı açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 12c4362ae1b075af132d5971f4fe0461c9d91733
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77083091"
---
# <a name="configure-dns-for-name-resolution-for-avs-private-cloud-vcenter-access-from-on-premises-workstations"></a>AVS 'nin şirket içi iş istasyonlarından özel bulut vCenter erişimi için ad çözümlemesi için DNS 'yi yapılandırma

Şirket içi iş istasyonlarından bir AVS özel bulutundaki vCenter Server 'a erişmek için, vCenter Server 'ın ana bilgisayar adının yanı sıra IP adresi ile adreslenmesi için DNS adresi çözümlemesini yapılandırmanız gerekir.

## <a name="obtain-the-ip-address-of-the-dns-server-for-your-avs-private-cloud"></a>AVS özel bulutunuz için DNS sunucusunun IP adresini edinin

1. [AVS portalında](access-cloudsimple-portal.md)oturum açın.

2. Bir **AVS özel bulutları** > **kaynaklar** ' a gıdın ve bağlanmak Istediğiniz AVS özel bulutunu seçin.

3. **Temel bilgiler**altında AVS özel bulutunun **Özet** sayfasında, AVS özel bulut DNS sunucusu IP adresini kopyalayın.

    ![AVS özel bulut DNS sunucuları](media/private-cloud-dns-server.png)


DNS yapılandırması için bu seçeneklerden birini kullanın.

* [*. Cloudsimple.io için DNS sunucusunda bir bölge oluşturun](#create-a-zone-on-a-microsoft-windows-dns-server)
* [*. Cloudsimple.io çözümlemek için şirket içi DNS sunucunuzda koşullu bir iletici oluşturun](#create-a-conditional-forwarder)

## <a name="create-a-zone-on-the-dns-server-for-cloudsimpleio"></a>*. Cloudsimple.io için DNS sunucusunda bir bölge oluşturun

Bir bölgeyi saplama bölgesi olarak ayarlayabilir ve ad çözümlemesi için özel buluttaki DNS sunucularına işaret edebilirsiniz. Bu bölüm, BIND DNS sunucusu veya Microsoft Windows DNS sunucusu kullanma hakkında bilgi sağlar.

### <a name="create-a-zone-on-a-bind-dns-server"></a>BIND DNS sunucusu üzerinde bölge oluşturma

Belirli dosya ve yapılandırılacak parametreler, bireysel DNS kuruluma göre farklılık gösterebilir.

Örneğin, varsayılan BIND sunucu yapılandırması için DNS sunucunuzdaki/etc/Named.exe dosyasını düzenleyin ve aşağıdaki bölge bilgilerini ekleyin.

```
zone "az.cloudsimple.io"
{
    type stub;
    masters { IP address of DNS servers; };
    file "slaves/cloudsimple.io.db";
};
```

### <a name="create-a-zone-on-a-microsoft-windows-dns-server"></a>Microsoft Windows DNS sunucusunda bölge oluşturma

1. DNS sunucusuna sağ tıklayın ve **Yeni bölge**' yi seçin. 
  
    ![Yeni bölge](media/DNS01.png)
2. **Saplama bölgesi** ' ni seçin ve **İleri**' ye tıklayın.

    ![Yeni bölge](media/DNS02.png)
3. Ortamınıza bağlı olarak uygun seçeneği belirleyin ve **İleri**' ye tıklayın.

    ![Yeni bölge](media/DNS03.png)
4. **İleri arama bölgesi** ' ni seçin ve **İleri**' ye tıklayın.

    ![Yeni bölge](media/DNS01.png)
5. Bölge adını girin ve **İleri**' ye tıklayın.

    ![Yeni bölge](media/DNS05.png)
6. AVS portalından edindiğiniz AVS özel bulutunuz için DNS sunucularının IP adreslerini girin.

    ![Yeni bölge](media/DNS06.png)
7. Sihirbaz kurulumunu gerçekleştirmek için gerektiğinde **İleri** ' ye tıklayın.

## <a name="create-a-conditional-forwarder"></a>Koşullu iletici oluşturma

Koşullu iletici, tüm DNS ad çözümleme isteklerini belirtilen sunucuya iletir. Bu kurulumla, tüm *. cloudsimple.io istekleri, AVS özel bulutu 'nda bulunan DNS sunucularına iletilir. Aşağıdaki örneklerde, farklı türlerde DNS sunucularında ileticilerin nasıl ayarlanacağı gösterilmektedir.

### <a name="create-a-conditional-forwarder-on-a-bind-dns-server"></a>BIND DNS sunucusunda koşullu iletici oluşturma

Belirli dosya ve yapılandırılacak parametreler, bireysel DNS kuruluma göre farklılık gösterebilir.

Örneğin, varsayılan BIND sunucu yapılandırması için DNS sunucunuzdaki/etc/Named.exe dosyasını düzenleyin ve aşağıdaki koşullu iletme bilgilerini ekleyin.

```
zone "az.cloudsimple.io" {
    type forward;
    forwarders { IP address of DNS servers; };
};
```

### <a name="create-a-conditional-forwarder-on-a-microsoft-windows-dns-server"></a>Microsoft Windows DNS sunucusunda koşullu iletici oluşturma

1. DNS sunucusunda DNS Yöneticisi 'Ni açın.
2. **Koşullu ileticiler** ' e sağ tıklayın ve yeni bir koşullu iletici ekleme seçeneğini belirleyin.

    ![Koşullu Iletici 1 Windows DNS](media/DNS08.png)
3. DNS etki alanını ve AVS özel bulutundaki DNS sunucularının IP adresini girip **Tamam**' a tıklayın.
