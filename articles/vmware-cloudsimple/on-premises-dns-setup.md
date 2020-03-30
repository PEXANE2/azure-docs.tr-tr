---
title: CloudSimple tarafından Azure VMware Çözümü - CloudSimple Private Cloud için DNS'yi yapılandırın
description: Şirket içi iş istasyonlarından CloudSimple Private Cloud'daki vCenter sunucusuna erişmek için DNS ad çözünürlüğünün nasıl ayarlanır olduğunu açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c2d69d21eb46d502a45c9df1dfaaa947d26ef7c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246116"
---
# <a name="configure-dns-for-name-resolution-for-private-cloud-vcenter-access-from-on-premises-workstations"></a>Şirket içi iş istasyonlarından Özel Bulut vCenter erişimi için ad çözümü için DNS'yi yapılandırın

Bir CloudSimple Private Cloud'daki vCenter sunucusuna şirket içi iş istasyonlarından erişmek için, vCenter sunucusunun ana bilgisayar adı ve IP adresiyle ele alınabilmesi için DNS adres çözünürlüğünü yapılandırmanız gerekir.

## <a name="obtain-the-ip-address-of-the-dns-server-for-your-private-cloud"></a>Özel Bulut'unuzun DNS sunucusunun IP adresini edinin

1. [CloudSimple portalında](access-cloudsimple-portal.md)oturum açın.

2. Özel **Kaynaklar** > **Bulutları'na** gidin ve bağlanmak istediğiniz Özel Bulut'u seçin.

3. **Temel Bilgiler**altında Özel Bulut'un **Özet** sayfasında, Private Cloud DNS sunucu IP adresini kopyalayın.

    ![Özel Bulut DNS sunucuları](media/private-cloud-dns-server.png)


DNS yapılandırması için bu seçeneklerden birini kullanın.

* [*.cloudsimple.io için DNS sunucusunda bir bölge oluşturma](#create-a-zone-on-a-microsoft-windows-dns-server)
* [*.cloudsimple.io çözmek için şirket içi DNS sunucunuzda koşullu bir iletme oluşturun](#create-a-conditional-forwarder)

## <a name="create-a-zone-on-the-dns-server-for-cloudsimpleio"></a>*.cloudsimple.io için DNS sunucusunda bir bölge oluşturma

Bir bölge saplama bölgesi olarak ayarlayabilir ve ad çözümlemesi için Özel Bulut'taki DNS sunucularını işaret edebilirsiniz. Bu bölümde, BIND DNS sunucusu veya Microsoft Windows DNS sunucusu kullanma hakkında bilgi verilmektedir.

### <a name="create-a-zone-on-a-bind-dns-server"></a>BIND DNS sunucusunda bölge oluşturma

Yapılandırmak için belirli dosya ve parametreler, bireysel DNS kurulumunuza bağlı olarak değişebilir.

Örneğin, varsayılan BIND sunucu yapılandırması için, DNS sunucunuzdaki /etc/named.conf dosyasını düzenleme ve aşağıdaki bölge bilgilerini ekleyin.

```
zone "az.cloudsimple.io"
{
    type stub;
    masters { IP address of DNS servers; };
    file "slaves/cloudsimple.io.db";
};
```

### <a name="create-a-zone-on-a-microsoft-windows-dns-server"></a>Microsoft Windows DNS sunucusunda bölge oluşturma

1. DNS sunucusuna sağ tıklayın ve **Yeni Bölge'yi**seçin. 
  
    ![Yeni Bölge](media/DNS01.png)
2. **Saplama Bölgesi'ni** seçin ve **İleri'yi**tıklatın.

    ![Yeni Bölge](media/DNS02.png)
3. Ortamınıza bağlı olarak uygun seçeneği seçin ve **İleri'yi**tıklatın.

    ![Yeni Bölge](media/DNS03.png)
4. **İleri arama bölgesini** seçin ve **İleri'yi**tıklatın.

    ![Yeni Bölge](media/DNS01.png)
5. Bölge adını girin ve **İleri'yi**tıklatın.

    ![Yeni Bölge](media/DNS05.png)
6. CloudSimple portalından elde ettiğiniz Özel Bulut için DNS sunucularının IP adreslerini girin.

    ![Yeni Bölge](media/DNS06.png)
7. Sihirbaz kurulumını tamamlamak için gerektiği gibi **İleri'yi** tıklatın.

## <a name="create-a-conditional-forwarder"></a>Koşullu iletme oluştur

Koşullu bir iletme, tüm DNS ad çözümleme isteklerini belirlenen sunucuya ileter. Bu kurulumla, *.cloudsimple.io'a yapılan tüm istekler Özel Bulut'ta bulunan DNS sunucularına iletilir. Aşağıdaki örnekler, farklı DNS sunucutürlerinde iletmelerin nasıl ayarlanacağı gösterilmektedir.

### <a name="create-a-conditional-forwarder-on-a-bind-dns-server"></a>BIND DNS sunucusunda koşullu iletme oluşturma

Yapılandırmak için belirli dosya ve parametreler, bireysel DNS kurulumunuza bağlı olarak değişebilir.

Örneğin, varsayılan BIND sunucu yapılandırması için, DNS sunucunuzdaki /etc/named.conf dosyasını düzenleme ve aşağıdaki koşullu iletme bilgilerini ekleyin.

```
zone "az.cloudsimple.io" {
    type forward;
    forwarders { IP address of DNS servers; };
};
```

### <a name="create-a-conditional-forwarder-on-a-microsoft-windows-dns-server"></a>Microsoft Windows DNS sunucusunda koşullu iletme oluşturma

1. DNS sunucusunda DNS Yöneticisi'ni açın.
2. **Koşullu İletmeciler'e** sağ tıklayın ve yeni bir koşullu iletme seçeneği ni seçin.

    ![Koşullu İletme 1 Windows DNS](media/DNS08.png)
3. Özel Bulut'a DNS etki alanını ve DNS sunucularının IP adresini girin ve **Tamam'ı**tıklatın.
