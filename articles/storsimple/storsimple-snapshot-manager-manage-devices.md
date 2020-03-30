---
title: StorSimple Snapshot Manager ile cihazları yönetme | Microsoft Dokümanlar
description: StorSimple aygıtlarını bağlamak ve yönetmek için StorSimple Snapshot Manager MMC snap-in'in nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: twooley
manager: timlt
editor: ''
ms.assetid: 966ecbe3-a7fa-4752-825f-6694dd949946
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: twooley
ms.openlocfilehash: a037ddc5a43b288349b17dc7d954991a2c634aa3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254696"
---
# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>StorSimple aygıtlarını bağlamak ve yönetmek için StorSimple Snapshot Manager'ı kullanın
## <a name="overview"></a>Genel Bakış
Alınan StorSimple aygıt verilerini doğrulamak ve bağlı depolama aygıtlarını yenilemek için StorSimple Snapshot Manager **Scope** bölmesinde düğümler kullanabilirsiniz. Ayrıca, **Aygıtlar** düğüm'üne tıkladığınızda, **Sonuçlar** bölmesinde bağlı aygıtların ve karşılık gelen durum bilgilerinin listesini görebilirsiniz.

![Bağlı cihazlar](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Şekil 1: StorSimple Snapshot Manager bağlı cihaz** 

**Görünüm** seçimlerinize bağlı olarak, **Sonuçlar** bölmesi her aygıt hakkında aşağıdaki bilgileri gösterir. (Görünümü yapılandırma hakkında daha fazla bilgi için [Görünüm menüsüne](storsimple-use-snapshot-manager.md#view-menu)gidin.

| Sonuçlar sütunu | Açıklama |
|:--- |:--- |
| Adı |Azure klasik portalında yapılandırılan aygıtın adı |
| Model |Cihazın model numarası |
| Sürüm |Aygıta yüklenen yazılımın sürümü |
| Durum |Cihazın kullanılabilir olup olmadığı |
| Son Senkronize |Aygıtın en son eşitlendiği tarih ve saat |
| Seri No |Aygıtın seri numarası |

**Kapsam** bölmesindeki **Aygıtlar** düğümünü sağ tıklarsanız, aşağıdaki eylemlerden birini seçebilirsiniz:

* Aygıt ekleme veya değiştirme
* Aygıtı bağlama ve içeri aktarmayı doğrulama
* Bağlı cihazları yenileyin

**Cihazlar** düğümünü tıklatıp **Sonuçlar** bölmesinde bir aygıt adını sağ tıklarsanız, aşağıdaki eylemlerden birini seçebilirsiniz:

* Aygıtın kimliğini doğrulatın
* Cihaz ayrıntılarını görüntüleme
* Cihazı yenile
* Aygıt yapılandırması silme
* Aygıt parolası değiştirme

> [!NOTE]
> Tüm bu eylemler **Eylemler** bölmesinde de kullanılabilir.


Bu öğretici, aygıtları bağlamak ve yönetmek ve aşağıdaki görevleri gerçekleştirmek için StorSimple Snapshot Manager'ın nasıl kullanılacağını açıklar:

* Aygıt ekleme veya değiştirme
* Aygıtı bağlama ve içeri aktarmayı doğrulama
* Bağlı cihazları yenileyin
* Aygıtın kimliğini doğrulatın
* Cihaz ayrıntılarını görüntüleme
* Tek bir aygıtı yenileme
* Aygıt yapılandırması silme
* Süresi dolmuş aygıt parolası değiştirme
* Başarısız bir aygıtı değiştirme

> [!NOTE]
> StorSimple Snapshot Manager arabirimini kullanma hakkında genel bilgi için [StorSimple Snapshot Manager kullanıcı arabirimine](storsimple-use-snapshot-manager.md)gidin.


## <a name="add-or-replace-a-device"></a>Aygıt ekleme veya değiştirme
StorSimple aygıtı eklemek veya değiştirmek için aşağıdaki yordamı kullanın.

#### <a name="to-add-or-replace-a-device"></a>Aygıt eklemek veya değiştirmek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, **Aygıtlar** düğümüne sağ tıklayın ve ardından **aygıtı yapılandır'ı**tıklatın. Aygıt **yapvekuralı** iletişim kutusu görüntülenir.
   
    ![StorSimple aygıtını yapılandırma](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 
3. **Aygıt** açılır kutusunda, aygıtın veya sanal aygıtın IP adresini seçin. 
4. **Parola** metin kutusuna, Aygıt için oluşturduğunuz StorSimple Snapshot Manager parolasını Azure klasik portalına yazın. **Tamam**'a tıklayın. StorSimple Snapshot Manager, tanımladığınız aygıtı arar. 
   
   * Aygıt kullanılabilirse, StorSimple Snapshot Manager bir bağlantı ekler.
   * Aygıt herhangi bir nedenle kullanılamıyorsa, StorSimple Snapshot Manager bir hata iletisi döndürür. Hata iletisini kapatmak için **Tamam'ı** tıklatın ve ardından **Aygıt Yapılaşı** iletişim kutusunu kapatmak için **İptal'i** tıklatın.

## <a name="connect-a-device-and-verify-imports"></a>Aygıtı bağlama ve içeri aktarmayı doğrulama
StorSimple aygıtını bağlamak ve ilişkili yedeklemeleri olan varolan birim gruplarının içe aktarıldığını doğrulamak için aşağıdaki yordamı kullanın.

#### <a name="to-connect-a-device-and-verify-imports"></a>Bir aygıtı bağlamak ve içeri aktarmayı doğrulamak için
1. Bir aygıtı StorSimple Snapshot Manager'a bağlamak için, aygıtı Ekle veya değiştirme yönergelerini izleyin. Bir aygıta bağlandığında, StorSimple Snapshot Manager aşağıdaki gibi yanıt verir:
   
   * Aygıt herhangi bir nedenle kullanılamıyorsa, StorSimple Snapshot Manager bir hata iletisi döndürür. 
   
   * Aygıt kullanılabilirse, StorSimple Snapshot Manager bir bağlantı ekler. Aygıtı seçtiğinizde, **Sonuçlar** bölmesinde görünür ve durum alanı aygıtın **Kullanılabilir**olduğunu gösterir. StorSimple Snapshot Manager, birim gruplarının ilişkili yedeklemeleri olması koşuluyla aygıt için yapılandırılan tüm birim gruplarını içeri zabıta eder. Yedekleme ilkeleri alınmaz. İlişkili yedeklemeleri olmayan birim grupları alınmaz.
2. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
3. **Kapsam** bölmesindeki üst düğüme sağ tıklayın ve sonra **Içe Geçiş Ekranı'nı**tıklatın.
   
    ![Geçiş Aktarımları Ekranı'nı seçin](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 
4. Alınan birim gruplarıve yedeklemelerin durumunu gösteren **Geçiş Aktarımlar Ekranı** iletişim kutusu görüntülenir. **Tamam**'a tıklayın.

Birim grupları ve yedeklemeler başarıyla alındıktan sonra, oluşturduğunuz ve StorSimple Snapshot Manager ile yapılandırdığınız ve yapılandırdığınız birim gruplarını ve yedeklemeleri yöneteceğiniz gibi, bunları yönetmek için StorSimple Snapshot Manager'ı da kullanabilirsiniz. 

## <a name="refresh-connected-devices"></a>Bağlı cihazları yenileyin
Bağlı StorSimple aygıtlarını StorSimple Snapshot Manager ile senkronize etmek için aşağıdaki yordamı kullanın.

#### <a name="to-refresh-connected-devices"></a>Bağlı cihazları yenilemek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, **Cihazlar'ı**sağ tıklatın ve ardından **Cihazları Yenile'yi**tıklatın. Bu, bağlı aygıtları StorSimple Snapshot Manager ile senkronize eder, böylece son eklenenler de dahil olmak üzere ses gruplarını ve yedeklemeleri görüntüleyebilirsiniz. 
   
    ![StorSimple aygıtlarını yenileyin](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)

**Cihazları Yenile** eylemi, bağlı aygıtlardan yeni birim gruplarını ve ilişkili yedeklemeleri alır. **Birim** düğümü için kullanılabilen **Rescan birimleri** eyleminin aksine, **Aygıtları Yenile** yedekleme kayıt defterini geri yüklemez.

## <a name="authenticate-a-device"></a>Aygıtın kimliğini doğrulatın
StorSimple Snapshot Manager ile bir StorSimple aygıtının kimliğini doğrulamak için aşağıdaki yordamı kullanın.

#### <a name="to-authenticate-a-device"></a>Aygıtın kimliğini doğrulamak için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde **Aygıtlar'ı**tıklatın.
3. **Sonuçlar** bölmesinde, aygıtın adını sağ tıklatın ve sonra **Kimlik Doğrulaması'yı**tıklatın.
4. **Kimlik Doğrulama** iletişim kutusu görüntülenir. Aygıt parolasını yazın ve **ardından Tamam'ı**tıklatın.
   
    ![Kimlik doğrulama iletişim kutusu](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 

## <a name="view-device-details"></a>Cihaz ayrıntılarını görüntüleme
StorSimple aygıtının ayrıntılarını görüntülemek ve gerekirse aygıtı StorSimple Snapshot Manager ile yeniden senkronize etmek için aşağıdaki yordamı kullanın.

#### <a name="to-view-and-resynchronize-device-details"></a>Cihaz ayrıntılarını görüntülemek ve yeniden senkronize etmek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde **Aygıtlar'ı**tıklatın.
3. **Sonuçlar** bölmesinde, aygıtın adını sağ tıklatın ve sonra **Ayrıntılar'ı**tıklatın.

**4.Aygıt Ayrıntıları** iletişim kutusu görüntülenir. Bu kutu, ad, model, sürüm, seri numarası, durum, hedef iSCSI Kalifiye Adı (IQN) ve son eşitleme tarih ve saatini gösterir.

* Aygıtı eşitlemek için **Yeniden Eşitle'yi** tıklatın.
* İletişim kutusunu kapatmak için **Tamam** veya **İptal'i** tıklatın.
  
  ![Cihaz ayrıntıları](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 

## <a name="refresh-an-individual-device"></a>Tek bir aygıtı yenileme
StorSimple Snapshot Manager ile tek bir StorSimple aygıtını yeniden senkronize etmek için aşağıdaki yordamı kullanın.

#### <a name="to-refresh-a-device"></a>Cihazı yenilemek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın. 
2. **Kapsam** bölmesinde **Aygıtlar'ı**tıklatın. 
3. **Sonuçlar** bölmesinde, aygıtın adını sağ tıklatın ve ardından **Aygıtı Yenile'yi**tıklatın. Bu, aygıtı StorSimple Snapshot Manager ile eşitler.

## <a name="delete-a-device-configuration"></a>Aygıt yapılandırması silme
StorSimple Snapshot Manager'dan tek bir StorSimple aygıt yapılandırmasını silmek için aşağıdaki yordamı kullanın.

#### <a name="to-delete-a-device-configuration"></a>Aygıt yapılandırmasını silmek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde **Aygıtlar'ı**tıklatın. 
3. **Sonuçlar** bölmesinde, aygıtın adını sağ tıklatın ve sonra **Sil'i**tıklatın. 
4. Aşağıdaki ileti görüntülenir. Yapılandırmayı silmek için **Evet'i** tıklatın veya silme işlemini iptal etmek için **Hayır'ı** tıklatın.
   
    ![Cihaz yapılandırmasını silme](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Süresi dolmuş aygıt parolası değiştirme
StorSimple Snapshot Manager ile bir StorSimple aygıtının kimliğini doğrulamak için bir parola girmeniz gerekir. Aygıtı ayarlamak için Windows PowerShell arabirimini kullandığınızda bu parolayı yapılandırırsınız. Ancak, parolanın süresi dolabilir. Bu durumda, parolayı değiştirmek için Azure klasik portalını kullanabilirsiniz. Daha sonra, aygıt parola süresi dolmadan önce StorSimple Snapshot Manager'da yapılandırıldığından, aygıtı StorSimple Snapshot Manager'da yeniden kimlik doğrulamanız gerekir.

#### <a name="to-change-the-expired-password"></a>Süresi dolmuş parolayı değiştirmek için
1. Azure klasik portalında StorSimple Manager hizmetini başlatın.
2. Aygıt için > **Aygıtyapılandırma'yı** tıklatın. **Devices**
3. StorSimple Snapshot Manager bölümüne gidin. 14-15 karakterlik bir parola girin. Parolanın büyük harf, küçük harf, sayısal ve özel karakterlerin bir karışımını içerdiğinden emin olun.
4. Onaylamak için parolayı yeniden girin.
5. Sayfanın alt kısmındaki **Kaydet**’e tıklayın.

#### <a name="to-re-authenticate-the-device"></a>Aygıtı yeniden doğrulamak için
1. StorSimple Snapshot Manager'ı başlatın.
2. **Kapsam** bölmesinde **Aygıtlar'ı**tıklatın. Yapılandırılmış aygıtların listesi **Sonuçlar** bölmesinde görünür.
3. Aygıtı seçin, sağ tıklatın ve sonra **Kimlik Doğrulaması'yı**tıklatın.
4. Kimlik **Doğrulaması** penceresinde yeni parolayı girin.
5. Aygıtı seçin, sağ tıklayın ve **aygıtı yenile'yi**seçin. Bu, aygıtı StorSimple Snapshot Manager ile eşitler.

## <a name="replace-a-failed-device"></a>Başarısız bir aygıtı değiştirme
StorSimple aygıtı arızalanırsa ve bir bekleme (failover) aygıtı yla değiştirilirse, yeni aygıta bağlanmak ve ilişkili yedeklemeleri görüntülemek için aşağıdaki adımları kullanın.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Arızadan sonra yeni bir cihaza bağlanmak için
1. iSCSI bağlantısını yeni aygıta yeniden yapılandırın. Talimatlar için, [şirket içi StorSimple aygıtınızı dağıt'ta](storsimple-8000-deployment-walkthrough-u2.md)"Adım 7: Montaj, başlatma ve bir birimi biçimlendirme" başlıklı sayfaya gidin.

> [!NOTE]
> Yeni StorSimple aygıtı eskiyle aynı IP adresine sahipse, eski yapılandırmayı bağlayabilirsiniz.


1. Microsoft StorSimple Yönetim Hizmetini Durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi Panosu'nda, **Araçlar** menüsünde **Hizmetler'i**seçin.
   3. **Hizmetler** penceresinde, Microsoft **StorSimple Yönetim Hizmeti'ni**seçin.
   4. Microsoft **StorSimple Management Service**altında, sağ **bölmede, hizmeti durdur'u**tıklatın.
2. Eski aygıtla ilgili yapılandırma bilgilerini kaldırın:
   
   1. Dosya Gezgini'nde C:\ProgramData\Microsoft\StorSimple\BACatalog'a göz atın.
   2. BACatalog klasöründeki dosyaları silin.
3. Microsoft StorSimple Yönetim Hizmetini yeniden başlatın:
   
   1. Sunucu Yöneticisi Panosu'nda, **Araçlar** menüsünde **Hizmetler'i**seçin.
   2. **Hizmetler** penceresinde, Microsoft **StorSimple Yönetim Hizmeti'ni**seçin.
   3. Microsoft **StorSimple Management Service**altında, sağ bölmede, **hizmeti Yeniden Başlat'ı**tıklatın.
4. StorSimple Snapshot Manager'ı başlatın.
5. Yeni StorSimple aygıtını yapılandırmak için, Adım 2' deki adımları tamamlayın: [StorSimple aygıtını Deploy StorSimple Snapshot Manager'](storsimple-snapshot-manager-deployment.md)a bağlayın.
6. **Kapsam** bölmesindeki üst düzey düğümü sağ tıklatın (örnekte StorSimple Snapshot Manager) ve sonra **Geçiş İçi Aktarımlar Ekranı'nı**tıklatın. 
7. Alınan birim grupları ve yedeklerI StorSimple Snapshot Manager'da görünür olduğunda bir ileti görüntülenir. **Tamam**'a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-admin.md)nasıl kullanacağınızı öğrenin.
* [Birimleri görüntülemek ve yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-manage-volumes.md)nasıl kullanacağınızı öğrenin.

