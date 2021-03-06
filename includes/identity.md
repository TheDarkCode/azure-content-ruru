Управление удостоверениями так же важно в общедоступном облаке, как и в локальных средах. Azure поддерживает несколько технологий управления удостоверениями в облаке. К ним относятся следующие:

- Служба Windows Server Active Directory (обычно называемая просто AD) может работать в облаке на виртуальных машинах, созданных с помощью виртуальных машин Azure. Такой подход имеет смысл при использовании Azure для переноса локального центра обработки данных в облако.


- Azure Active Directory можно использовать для предоставления пользователям возможности единого входа в приложения [SaaS](https://azure.microsoft.com/overview/what-is-saas/). Эта технология используется, например, в Microsoft Office 365, а также может использоваться в приложениях, работающих на платформе Azure и на других облачных платформах.


- Облачные или локальные приложения могут использовать компонент Azure Active Directory Access Control, который поддерживает вход с помощью удостоверений Facebook, Google, Майкрософт и других поставщиков удостоверений.


В этой статье описываются все три варианта.

## Оглавление

- [Выполнение Windows Server Active Directory на виртуальных машинах](#adinvm)

- [Использование Azure Active Directory](#ad)

- [Использование службы контроля доступа Azure Active Directory](#ac)


## <a name="adinvm"></a>Выполнение Windows Server Active Directory на виртуальных машинах

Выполнение Windows Server AD на виртуальных машинах Azure очень похоже на выполнение в локальных средах. [На рисунке 1](#fig1) показан типичный пример того, как это выглядит.

![Azure Active Directory на виртуальной машине](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Рис. 1.Windows Server Active Directory может выполняться на виртуальных машинах Azure, подключенных к локальному центру обработки данных организации с помощью виртуальной сети Azure.

В показанном примере Windows Server AD выполняется на виртуальных машинах Azure, созданных на основе технологии IaaS. Эти виртуальные машины сгруппированы с другими машинами в одну виртуальную сеть, подключенную к локальному центру обработки данных с помощью компонента виртуальной сети Azure. Виртуальная сеть охватывает группу облачных виртуальных машин, которые взаимодействуют с локальной сетью через подключение к виртуальной частной сети (VPN). Таким образом, локальный центр обработки данных может работать с виртуальными машинами Azure как с обычной подсетью. Как показано на рисунке, на двух виртуальных машинах работают контроллеры домена Windows Server AD. На других виртуальных машинах в виртуальной сети могут выполняться приложения, например SharePoint; они также могут использоваться в иных целях, например, для разработки и тестирования. В локальном центре обработки данных также работают два контроллера домена Windows Server AD.

Существует несколько способов подключения контроллеров домена в облаке к локальным контроллерам домена.

- — Включить все контроллеры в один домен Active Directory.

- — Создать отдельные домены AD — локальный и облачный — и включить их в один лес.

- Создать отдельные леса AD — облачный и локальный — и подключить их, определив отношения доверия между лесами или с помощью служб федерации Windows Server Active Directory (AD FS), которые также могут выполняться на виртуальных машинах Azure.

Независимо от выбранного варианта, администратор должен убедиться, что запросы проверки подлинности от локальных пользователей поступают на облачные контроллеры домена, только когда это необходимо, поскольку связь с облаком очевидно будет более медленной, чем с локальными сетями. При подключении облачных и локальных контроллеров домена также следует учитывать трафик, генерируемый репликацией. Облачные контроллеры домена как правило выполняются на собственном сайте AD, что позволяет администратору запланировать периодичность выполнения репликации. При использовании Azure оплачивается только трафик, исходящий из центра обработки данных Azure, а входящий трафик не учитывается. Администратор может принять во внимание этот факт при составлении расписания репликации. Также следует отметить, что хотя Azure поддерживает собственную систему доменных имен (DNS), у этой службы нет компонентов, требуемых службой Active Directory (таких как поддержка динамических DNS и записей SRV). Поэтому для работы Windows Server AD на Azure необходимо настроить собственные DNS-серверы в облаке.

Выполнение Windows Server AD на виртуальных машинах Azure может иметь смысл в различных ситуациях. Ниже приведены некоторые примеры:

- — Если виртуальные машины Azure используются в качестве расширения локального центра обработки данных, в облаке могут выполняться приложения, которым локальные контроллеры домена необходимы для обработки таких компонентов, как запросы встроенной проверки подлинности Windows или запросы LDAP. Например, SharePoint часто взаимодействует с Active Directory. Поэтому, хотя ферма SharePoint может работать на Azure с использованием локального каталога, настройка контроллеров домена в облаке существенно повысит производительность. (Важно понимать, что это необязательно. Многие приложения могут успешно выполняться в облаке с использованием только локальных контроллеров домена).

- — Если удаленному филиалу не хватает ресурсов для работы собственных контроллеров домена. Сегодня пользователи в таких филиалах должны проходить проверку подлинности на компьютерах, расположенных на другом конце света, поэтому процедура входа выполняется медленно. Выполнение Active Directory на Azure в ближайшем центре обработки данных Майкрософт может ускорить этот процесс без установки дополнительных серверов в филиале.

- — Организация, использующая Azure для аварийного восстановления, может поддерживать в облаке небольшое количество активных виртуальных машин, включая контроллер домена. Его можно подготовить для развертывания сайта в случае сбоев в других расположениях.

Существуют и другие возможности. Например, необязательно подключать облачную службу Windows Server AD к локальному центру обработки данных. Если необходимо, чтобы ферма SharePoint обслуживала ограниченное множество пользователей, например, только пользователей с облачными удостоверениями, можно создать отдельный лес в Azure. Способ использования этой технологии зависит от целей. (Более подробное руководство по использованию Windows Server AD с Azure [см. здесь](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Использование Azure Active Directory

По мере роста популярности приложений SaaS возникает очевидный вопрос: какие службы каталогов должны использовать эти облачные приложения? У корпорации Майкрософт есть ответ на этот вопрос: Azure Active Directory.

Существует два основных варианта использования этой службы каталогов в облаке:

- — Люди и организации, использующие только приложения SaaS, могут использовать Azure Active Directory в качестве единой службы каталогов.

- — Организации, использующие Windows Server Active Directory, могут подключить свой локальный каталог к Azure Active Directory и использовать эту службу для предоставления своим пользователям единого входа в приложения SaaS.


[На рис. 2](#fig2) показан первый из этих вариантов, в котором Microsoft Azure Active Directory обеспечивает все необходимые возможности.

![Azure Active Directory на виртуальной машине](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Рис. 2. Azure Active Directory предоставляет пользователям организации возможность единого входа в приложения SaaS, включая Office 365.

Как показано на рисунке, Azure AD является мультитенантной службой. Это значит, что она может одновременно поддерживать множество организаций и хранить каталоги пользователей для каждой из них. В этом примере пользователь из организации A пытается получить доступ к приложению SaaS. Это приложение может быть частью Office 365, например SharePoint Online, или любым другим — приложения сторонних разработчиков также могут использовать эту технологию. Поскольку Azure AD поддерживает протокол SAML 2.0, приложению требуется лишь возможность взаимодействия с использованием этого промышленного стандарта. (На самом деле приложения, использующие Azure AD, могут выполняться в любом центре обработки данных, а не только в центре обработки данных Azure).

Процесс начинается, когда пользователь получает доступ к приложению SaaS (шаг 1). Чтобы использовать это приложение, пользователь должен представить маркер, выданный службой Azure AD.

Этот маркер содержит данные, идентифицирующие пользователя, а также цифровую подпись Azure AD. Чтобы получить маркер, пользователь проходит проверку подлинности в Azure AD, вводя имя пользователя и пароль (шаг 2). После этого Azure AD выдает пользователю маркер (шаг 3).

Этот маркер отправляется в приложение SaaS (шаг 4), которое проверяет его подпись и использует его содержимое (шаг 5). Как правило, приложение использует сведения об удостоверении, содержащиеся в маркере, чтобы решить, к каким данным предоставить доступ пользователю, а также в иных целях.

Если приложению требуются дополнительные сведения о пользователе, которых нет в маркере, оно может запросить эти данные непосредственно из Azure AD с помощью Azure AD Graph API (шаг 6). В первоначальной версии Azure AD схема каталога довольно проста: она содержит сведения о пользователях, группах и связях между ними. Приложения могут использовать эти сведения, чтобы определить связи между пользователями. Например, предположим, что приложению нужно определить, кто руководитель данного пользователя, чтобы решить, разрешен ли ему доступ к некоторым данным. Оно может получить эти сведения, запросив Azure AD через Graph API.

Graph API использует обычный протокол RESTful, поэтому его можно использовать практически из любых клиентов, включая мобильные устройства. Этот API также поддерживает расширения, определенные посредством OData, что позволяет использовать дополнительные средства, например, языки запросов, для упрощения доступа клиентов к данным. (Дополнительные сведения по OData см. в документе [Введение в OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf)). Поскольку Graph API можно использовать для получения сведений о связях между пользователями, приложения могут определить социальный граф, встроенный в схему Azure AD для конкретной организации (именно поэтому интерфейс называется Graph API). Для прохождения проверки подлинности в Azure AD и отправки запросов Graph API приложения используют OAuth 2.0.

Если организация не использует Windows Server Active Directory (т. е. у нее нет локальных серверов или доменов) и полагается только на облачные приложения, использующие Azure AD, то этот облачный каталог может предоставить пользователям организации единый вход для всех приложений. Хотя этот сценарий становится все более распространенным, большинство организаций по-прежнему используют локальные домены, созданные с помощью Windows Server Active Directory. В этом случае Azure AD также имеет важное значение, как показано на [рис. 3](#fig3).

![Azure Active Directory на виртуальной машине](./media/identity/identity_03_AD.png) <a id="fig3"></a>Рис. 3. Организация может установить отношения федерации между Windows Server Active Directory и Azure Active Directory, чтобы предоставить своим пользователям единый вход в приложения SaaS.

В этом сценарии пользователь из организации B хочет получить доступ к приложению SaaS. Перед этим администраторы каталога организации должны установить федеративные отношения с Azure AD с помощью AD FS, как показано на рисунке. Администраторы также должны настроить синхронизацию данных между локальной службой Windows Server AD и Azure AD. При этом произойдет автоматическое копирование сведений о пользователях и группах из локального каталога в Azure AD. Обратите внимание, что это позволяет фактически перенести локальный каталог организации в облако. Такое объединение Windows Server AD и Azure AD предоставляет организации службу каталогов, которой можно управлять как одним объектом, и в то же время хранить данные как локально, так и в облаке.

Чтобы использовать Azure AD, пользователь выполняет вход в локальный домен Active Directory, как обычно (шаг 1). Когда пользователь пытается получить доступ к приложению SaaS (шаг 2), в результате действия процесса федерации Azure AD выдает ему маркер для этого приложения (шаг 3). (Дополнительные сведения о действии федерации см. в документе [Удостоверение на основе утверждений для Windows: технологии и сценарии](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Как и раньше, этот маркер содержит данные, идентифицирующие пользователя, а также цифровую подпись Azure AD. Этот маркер отправляется в приложение SaaS (шаг 4), которое проверяет его подпись и использует его содержимое (шаг 5). Как и в предыдущем сценарии, приложение SaaS может использовать Graph API для получения дополнительных сведений о пользователе, если это необходимо (шаг 6).

Сегодня Azure AD не может полностью заменить локальную службу Windows Server AD. Как отмечалось выше, облачный каталог имеет гораздо более простую схему, а также не поддерживает групповые политики, не может хранить сведения о машинах и не поддерживает LDAP. (Фактически, машину Windows нельзя настроить так, чтобы пользователи могли входить в нее только с помощью Azure AD — такой сценарий не поддерживается). Первоначальные цели Azure AD — предоставить пользователям предприятия доступ к облачным приложениям без поддержания отдельного входа и освободить администраторов локального каталога от ручной синхронизации локального каталога с каждым приложением SaaS, используемым организацией. Однако со временем эта облачная служба каталогов будет поддерживать более широкий диапазон сценариев.

## <a name="ac"></a>Использование службы контроля доступа Azure Active Directory

Облачные технологии управления удостоверениями можно использовать для решения целого ряда задач. Например, Azure Active Directory может предоставить пользователям организации единый вход для нескольких приложений SaaS. Однако облачные технологии идентификации можно использовать и в других целях.

Например, предположим, что пользователи должны входить в приложение с помощью маркеров, выдаваемых несколькими *поставщиками удостоверений (IdPs)*. Сегодня существует множество поставщиков удостоверений, таких как Facebook, Google, Майкрософт и другие, и приложения часто позволяют пользователям использовать их удостоверения для входа. Зачем приложению поддерживать собственный список пользователей и паролей, если оно может использовать удостоверения, которые уже существуют? Использование существующих удостоверений облегчает жизнь как пользователям, которые могут запоминать меньше учетных данных, так и разработчикам приложений, которым не нужно предусматривать поддержку собственных списков имен пользователей и паролей.

Однако, хотя все поставщики удостоверений выдают маркеры, эти маркеры нестандартные — каждый поставщик использует собственный формат. Более того, данные в этих маркерах также нестандартные. Если приложение должно принимать маркеры, выдаваемые, к примеру, такими поставщиками как Facebook, Google и Майкрософт, то в нем должен быть предусмотрен код для обработки каждого из этих форматов.

Но так ли это необходимо? Почему бы не создать посредника, который генерирует единый формат маркера с общим представлением сведений об удостоверении? Такой подход облегчит жизнь разработчиков, которые создают приложения, так как им нужно будет обрабатывать только один вид маркеров. Именно это и делает Azure Active Directory Access Control, предоставляя облачного посредника для работы с различными маркерами. [На рисунке 4](#fig4) показано, как это работает

![Azure Active Directory на виртуальной машине](./media/identity/identity_04_IdentityProviders.png) <a id="fig4"></a>Рисунок 4. Azure Active Directory Access Control упрощает для приложений прием маркеров удостоверений, выпускаемых различными поставщиками.

Процесс начинается, когда пользователь пытается получить доступ к приложению из браузера. Приложение перенаправляет пользователя на выбранного пользователем поставщика удостоверений (которому приложение также доверяет). Пользователь проходит проверку подлинности у этого поставщика, вводя свои учетные данные (шаг 1), и поставщик возвращает маркер, содержащий сведения о пользователе (шаг 2).

Как показано на рисунке, Access Control поддерживает целый ряд облачных поставщиков удостоверений, включая учетные записи, созданные службами Google, Yahoo, Facebook, Майкрософт (ранее известные как Windows Live ID) и любыми поставщиками OpenID. Поддерживаются также удостоверения, созданные с помощью Azure Active Directory, и, посредством федерации с AD FS, с помощью Windows Server Active Directory. Целью является поддержка самых распространенных в настоящее время удостоверений, выпускаемых поставщиками в облаке или локально.

После того как браузер пользователя получил маркер от выбранного поставщика удостоверений, этот маркер отправляется в Access Control (шаг 3). Access Control проверяет маркер, убеждается, что он действительно выдан данным поставщиком, и создает новый маркер в соответствии с правилами, определенными для данного приложения. Подобно Azure Active Directory, Access Control является мультитенантной службой, однако ее клиентами являются приложения, а не организации. Как показано на рисунке, каждое приложение может получить собственное пространство имен и определить правила авторизации и другие правила.

Эти правила позволяют администратору каждого приложения определить порядок преобразования маркеров различных поставщиков в маркер Access Control. Например, если поставщики удостоверений используют разные типы для представления имен пользователей, правила Access Control могут преобразовать все эти типы в общий тип имени пользователя. После этого Access Control отправляет новый маркер обратно в браузер (шаг 4), который, в свою очередь, отправляет его в приложение (шаг 5). После получения маркера Access Control приложение проверяет, что маркер действительно выдан службой Access Control, а затем использует данные, которые в нем содержатся (шаг 6).

Хотя этот процесс может показаться сложным, на самом деле он существенно упрощает жизнь создателям приложений. Вместо того, чтобы обрабатывать различные маркеры, содержащие разные данные, приложение может принимать удостоверения, выдаваемые несколькими поставщиками, и при этом получать лишь один маркер с известным форматом данных. Кроме того, приложения не нужно настраивать для доверия различным поставщикам удостоверений, поскольку отношения доверия поддерживаются службой Access Control, а приложениям требуется только доверие к этой службе.

Стоит отметить, что служба Access Control не привязана к Windows — ее точно так же могут использовать приложения Linux, принимающие только удостоверения Google и Facebook. Хотя Access Control принадлежит к семейству Azure Active Directory, эту службу можно считать совершенно отличной от служб, описанных в предыдущем разделе. Хотя обе технологии работают с удостоверениями, они решают совершенно разные задачи (хотя корпорация Майкрософт заявила о своем намерении интегрировать их в будущем).

Работа с удостоверениями важна практически в любом приложении. Цель Access Control — упростить для разработчиков создание приложений, принимающих удостоверения от различных поставщиков. Разместив эту службу в облаке, корпорация Майкрософт сделала ее доступной для любых приложений, работающих на любых платформах.

##Об авторе

Дэвид Чаппел (David Chappell) является главой компании Chappell & Associates [www.davidchappell.com](http://www.davidchappell.com), расположенной в Сан-Франциско, штат Калифорния.

<!---HONumber=AcomDC_0727_2016-->