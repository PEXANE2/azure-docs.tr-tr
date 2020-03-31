---
title: 'Azure AD Connect: Önceki sürümden yükseltme | Microsoft Dokümanlar'
description: Yerinde yükseltme ve salıncak geçişi de dahil olmak üzere Azure Active Directory Connect'in en son sürümüne yükseltme nin farklı yöntemlerini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 31f084d8-2b89-478c-9079-76cf92e6618f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 04/08/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2a3e7373a8b0354a3d08debf944f2f77f1609382
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60347761"
---
# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Önceki bir sürümden en son sürüme yükseltme
Bu konu, Azure Etkin Dizin (Azure AD) Connect yüklemenizi en son sürüme yükseltmek için kullanabileceğiniz farklı yöntemleri açıklar. Azure AD Connect sürümlerinde kendinizi güncel tutmanızı öneririz. Önemli bir yapılandırma değişikliği [yaptığınızda, Swing geçiş](#swing-migration) bölümündeki adımları da kullanırsınız.

>[!NOTE]
> Şu anda Azure AD Connect'in herhangi bir sürümünden geçerli sürüme yükseltmek için desteklenir. DirSync veya ADSync'in yerinde yükseltmeleri desteklenmez ve bir salıncak geçişi gerekir.  DirSync'den yükseltme yapmak istiyorsanız, [Azure AD eşitleme aracından (DirSync)](how-to-dirsync-upgrade-get-started.md) veya [Salıncak geçiş](#swing-migration) bölümünden yükseltme bölümüne bakın.  </br>Uygulamada, son derece eski sürümdeki müşteriler Azure AD Connect ile doğrudan ilişkili olmayan sorunlarla karşılaşabilir. Birkaç yıldır üretimde olan sunucular, genellikle onlara uygulanan birkaç yamalar olmuştur ve bunların hepsi için hesap olabilir.  Genellikle, 12-18 ay içinde yükseltilmiş değil müşteriler bu en muhafazakar ve en az riskli seçenek olarak yerine bir salıncak yükseltme düşünmelisiniz.

DirSync'den yükseltme yapmak istiyorsanız, bunun yerine [Azure AD eşitleme aracından (DirSync) Yükseltme'ye](how-to-dirsync-upgrade-get-started.md) bakın.

Azure AD Connect'i yükseltmek için kullanabileceğiniz birkaç farklı strateji vardır.

| Yöntem | Açıklama |
| --- | --- |
| [Otomatik yükseltme](how-to-connect-install-automatic-upgrade.md) |Bu, ekspres yüklemesi olan müşteriler için en kolay yöntemdir. |
| [Yerinde yükseltme](#in-place-upgrade) |Tek bir sunucunuz varsa, yüklemeyi aynı sunucuda yerinde yükseltebilirsiniz. |
| [Swing geçişi](#swing-migration) |İki sunucuyla, sunuculardan birini yeni sürüm veya yapılandırmayla hazırlayabilir ve hazır olduğunuzda etkin sunucuyu değiştirebilirsiniz. |

İzin bilgileri [için yükseltme için gereken izinlere](reference-connect-accounts-permissions.md#upgrade)bakın.

> [!NOTE]
> Yeni Azure AD Connect sunucunuzun Azure AD'deki değişiklikleri eşitlemeye başlamasını sağladıktan sonra, DirSync veya Azure AD Sync'i kullanmaya geri dönmemelisiniz. Azure AD Connect'ten DirSync ve Azure AD Eşitleme gibi eski istemcilere indirgeme desteklenmez ve Azure AD'de veri kaybı gibi sorunlara yol açabilir.

## <a name="in-place-upgrade"></a>Yerinde yükseltme
Yerinde yükseltme, Azure AD Eşitlemi veya Azure AD Connect'ten taşınmaya yönelik çalışır. DirSync'den veya Forefront Identity Manager (FIM) + Azure AD Bağlayıcısı ile bir çözüm için çalışmaz.

Bu yöntem, tek bir sunucunuz ve yaklaşık 100.000'den az nesneniz olduğunda tercih edilir. Kutudan çıkma eşitleme kurallarında herhangi bir değişiklik varsa, yükseltmeden sonra tam bir alma ve tam eşitleme oluşur. Bu yöntem, yeni yapılandırmanın sistemdeki tüm varolan nesnelere uygulanmasını sağlar. Bu çalışma, eşitleme altyapısı kapsamında olan nesnelerin sayısına bağlı olarak birkaç saat sürebilir. Normal delta eşitleme zamanlayıcısı (varsayılan olarak her 30 dakikada bir eşitler) askıya alınır, ancak parola eşitleme devam eder. Bir hafta sonu boyunca yerinde yükseltme yapmayı düşünebilirsiniz. Yeni Azure AD Connect sürümüyle kutudan çıkma yapılandırmada değişiklik yoksa, bunun yerine normal bir delta alma/eşitleme başlar.  
![Yerinde yükseltme](./media/how-to-upgrade-previous-version/inplaceupgrade.png)

Kutudan çıkan eşitleme kurallarında değişiklik yaptıysanız, bu kurallar yükseltmedeki varsayılan yapılandırmaya geri ayarlanır. Yapılandırmanızın yükseltmeler arasında tutulduğundan emin olmak için, [varsayılan yapılandırmayı değiştirmek için en iyi uygulamalarda](how-to-connect-sync-best-practices-changing-default-configuration.md)açıklandığı gibi değişiklik yaptığınızdan emin olun.

Yerinde yükseltme sırasında, yükseltme tamamlandıktan sonra yürütülmesi için belirli eşitleme etkinlikleri (Tam Alma adımı ve Tam Eşitleme adımı dahil) gerektiren değişiklikler olabilir. Bu tür etkinlikleri ertelemek için, [yükseltmeden sonra tam eşitleme nasıl ertelenirene](#how-to-defer-full-synchronization-after-upgrade)bakın.

Azure AD Connect'i standart olmayan bağlayıcıyla (örneğin, Genel LDAP Bağlayıcısı ve Genel SQL Bağlayıcısı) kullanıyorsanız, yerinde yükseltmeden sonra [Eşitleme Hizmet Yöneticisi'nde](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-connectors) ilgili bağlayıcı yapılandırmasını yenilemeniz gerekir. Bağlayıcı yapılandırmasının nasıl yenileyiş nedeni hakkında ayrıntılı bilgi için makale bölümüne bakın [Bağlayıcı Sürüm Yayın Geçmişi - Sorun Giderme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history#troubleshooting). Yapılandırmayı yenilemezseniz, dışa aktarma ve dışa aktarma adımları bağlayıcı için doğru çalışmaz. Uygulama olay günlüğünde *"AAD Konektörü yapılandırmasında montaj sürümü ("X.X.XXX. X") gerçek sürümden daha erkendir ("X.X.XXX. X") "C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll".*

## <a name="swing-migration"></a>Swing geçişi
Karmaşık bir dağıtım veya çok sayıda nesneniz varsa, canlı sistemde yerinde yükseltme yapmak pratik olmayabilir. Bazı müşteriler için bu işlem birden çok gün sürebilir ve bu süre içinde delta değişiklikleri işlenmez. Yapılandırmanızda önemli değişiklikler yapmayı planladığınızda ve buluta itilmeden önce bunları denemek istediğinizde de bu yöntemi kullanabilirsiniz.

Bu senaryolar için önerilen yöntem bir salıncak geçiş kullanmaktır. (En az) iki sunucuya ihtiyacınız vardır-- biri etkin sunucu ve bir hazırlama sunucusu. Etkin sunucu (aşağıdaki resimde düz mavi çizgilerle gösterilir) etkin üretim yükünden sorumludur. Evreleme sunucusu (kesikli mor çizgilerle gösterilir) yeni sürüm veya yapılandırma ile hazırlanır. Tamamen hazır olduğunda, bu sunucu etkin hale getirilir. Şimdi eski sürümü veya yapılandırmayüklü olan önceki etkin sunucu, evreleme sunucusuna yapılır ve yükseltilir.

İki sunucu farklı sürümleri kullanabilir. Örneğin, devre dışı bırakmayı planladığınız etkin sunucu Azure AD Eşitlemeyi'ni kullanabilir ve yeni evreleme sunucusu Azure AD Connect'i kullanabilir. Yeni bir yapılandırma geliştirmek için salıncak geçişini kullanıyorsanız, iki sunucuda aynı sürümlere sahip olmak iyi bir fikirdir.  
![Evreleme sunucusu](./media/how-to-upgrade-previous-version/stagingserver1.png)

> [!NOTE]
> Bazı müşteriler bu senaryo için üç veya dört sunucuya sahip olmayı tercih ediyor. Evreleme sunucusu yükseltildiğinde, [olağanüstü durum kurtarma](how-to-connect-sync-staging-server.md#disaster-recovery)için yedek sunucunuz yoktur. Üç veya dört sunucuyla, her zaman devralmaya hazır bir hazırlama sunucusu olmasını sağlayan yeni sürümle bir birincil/bekleme sunucusu kümesi hazırlayabilirsiniz.

Bu adımlar, Azure AD Eşitleme'den veya FIM + Azure AD Bağlayıcısı ile bir çözümden de geçmeye çalışır. Bu adımlar DirSync için çalışmaz, ancak DirSync için adımlarla aynı salıncak geçiş yöntemi (paralel dağıtım olarak da adlandırılır) [Yükseltme Azure Active Directory sync (DirSync)](how-to-dirsync-upgrade-get-started.md)'de dir.

### <a name="use-a-swing-migration-to-upgrade"></a>Yükseltmek için salıncak geçişi kullanın
1. Azure AD Connect'i her iki sunucuda da kullanıyorsanız ve yalnızca yapılandırma değişikliği yapmayı planlıyorsanız, etkin sunucunuzun ve evreleme sunucunuzun her ikisinin de aynı sürümü kullandığından emin olun. Bu daha sonra farklılıkları karşılaştırmak kolaylaştırır. Azure AD Eşitleme'den yükseltme yapıyorsunuz, bu sunucuların farklı sürümleri vardır. Azure AD Connect'in eski bir sürümünden yükseltme yapıyorsunuz, aynı sürümü kullanan iki sunucuyla başlamak iyi bir fikirdir, ancak gerekli değildir.
2. Özel bir yapılandırma yaptıysanız ve hazırlama sunucunuzda bu yapılandırma yoksa, [etkin sunucudan evreleme sunucusuna özel bir yapılandırma yı taşıyın](#move-a-custom-configuration-from-the-active-server-to-the-staging-server)altındaki adımları izleyin.
3. Azure AD Connect'in önceki bir sürümünden yükseltme yapıyorsunuz, evreleme sunucusunu en son sürüme yükseltin. Azure AD Sync'den taşınıyorsanız, azure AD Connect'i hazırlama sunucunuza yükleyin.
4. Eşitleme motorunun evreleme sunucunuzda tam alma ve tam eşitleme çalışmasına izin verin.
5. Yeni yapılandırmanın [sunucunun yapılandırmasını doğrula'daki](how-to-connect-sync-staging-server.md#verify-the-configuration-of-a-server)"Doğrula" (Doğrula) (Doğrula) ()'deki "Verify" (Doğrula) ( Bir şey beklendiği gibi değilse, adımları izleyerek, içe aktarma ve eşitleme çalıştırın ve verileri iyi görünene kadar doğrulayın.
6. Hazırlama sunucusunu etkin sunucu olarak değiştirin. Bu, [bir sunucunun yapılandırmasını doğrula'daki](how-to-connect-sync-staging-server.md#verify-the-configuration-of-a-server)son adım "Etkin sunucu yut" dır.
7. Azure AD Connect'i yükseltiyorsanız, şu anda evreleme modunda olan sunucuyu en son sürüme yükseltin. Verileri ve yapılandırmayı yükseltmeyi almak için önceki yle aynı adımları izleyin. Azure AD Eşitleme'den yükselttiyseniz, artık eski sunucunuzu kapatıp devre dışı bırakabilirsiniz.

### <a name="move-a-custom-configuration-from-the-active-server-to-the-staging-server"></a>Etkin sunucudan evreleme sunucusuna özel bir yapılandırma taşıma
Etkin sunucuda yapılandırma değişiklikleri yaptıysanız, aynı değişikliklerin evreleme sunucusuna da uygulandığından emin olmanız gerekir. Bu taşımaya yardımcı olmak için [Azure AD Connect yapılandırma belgeleyicisini](https://github.com/Microsoft/AADConnectConfigDocumenter)kullanabilirsiniz.

PowerShell'i kullanarak oluşturduğunuz özel eşitleme kurallarını taşıyabilirsiniz. Diğer değişiklikleri her iki sistemde de aynı şekilde uygulamanız gerekir ve değişiklikleri geçiremezsiniz. [Yapılandırma belgeleyici,](https://github.com/Microsoft/AADConnectConfigDocumenter) aynı olduğundan emin olmak için iki sistemi karşılaştırmanıza yardımcı olabilir. Araç, bu bölümde bulunan adımları otomatikleştirmekte de yardımcı olabilir.

Aşağıdaki şeyleri her iki sunucuda da aynı şekilde yapılandırmanız gerekir:

* Aynı ormanlara bağlantı
* Herhangi bir etki alanı ve OU filtreleme
* Parola eşitleme ve parola yazma gibi aynı isteğe bağlı özellikler

**Özel eşitleme kurallarını taşıma**  
Özel eşitleme kurallarını taşımak için aşağıdakileri yapın:

1. Etkin sunucunuzda **Eşitleme Kuralları** Düzenleyicisi'ni açın.
2. Özel bir kural seçin. **Dışarı aktar**'a tıklayın. Bu bir Not Defteri penceresi getiriyor. Geçici dosyayı PS1 uzantısı ile kaydedin. Bu bir PowerShell komut dosyası yapar. PS1 dosyasını evreleme sunucusuna kopyalayın.  
   ![Eşitleme kuralı dışa aktarma](./media/how-to-upgrade-previous-version/exportrule.png)
3. Bağlayıcı GUID evreleme sunucusunda farklıdır ve bunu değiştirmeniz gerekir. GUID'i almak için **Eşitleme Kuralları**Düzenleyicisi'ni başlatın, aynı bağlı sistemi temsil eden kutu dan sıcağa giden kurallardan birini seçin ve **Dışa**Aktar'ı tıklatın. PS1 dosyanızdaki GUID'i evreleme sunucusundaki GUID ile değiştirin.
4. PowerShell isteminde PS1 dosyasını çalıştırın. Bu, evreleme sunucusunda özel eşitleme kuralını oluşturur.
5. Tüm özel kurallarınız için bunu tekrarlayın.

## <a name="how-to-defer-full-synchronization-after-upgrade"></a>Yükseltmeden sonra tam eşitleme nasıl ertelenir
Yerinde yükseltme sırasında, yürütülmesi için belirli eşitleme etkinlikleri (Tam Alma adımı ve Tam Eşitleme adımı dahil) gerektiren değişiklikler getirilebilir. Örneğin, bağlayıcı şeması değişiklikleri **tam alma** adımı gerektirir ve kutudan çıkma eşitleme kuralı değişiklikleri, etkilenen bağlayıcılarda yürütülmesi için **tam eşitleme** adımı gerektirir. Yükseltme sırasında Azure AD Connect hangi eşitleme etkinliklerinin gerekli olduğunu belirler ve bunları *geçersiz kılar*olarak kaydeder. Aşağıdaki eşitleme döngüsünde, eşitleme zamanlayıcısı bu geçersiz kılmaları alır ve yürütür. Geçersiz kılma başarıyla yürütüldükten sonra kaldırılır.

Bu geçersiz kılmaların yükseltmeden hemen sonra gerçekleşmesini istemediğiniz durumlar olabilir. Örneğin, çok sayıda eşitlenmiş nesneniz vardır ve bu eşitleme adımlarının iş saatlerinden sonra gerçekleşmesini istersiniz. Bu geçersiz kılmaları kaldırmak için:

1. Yükseltme sırasında, yapılandırma **tamamlandığında eşitleme işlemini başlat**seçeneğini **işaretleyin.** Bu, eşitleme zamanlayıcısıdevre devre dışı kalır ve geçersiz kılmalar kaldırılmadan önce eşitleme döngüsünün otomatik olarak gerçekleşmesini engeller.

   ![Devre Dışı KaldıktanSonraYükseltme](./media/how-to-upgrade-previous-version/disablefullsync01.png)

2. Yükseltme tamamlandıktan sonra, hangi geçersiz kılmaların eklendiğini öğrenmek için aşağıdaki cmdlet'i çalıştırın:`Get-ADSyncSchedulerConnectorOverride | fl`

   >[!NOTE]
   > Geçersiz kılmalar bağlayıcıya özgüdir. Aşağıdaki örnekte, tam alma adımı ve Tam Eşitleme adımı hem şirket içi AD Bağlayıcısı'na hem de Azure AD Bağlayıcısı'na eklenmiştir.

   ![Devre Dışı KaldıktanSonraYükseltme](./media/how-to-upgrade-previous-version/disablefullsync02.png)

3. Eklenen varolan geçersiz kılmaları not edin.
   
4. Rasgele bir bağlayıcıda hem tam alma hem de tam eşitleme için geçersiz kılmaları kaldırmak için aşağıdaki cmdlet'i çalıştırın:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid-of-ConnectorIdentifier> -FullImportRequired $false -FullSyncRequired $false`

   Tüm bağlayıcılar üzerindeki geçersiz kılmaları kaldırmak için aşağıdaki PowerShell komut dosyasını çalıştırın:

   ```
   foreach ($connectorOverride in Get-ADSyncSchedulerConnectorOverride)
   {
       Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier $connectorOverride.ConnectorIdentifier.Guid -FullSyncRequired $false -FullImportRequired $false
   }
   ```

5. Zamanlayıcıyı devam ettirmek için aşağıdaki cmdlet'i çalıştırın:`Set-ADSyncScheduler -SyncCycleEnabled $true`

   >[!IMPORTANT]
   > En kısa zamanda gerekli eşitleme adımlarını yürütmeyi unutmayın. Bu adımları Eşitleme Hizmet Yöneticisi'ni kullanarak el ile yürütebilir veya Set-ADSyncSchedulerConnectorOverride cmdlet'i kullanarak geçersiz kılmaları geri ekleyebilirsiniz.

Rasgele bir bağlayıcıüzerinde hem tam alma hem de tam eşitleme için geçersiz kılmaları eklemek için aşağıdaki cmdlet'i çalıştırın:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid> -FullImportRequired $true -FullSyncRequired $true`

## <a name="troubleshooting"></a>Sorun giderme
Aşağıdaki bölümde, Azure AD Connect'i yükseltme sorunuyla karşılaştığınız da kullanabileceğiniz sorun giderme ve bilgiler yer almaktadır.

### <a name="azure-active-directory-connector-missing-error-during-azure-ad-connect-upgrade"></a>Azure AD Connect yükseltmesi sırasında Azure Active Directory bağlayıcısı eksik hata

Azure AD Connect'i önceki bir sürümden yükselttiğinde, yükseltmenin başında aşağıdaki hataya ulaşabilirsiniz 

![Hata](./media/how-to-upgrade-previous-version/error1.png)

Bu hata, tanımlayıcılı B891884f-051e-4a83-95af-2544101c9083 tanımlayıcılı Azure Active Directory bağlayıcısının geçerli Azure AD Connect yapılandırmasında bulunmaması nedeniyle olur. Durumun böyle olduğunu doğrulamak için bir PowerShell penceresi açın, Cmdlet'i çalıştırın`Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083`

```
PS C:\> Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083
Get-ADSyncConnector : Operation failed because the specified MA could not be found.
At line:1 char:1
+ Get-ADSyncConnector -Identifier b891884f-051e-4a83-95af-2544101c9083
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ReadError: (Microsoft.Ident...ConnectorCmdlet:GetADSyncConnectorCmdlet) [Get-ADSyncConne
   ctor], ConnectorNotFoundException
    + FullyQualifiedErrorId : Operation failed because the specified MA could not be found.,Microsoft.IdentityManageme
   nt.PowerShell.Cmdlet.GetADSyncConnectorCmdlet

```

PowerShell Cmdlet, **belirtilen MA'nın bulunamadığı**hatayı bildirir.

Bunun nedeni, geçerli Azure AD Connect yapılandırmasının yükseltme için desteklenmemesidir. 

Azure AD Connect'in daha yeni bir sürümünü yüklemek istiyorsanız: Azure AD Connect sihirbazını kapatın, varolan Azure AD Connect'i kaldırın ve yeni Azure AD Connect'in temiz bir yüklemesini gerçekleştirin.



## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)hakkında daha fazla bilgi edinin.
