---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-02"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# プッシュ通知パッケージの使用
{: #openwhisk_catalog_pushnotifications}

`/whisk.system/pushnotifications` パッケージは、プッシュ・サービスを使用した処理を可能にします。
{: shortdesc}

このパッケージには、以下のアクションおよびフィードが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | パッケージ | appId、appSecret| プッシュ・サービスを使用した処理を行う|
| `/whisk.system/pushnotifications/sendMessage` | アクション | text、url、deviceIds、platforms、userIds、tagNames、gcmCollapseKey、gcmCategory、gcmIcon、gcmDelayWhileIdle、gcmSync、gcmVisibility、gcmPayload、gcmPriority、gcmSound、gcmTimeToLive、gcmStyleType、gcmStyleTitle、gcmStyleUrl、gcmStyleText、gcmStyleLines、gcmLightsLedArgb、gcmLightsLedOnMs、gcmLightsLedOffMs、apnsBadge、apnsCategory、apnsIosActionKey、apnsPayload、apnsType、apnsSound、apnsTitleLocKey、apnsLocKey、apnsLaunchImage、apnsTitleLocArgs、apnsLocArgs、apnstitle、apnsSubtitle、apnsAttachmentUrl、fireFoxTitle、fireFoxIconUrl、fireFoxTimeToLive、fireFoxPayload、safariTitle、safariUrlArgs、safariAction、chromeTitle、chromeIconUrl、chromeTimeToLive、chromePayload、chromeAppExtTitle、chromeAppExtCollapseKey、chromeAppExtDelayWhileIdle、chromeAppExtIconUrl、chromeAppExtTimeToLive、chromeAppExtPayload| 指定された 1 つ以上のデバイスにプッシュ通知を送信する|
| `/whisk.system/pushnotifications/webhook` | フィード | events | プッシュ・サービスでデバイスのアクティビティー (デバイスの登録、登録解除、サブスクリプション、アンサブスクリプション) に基づいてトリガー・イベントを起動する|
`appId` と `appSecret` の値を使用して、パッケージ・バインディングを作成することをお勧めします。この方法を使用すると、パッケージ内のアクションを呼び出すたびにこれらの資格情報を指定する必要がありません。

## プッシュ通知パッケージ・バインディングの作成
{: #openwhisk_catalog_pushnotifications_create}

プッシュ通知パッケージ・バインディングを作成するには、以下のパラメーターを指定する必要があります。

-  `appId`: {{site.data.keyword.Bluemix}} アプリ GUID。
-  `appSecret`: {{site.data.keyword.Bluemix_notm}} プッシュ通知サービスの appSecret。

以下の手順例を参照して、パッケージ・バインディングを作成します。

1. [{{site.data.keyword.Bluemix_notm}} ダッシュボード](http://console.bluemix.net)で {{site.data.keyword.Bluemix_notm}} アプリケーションを作成します。

2. プッシュ通知サービスを初期化し、{{site.data.keyword.Bluemix_notm}} アプリケーションにバインドします。

3. [プッシュ通知アプリケーション](https://console.bluemix.net/docs/services/mobilepush/index.html)を構成します。

  作成した {{site.data.keyword.Bluemix_notm}} アプリの `App GUID` と `App Secret` を必ず覚えておいてください。

4. `/whisk.system/pushnotifications` を使用してパッケージ・バインディングを作成します。

  ```
  wsk package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}
  
5. このパッケージ・バインディングが存在することを確認します。

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myNamespace/myPush private binding
  ```


## プッシュ通知パラメーター
{: #openwhisk_push_parameters}

`/whisk.system/pushnotifications/sendMessage` アクションは、登録されたデバイスにプッシュ通知を送信します。パラメーターは次のとおりです。

- `text`: ユーザーに表示する通知メッセージ。例えば、`-p text "Hi, OpenWhisk send a notification"` です。
- `url`: アラートと一緒に送信できる URL。例えば、`-p url "https:\\www.w3.ibm.com"` です。
- `deviceIds`: 指定されるデバイスのリスト。例えば、`-p deviceIds ["deviceID1"]` です。
- `platforms`: 指定されるプラットフォームのデバイスに通知を送信します。Apple (iOS) デバイスの場合は「A」、Google (Android) デバイスの場合は「G」です。例えば、`-p platforms ["A"]` です。
- `userIds`: 指定されるユーザーのデバイスに通知を送信します。例えば、`-p userIds "[\"testUser\"]"` です。
- `tagNames`: これらのタグのいずれかにサブスクライブしているデバイスに通知を送信します。例えば、`-p tagNames "[\"tag1\"]"` です。
- `gcmCollapseKey`: このパラメーターは、メッセージのグループを識別します。
- `gcmCategory`: 対話式プッシュ通知に使用されるカテゴリー ID。
- `gcmIcon`: 通知に対して表示されるアイコンの名前を指定します。このアイコンが既にクライアント・アプリケーションと共にパッケージされていることを確認してください。
- `gcmDelayWhileIdle`: このパラメーターが true に設定されている場合、メッセージはデバイスがアクティブになるまで送信されます。
- `gcmSync`: デバイス・グループ・メッセージングは、グループ内のすべてのアプリ・インスタンスが最新メッセージング状態を反映することを可能にします。
- `gcmVisibility`: private/public - この通知の可視性。これは、通知がいつ、どのように、ロックされた保護画面に表示されるのかに影響します。
- `gcmPayload`: 通知メッセージの一部として送信されるカスタム JSON ペイロード。例えば、`-p gcmPayload "{\"hi\":\"hello\"}"` です。
- `gcmPriority`: メッセージの優先順位を設定します。
- `gcmSound`: 通知がデバイスに到着したときに再生される (デバイス上の) 音声ファイル。
- `gcmTimeToLive`: このパラメーターは、デバイスがオフラインの場合に GCM ストレージ内にメッセージが保持される時間 (秒) を指定します。
- `gcmStyleType`: 展開可能な通知のタイプを指定します。指定できる値は、`bigtext_notification`、`picture_notification`、`inbox_notification` です。
- `gcmStyleTitle`: 通知のタイトルを指定します。タイトルは通知が展開されると表示されます。タイトルは、展開可能な 3 つの通知のすべてで指定される必要があります。
- `gcmStyleUrl`: 通知のピクチャーの取得元である URL。`picture_notification` にはこれを指定する必要があります。
- `gcmStyleText`: `bigtext_notification` の展開時に表示される必要がある大きいテキスト。`bigtext_notification` にはこれを指定する必要があります。
- `gcmStyleLines`: `inbox_notification` の場合にボックス内スタイルで表示されるストリングの配列。`inbox_notification` にはこれを指定する必要があります。
- `gcmLightsLedArgb`: LED の色。ハードウェアはできる限りの近似を行います。
- `gcmLightsLedOnMs`: LED の点滅時に明かりがついている時間 (ミリ秒)。ハードウェアはできる限りの近似を行います。
- `gcmLightsLedOffMs`: LED の点滅時に明かりが消えている時間 (ミリ秒)。ハードウェアはできる限りの近似を行います。
- `apnsBadge`: アプリケーション・アイコンのバッジとして表示する番号。
- `apnsCategory`: 対話式プッシュ通知に使用されるカテゴリー ID。
- `apnsIosActionKey`: アクション・キーのタイトル。
- `apnsPayload`: 通知メッセージの一部として送信されるカスタム JSON ペイロード。
- `apnsType`: ['DEFAULT', 'MIXED', 'SILENT']。
- `apnsSound`: アプリケーション・バンドル内の音声ファイルの名前。このファイルの音声がアラートとして再生されます。
- `apnsTitleLocKey`: 現行のローカリゼーション用の `Localizable.strings` ファイル内のタイトル・ストリングのキー。キー・ストリングは、`titleLocArgs` 配列に指定された変数を取り込むために、%@ および %n$@ の指定子を使用してフォーマット設定できます。
- `apnsLocKey`: (ユーザーの言語設定によって指定された) 現行のローカリゼーション用の `Localizable.strings` ファイル内のアラート・メッセージ・ストリングのキー。キー・ストリングは、locArgs 配列に指定された変数を取り込むために、%@ および %n$@ の指定子を使用してフォーマット設定できます。
- `apnsLaunchImage`: アプリケーション・バンドル内のイメージ・ファイルのファイル名 (ファイル名拡張子を含めても含めなくてもよい)。このイメージは、ユーザーがアクション・ボタンをタップした時、またはアクション・スライダーを移動した時に起動イメージとして使用されます。
- `pnsTitleLocArgs`: `title-loc-key` 内のフォーマット指定子を置換する変数ストリング値。
- `apnsLocArgs`: `locKey` 内のフォーマット指定子を置換する変数ストリング値。
- `apnstitle`: リッチ・プッシュ通知のタイトル (iOS 10 以上のみでサポートされます)。
- `apnsSubtitle`: リッチ通知のサブタイトル (iOS 10 以上でのみサポートされます)。
- `apnsAttachmentUrl`: iOS 通知メディアへのリンク (ビデオ、オーディオ、GIF、イメージ - iOS 10 以上のみでサポートされます)。
- `fireFoxTitle`: Web プッシュ通知に対して設定されるタイトルを指定します。
- `fireFoxIconUrl`: Web プッシュ通知に対して設定されるアイコンの URL。
- `fireFoxTimeToLive`: このパラメーターは、デバイスがオフラインの場合に GCM ストレージ内にメッセージが保持される時間 (秒) を指定します。
- `fireFoxPayload`: 通知メッセージの一部として送信されるカスタム JSON ペイロード。
- `chromeTitle`: Web プッシュ通知に対して設定されるタイトルを指定します。
- `chromeIconUrl`: Web プッシュ通知に対して設定されるアイコンの URL。
- `chromeTimeToLive`: このパラメーターは、デバイスがオフラインの場合に GCM ストレージ内にメッセージが保持される時間 (秒) を指定します。
- `chromePayload`: 通知メッセージの一部として送信されるカスタム JSON ペイロード。
- `safariTitle`: Safari プッシュ通知に対して設定されるタイトルを指定します。
- `safariUrlArgs`: この通知で使用する必要がある URL 引数。これらの引数は JSON 配列の形式で指定されます。
- `safariAction`: アクション・ボタンのラベル。
- `chromeAppExtTitle`: Web プッシュ通知に対して設定されるタイトルを指定します。
- `chromeAppExtCollapseKey`: このパラメーターは、メッセージのグループを識別します。
- `chromeAppExtDelayWhileIdle`: このパラメーターが true に設定されている場合、デバイスがアクティブになるまでメッセージが送信されないことを示します。
- `chromeAppExtIconUrl`: Web プッシュ通知に対して設定されるアイコンの URL。
- `chromeAppExtTimeToLive`: このパラメーターは、デバイスがオフラインの場合に GCM ストレージ内にメッセージが保持される時間 (秒) を指定します。
- `chromeAppExtPayload`: 通知メッセージの一部として送信されるカスタム JSON ペイロード。


## プッシュ通知の送信
{: #openwhisk_send_push_notifications}

プッシュ通知パッケージからプッシュ通知を送信するには、以下の例を参照してください。

前に作成したパッケージ・バインディング内の `sendMessage` アクションを使用してプッシュ通知を送信します。`/myNamespace/myPush` は実際のパッケージ名に置き換えてください。
```
wsk action invoke /myNamespace/myPush/sendMessage --blocking --result  -p url https://example.com -p text "this is my message"  -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

```json
{
  "result": {
  "pushResponse":
    {
      "messageId":"11111H",
      "message":{
        "alert":"this is my message",
        "url":""
      },
      "settings":{
        "apns":{
          "sound":"default"
        },
        "gcm":{
          "sound":"default"
          },
        "target":{
          "deviceIds":["T1","T2"]
        }
      }
    }
  },
  "status": "success",
  "success": true
}
```


## プッシュ通知サービス・アクティビティーでのトリガー・イベントの起動
{: #openwhisk_catalog_pushnotifications_fire}

`/whisk.system/pushnotifications/webhook` は、指定されたアプリケーションでデバイス・アクティビティー (デバイスの登録/登録解除、サブスクライブ/アンサブスクライブなど) があるとトリガーを起動するプッシュ・サービスを構成します。

パラメーターは次のとおりです。

- `appId:` {{site.data.keyword.Bluemix_notm}} アプリ GUID。
- `appSecret:` {{site.data.keyword.Bluemix_notm}} プッシュ通知サービスの appSecret。
- `events:` サポートされるイベントは、`onDeviceRegister`、`onDeviceUnregister`、`onDeviceUpdate`、`onSubscribe`、および `onUnsubscribe` です。ワイルドカード文字 `*` を使用して、すべてのイベントに対して通知されるようにすることができます。

プッシュ通知サービス・アプリケーションに新規デバイスが登録されるたびに起動されるトリガーを作成するには、以下の例を参照してください。

1. ご使用の appId と appSecret を使用して、プッシュ通知サービス用に構成されたパッケージ・バインディングを作成します。
  ```
  wsk package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. `myPush/webhook` フィードを使用して、プッシュ通知サービス `onDeviceRegister` イベント・タイプ用のトリガーを作成します。
  ```
  wsk trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. 新規デバイスが登録されるたびにメッセージを送信するルールを作成できます。前のアクションとトリガーを使用してルールを作成します。
  ```
  wsk rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

  `wsk activation poll` で結果をチェックします。

  {{site.data.keyword.Bluemix_notm}} アプリケーションでデバイスを登録します。OpenWhisk [ダッシュボード](https://console.{Domain}/openwhisk/dashboard)で、`ルール`、`トリガー`、および`アクション`が実行されるのを確認できます。

  アクションはプッシュ通知を送信します。

