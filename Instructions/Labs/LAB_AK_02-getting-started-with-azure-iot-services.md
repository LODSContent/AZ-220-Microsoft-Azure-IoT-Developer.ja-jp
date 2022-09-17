---
lab:
  title: 'ラボ 02:Azure IoT Services の開始'
  module: 'Module 1: Introduction to IoT and Azure IoT Services'
---

# <a name="getting-started-with-azure-iot-services"></a>Azure IoT Services の開始

## <a name="lab-scenario"></a>課題シナリオ

あなたは、グルメチーズを製造し販売する会社である Contoso で働く Azure IoT 開発者です。

Contoso の IoT ソリューションの開発に使用する Azure と Azure IoT サービスの調査を任されています。 Azure portal をすでに理解し、プロジェクトのリソース グループを作成しました。 次に、Azure IoT サービスの調査を開始する必要があります。

## <a name="in-this-lab"></a>このラボでは

このラボでは、Azure IoT Hub と IoT Hub デバイス プロビジョニング サービスを作成して調査します。 ラボには、次の演習が含まれます。

* グローバルに一意のリソース命名要件を調べる
* Azure portal を使用して IoT Hub を作成する
* IoT Hub サービスの機能を確認する
* Device Provisioning Service を作成し、IoT Hub にリンクする
* Device Provisioning Service の機能を確認する

## <a name="lab-instructions"></a>ラボの手順

### <a name="exercise-1-explore-globally-unique-resource-naming-requirements"></a>演習 1:グローバルに一意のリソース命名要件を調べる

このコースのラボ 2 から 19 では、IoT ソリューションの開発に使用される Azure リソースを作成し、構成します。 ラボ全体の一貫性を確保し、リソースの使用が終了したときにリソースを整理するのに役立つように、推奨されるリソース名がラボの手順に記載されています。 可能な限り、推奨されるリソース名は、ここで推奨される命名ガイドラインに従います。[推奨される名前付けおよびタグ付け規則](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)。 ただし、このコースで作成するリソースの多くは、Web 全体で利用できるサービスを公開しているため、グローバルに一意の名前を付ける必要があります。 これらのリソースがグローバルに一意の要件を確実に満たすようにするには、必要に応じてリソース名の末尾に一意の識別子を追加します。

この演習では、一意の ID を作成し、例をいくつか確認します。これは、このコースのラボ 2 から 19 で一意の ID をどのように使うかを理解するために役立ちます。

#### <a name="task-1-create-your-unique-id"></a>タスク 1:一意の ID を作成する

1. 次のパターンで小文字のイニシャルと現在の日付を使用して、一意の ID を作成します。

    ```text
    YourInitialsYYMMDD
    ```

    一意の ID の最初の部分は、小文字のイニシャルになります。 2番目の部分は、現在の年の最後の 2 桁、現在の数値月、および現在の数値日になります。 次に例をいくつか示します。

    ```text
    gwb200123
    bho200504
    cah201216
    dm200911
    ```

    ラボの説明では、一意の ID を入力する必要がある場合は常に、推奨されるリソース名の一部として `{your-id}` が表示されます。 提案されたリソース名の `{your-id}` 部分はプレースホルダーです。 プレースホルダー文字列全体 (`{}` を含む) を一意の値に置き換えます。

1. 一意の ID をメモし、**コース全体で同じ値を使用してください**。

    > **注**:一意の ID の日付部分を毎日変更しないでください。 コースでは毎日、同じ一意の ID を使用してください。

#### <a name="task-2-review-how-and-when-to-apply-your-unique-id"></a>タスク 2:一意の ID を適用する方法とタイミングを確認する

このコースのラボで作成するリソースの多くは、パブリックにアドレス指定可能な (セキュリティで保護された) エンドポイントを持つため、グローバルに一意である必要があります。 グローバルに一意の名前を必要とするリソースの例としては、IoT Hub、Device Provisioning Services、Azure Storage アカウントなどがあります。

上記のように、これらの種類のリソースを作成すると、推奨されるガイドラインに従ったリソース名が提供され、リソース名の一部として一意の ID を含めるように指示されます。 一意の ID をいつ入力する必要があるかを明確にするために、推奨されるリソース名には、一意の ID のプレースホルダー値が含まれます。 プレースホルダー値 `{your-id}` を一意の ID に置き換えるように指示されます。

1. 次のリソース命名の例を確認してください。

    一意の ID が次の場合: **cah191216**

    | リソースの種類 | 名前テンプレート | 例 |
    | :--- | :--- | :--- |
    | IoT Hub | iot-az220-training-{your-id} | iot-az220-training-cah191216 |
    | デバイス プロビジョニング サービス | dps-az220-training-{your-id} | dps-az220-training-cah191216 |
    | Azure Storage アカウント <br/>(ダッシュを含まない、小文字の名前にする必要があります) | az220storage{your-id} | az220storagecah191216 |

1. Bash スクリプト内で一意の ID を適用するには、次の例を確認してください。

    このコースの後半の一部のラボでは、Bash スクリプト内で一意の ID 値を適用するように指示されます。 提供されている Bash スクリプト ファイルには、次のようなコードが含まれている場合があります。

    ```bash
    #!/bin/bash

    YourID="{your-id}"
    RGName="rg-az220"
    IoTHubName="iot-az220-training-$YourID"
    ```

    上記のコードでは、一意の ID の値が `cah191216` の場合、`YourID="{your-id}"` を含む行を `YourID="cah191216"` に更新する必要があります。

    > **注**:最終的なコード行の `$YourID` 値は変更しないことに注意してください。 `{your-id}` でない場合は、置き換えないでください。

1. C# コード内で一意の ID を適用するには、次の例を確認してください。

    このコースの後半の一部のラボでは、C# ソース ファイル内で一意の ID 値を適用するように指示されます。 提供されている C# ソース コードには、次のようなコード セクションが含まれている場合があります。

    ```csharp
    private string _yourId = "{your-id}";
    private string _rgName = "rg-az220";
    private string _iotHubName = $"iot-az220-training-{_yourId}";
    ```

    上記のコードでは、一意の ID の値が `cah191216` の場合、`private string _yourId = "{your-id}";` を含む行を `private string _yourId = "cah191216";` に更新する必要があります

    > **注**:最終的なコード行の `_yourId` 値は変更しないことに注意してください。 繰り返しになりますが、`{your-id}` でない場合は、置き換えないでください。

1. すべてのリソース名で一意の ID を適用する必要があるわけではないことに注意してください。

    すでに検討したかもしれませんが、前のラボで作成したリソース グループには、一意の ID 値が含まれていませんでした。

    リソース グループなどの一部のリソースには、サブスクリプション内で一意の名前を付ける必要がありますが、名前はグローバルに一意である必要はありません。 したがって、このコースを受講する各受講生は、リソース グループ名 **rg-az220** を使用できます。 もちろん、これは各受講生が独自のサブスクリプションを使用する場合にのみ当てはまりますが、そうであるはずです。

1. 一意の ID がそれほど一意でないことが判明した場合は、追加の `01` または `02` を適用します。

    同じイニシャルを持つ 2 人以上の人が同じ日にコースを開始する可能性があります。 次の演習で IoT Hub を作成するまで、その人があなたの隣に座っていない限り、あなたは知らないかもしれません。 Azure は、一意の ID を含む、提案されたリソース名がグローバルに一意でないかどうかを通知します。 その場合、追加の `##` 値を追加して、一意の ID を更新するように指示されます。 たとえば、一意の ID の値が `cah191216` の場合、更新された一意の ID の値は次のようになります。

    ```text
    cah20121600
    cah20121601
    cah20121602
    ...
    cah20121699
    ```

    一意の ID を更新する必要がある場合は、それを一貫して使用するようにしてください。

### <a name="exercise-2-create-an-iot-hub-using-the-azure-portal"></a>演習 2:Azure portal を使用して IoT Hub を作成する

Azure IoT Hub は、IoT デバイスと Azure の間で信頼性とセキュリティで保護された双方向通信を実現するフルマネージド サービスです。 Azure IoT Hub サービスでは、次の機能が提供されます。

* 数十億台の IoT デバイスと双方向通信を確立
* 一方向メッセージング、ファイル転送、要求/応答方式など、複数の device-to-cloud への通信オプションと cloud-to-device への通信オプション。
* 他の Azure サービスへの組み込みの宣言型メッセージ ルーティング。
* デバイス メタデータと同期状態情報のクエリ可能なストア。
* デバイスごとのセキュリティ キーまたは X.509 証明書を使用した、セキュリティ保護された通信とアクセス制御。
* デバイス接続性とデバイス ID 管理イベントの広範囲のモニタリング。
* 最も一般的な言語とプラットフォームの SDK デバイス ライブラリ。

IoT Hub の作成には、いくつかの方法を使用できます。 たとえば、Azure portal を使用して IoT Hub リソースを作成したり、プログラムで IoT Hub (およびその他のリソース) を作成したりできます。 このコースでは、Azure CLI や Bash スクリプトなど、Azure リソースの作成と管理に使用できるさまざまな方法を調査します。

この演習では、Azure portal を使用して IoT Hub 作成および構成します。

#### <a name="task-1-use-the-azure-portal-to-create-a-resource-iot-hub"></a>タスク 1:Azure portal を使用して リソース (IoT Hub) を作成する

1. ラボ仮想マシン環境で Microsoft Edge ブラウザー ウィンドウを開き、次の Web アドレスを使用して Azure portal に移動します。

    +++http://portal.azure.com+++

    > **注**:緑色の "T" 記号 (例: +++このテキストを入力+++) が表示されているときはいつでも、関連付けられているテキストをクリックすると、仮想マシン環境内の現在のフィールドに情報が入力されます。

1. Azure アカウントの資格情報を使用してサインインするように求めるメッセージが表示されたら、このコースで使用している Azure 資格情報を入力します。

    複数の Azure アカウントをお持ちの場合は、このコースで使用するサブスクリプションに関連付けられているアカウントを使用してログインしていることを確認してください。

1. 前のラボで作成した AZ-220 ダッシュボードが読み込まれています。

    コースが進むにつれて、ダッシュボードにリソースを追加します。

1. Azure portal で、 **[+ リソースの作成]** をクリックします。

    開く**新しい**ブレードは、Azure で作成できるすべてのリソースのコレクションである Azure Marketplace のフロントエンドです。 Marketplace には、Microsoft とコミュニティの両方からのリソースが含まれています。

1. [検索] テキストボックスに「**iot hub**」と入力し、**Enter** キーを押します。

    **[Marketplace]** ブレードが開き、検索条件に一致する利用可能なサービスが表示されます。

    > **注**:非公開の貢献者が提供する Marketplace サービスには、Microsoft Azure Pass またはその他の Microsoft クレジット オファリングの対象外のコストが含まれる場合があります。 このコースのラボでは、Microsoft が提供するリソースを使用します。

1. **[Marketplace]** ブレードで、**IoT Hub** の検索結果をクリックします。

    > **注**:**作成**アクションは、**IoT Hub** 検索結果の下部に表示されます。これにより、IoT Hub 作成ビューに直接移動します。 通常の使用では、これをクリックすることを選択できます。チュートリアルの目的で、**IoT Hub** 検索結果の本体の任意の場所をクリックします。

1. **[IoT Hub]** ブレードで、 **[使用情報 + サポート]** をクリックします

    **[便利なリンク]** の下にあるリソース リンクのリストに注目してください。

    今すぐこれらのリンクを調べる必要はありませんが、利用できることは注目に値します。 たとえば、 _[ドキュメント]_ リンクをクリックすると、IoT Hub のリソースとドキュメントのルート ページに移動します。 このページを使用して、最新の Azure IoT Hub ドキュメントを確認し、このコースの範囲外の追加リソースを調べることができます。 特定のトピックに関する追加の資料については、このコース全体で docs.microsoft.com サイトを参照してください。

    リンクの 1 つを開いた場合は、今すぐ閉じて、ブラウザーを使用して [Azure portal] タブに戻ります。

#### <a name="task-2-create-an-iot-hub-with-required-property-settings"></a>タスク 2:必要なプロパティ設定を使用して IoT Hub を作成する

1. 新しい IoT Hub の作成プロセスを開始するには、 **[作成]** をクリックします。

    > **ヒント:** 今後、Azure リソースの種類の_作成_エクスペリエンスを実現する方法は、他に 2 つあります。
    >
    >    1. お気に入りにサービスがある場合は、サービスをクリックしてインスタンスのリストに移動し、上部の[ _+ 追加_] ボタンをクリックします。
    >    2. ポータルの上部にある [_検索_] ボックスでサービス名を検索して、インスタンスの一覧に移動し、上部の [ _+ 追加_] ボタンをクリックします。

    次の手順では、IoT Hub の作成に必要な設定について説明し、入力する各フィールドについて説明します。

1. **IoT Hub** ブレードの **サブスクリプション** ドロップダウンで、このコースで使用する Azure サブスクリプション が選択されていることを確認します。

    最初に選択された _[基本]_ タブには、入力する必要のある初期化されていないフィールドが含まれていますが、他のタブにも精通している必要のある設定があります。

1. **[リソース グループ]** の右側にあるドロップダウンを開き、 **[rg-az220]** をクリックします

    これは、前のラボで作成したリソース グループです。 このコースで作成したリソースを、同じリソース グループにまとめます。 この方法で関連リソースをグループ化することをお勧めします。リソースが不要になったときに、リソースをクリーンアップするのに役立ちます。


1. **[IoT Hub 名]** の右側に、次のように IoT Hub のグローバルに一意な名前を入力します。

    グローバルに一意の名前を指定するには、**iot-az220-training-{your-id}** と入力します ( **{your-id}** を演習 1 で作成した一意の ID に置き換えることを忘れないでください)。

    例: **iot-az220-training-cah191216**

    IoT Hub の名前は、パブリックにアクセス可能なリソースなので、IP 対応の IoT デバイスからからでもアクセスできる、グローバルに一意である必要があります。

    新しい IoT Hub に一意の名前を指定する場合は、次の点を考慮してください。

    * **IoT Hub 名**に適用する値は、すべての Azure で一意である必要があります。 これは、名前に割り当てられた値が IoT Hub の接続文字列で使用されるためです。 Azure では世界中のどこからでもデバイスをハブに接続できるため、すべての Azure IoT Hub は、接続文字列を使用してインターネットからアクセスできる必要があり、したがって接続文字列は一意である必要があります。 このラボでは、後で接続文字列について説明します。

    * **IoT Hub 名**に割り当てる値は、リソースの作成後に変更することはできません。 名前を変更する必要がある場合は、目的の名前で新しい IoT Hub を作成し、元のハブからデバイスを再登録して新しいハブに登録し、古い IoT Hub を削除する必要があります。

    * **IoT Hub 名**フィールドは必須フィールドです。

    > **注**:入力した名前が一意であることを Azure が確認します。 入力した名前が一意でない場合、Azure は警告として名前フィールドの下のメッセージを表示します。 警告メッセージが表示された場合は、一意の ID を更新する必要があります。 グローバルに一意の名前を実現するには、一意の ID に '**00**'、'**01**'、'**02**' などを追加してみてください。

    > **注**:一部のリソース名では、ダッシュ (-) やアンダースコア (_) などの拡張文字を使用できないため、一意の ID を更新するときは数字を使用してください。

1. **[リージョン]** の右側にあるドロップダウン リストを開き、リソースグループに選択したのと同じリージョンを選択します。

    > **注**:今後のラボの 1 つは、Event Grid を使用します。 この将来のラボをサポートするには、Event Grid をサポートするリージョンを選択する必要があります。 Event Grid をサポートするリージョンの現在のリストについては、次のリンクを参照してください。[リージョン別の利用可能な製品](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=event-grid&regions=all)

    以前、見たように、Azure は世界各地における一連のデータセンターでサポートされています。 Azure で何かを作成する場合は、これらのデータセンターの場所のいずれかにデプロイします。

    > **注**:リソースをホストするリージョンを選択する場合、エンド ユーザーに近いリージョンを選択すると、読み込み/応答時間が減少することに注意してください。 運用環境では、エンド ユーザーから世界の反対側にいる場合は、最も近いリージョンを選択しないでください。

1. ブレードの上部で、 **[管理]** をクリックします。

    このタブに表示されるフィールドやその他の情報を確認してください。

1. **[価格とスケールティア]** の右側で **[S1: Standard レベル]** が選択されていることを確認します。

    Azure IoT Hub は、必要な機能の数と、ソリューション内で 1 日に送信する必要のあるメッセージの数に応じていくつかのレベル オプションを提供します。 このコースで使用する _S1_ レベルでは、1 日あたり合計 400,000 件のメッセージを受信でき、このトレーニングに必要なすべてのサービスを提供します。 実際には、ユニットあたり 1 日あたり 400,000 メッセージは必要ありませんが、_Cloud-to-device コマンド_、_デバイス管理_、および_IoT Edge_など、Standard レベルで提供される機能を使用します。 IoT Hub では、テストおよび評価のための Free レベルも提供されています。 Standard レベルと同じ機能がありますが、メッセージングの許容量が制限されています。 Free レベルから Basic レベルまたは Standard レベルにアップグレードすることはできないことに注意してください。 Free レベルでは、IoT Hub に接続できるデバイスは 500 個で、1 日に許可されるメッセージ数は最大 8,000 件です。 Azure サブスクリプションごとに、Free レベルの IoT Hub を 1 つ作成できます。

    > **注**:_S1 - Standard_ 階層のコストは、単位あたり月額 25.00 米国ドルです。 1 台指定します。 その他の階層オプションの詳細については、[[ソリューションに適した IoT Hub階層の選択](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-scaling)] を参照してください。

1. **[S1 IoT Hub ユニットの数]** の右側で **[1]** が選択されていることを確認します。

    前述のように、選択した価格レベルによって 1 日あたりにハブが 1 つの単位で処理できるメッセージの数が設定されます。 高い価格レベルに移行せずにハブで処理できるメッセージの数を増やすには、単位数を増やします。 たとえば、IoT Hub で 1 日あたり 800,000 メッセージのイングレスをサポートする場合は *2 つの* S1 レベル単位を指定します。 このコースでは、1 ユニットのみを使用します。

1. **Defender for IoT** の右側で、 **[オフ]** が選択されていることを確認します。

    Azure Defender for IoT は、IoT/OT デバイス、脆弱性、脅威を特定するための統合セキュリティ ソリューションです。 これを使用すれば、既存の IoT/OT デバイスを保護する必要があるか、新しい IoT イノベーションにセキュリティを組み込む必要があるかにかかわらず、IoT/OT 環境全体をセキュリティで保護することができます。

    > **助言**：**Azure Defender for IoT** は以前は **Azure Security Center** と呼ばれていましたが、Azure 内、およびこのコンテンツ内で、名前がまだ更新されていない場所が引き続き表示される場合があります。

    IoT ソリューションにとってセキュリティが重要であるため、Azure Defender for IoT は既定でオンになっています。 このコースのラボ 19 では、Azure Defender for IoT について学習します。 今のところ無効にすると、ラボ 19 の手順が期待どおりに機能するようになります。

    現在、Azure portal を使用して、サブスクリプション レベルで Azure Defender を有効にできます。 Azure Defender は、最初の 30 日間は無料で利用できます。 30 日を超える使用は[ここ](https://azure.microsoft.com/en-us/pricing/details/azure-defender/)で説明する価格設定情報に従って自動的に課金されます。

1. 現在の設定とコストをまとめた表を確認します。

1. **[詳細設定]** (必要に応じて下にスクロールします) で、 **[Device-to-cloud パーティション]** が **4** に設定されていることを確認します。

    パーティションの数は、device-to-cloud メッセージの、同時読み取り数に関連しています。 ほとんどの IoT Hub は、4 つのパーティション (既定値) のみが必要です。 このコースでは、既定のパーティション数で IoT Hub を作成します。

1. **[トランスポート層セキュリティ (TLS)]** セクションで、 **[最小 TLS バージョン]** が **1.0** に設定されていることを確認します。

    IoT Hub は、トランスポート層セキュリティ (TLS) を使用して、IoT デバイスとサービスからの接続をセキュリティで保護します。 現時点では 2 つのバージョンの TLS プロトコル (つまり、バージョン 1.0 と 1.2) がサポートされています。

    > [!Important]
    > IoT Hub リソースが作成されると、 **[最小 TLS バージョン]** プロパティを変更することはできません。 したがって、すべての IoT デバイスとサービスが TLS 1.2 および推奨される暗号と互換性があることを、事前に適切にテストし、検証する必要があります。 IoT Hub と TLS の詳細については、以下を参照してください。
    > * [IoT Hub でのトランスポート層セキュリティ (TLS) のサポート](https://docs.microsoft.com/azure/iot-hub/iot-hub-tls-support)

1. ブレードの最下部で、 **[Review + create]** をクリックします。

    設定した内容をレビューして確認します。

1. IoT Hub の作成を完了するには、ブレードの下部で、 **[作成]** をクリックします。

    デプロイが完了するまでに 1 分以上かかることがあります。 Azure portal の通知ウィンドウを開いて、進行状況を監視できます。

1. 数分後に、IoT Hub が **rg-az220** リソース グループに正常にデプロイされたことを示す通知が表示されます。

1. Azure portal メニューで **[ダッシュボード]** をクリックしてから、 **[更新]** をクリックします。

    リソース グループのタイルに新しい IoT Hub が表示されます。

### <a name="exercise-3-examine-the-iot-hub-service"></a>演習 3:IoT Hub サービスを確認する

すでに学習したように、IoT Hub はクラウドでホストされるマネージド サービスであり、Azure IoT サービスと接続されたデバイス間の双方向通信の中央メッセージ ハブとして機能します。

IoT Hub の機能は、製造で使用される産業機器の管理、ヘルスケアの貴重な資産のトラッキング、オフィス ビルの使用状況の監視など、スケーラブルですべての機能を備えた IoT ソリューションの構築に役立ちます。 IoT Hub の監視は、デバイスの作成、デバイスの障害、デバイスの接続などのイベントを追跡することにより、ソリューションの正常性を維持するのに役立ちます。

この演習では、IoT Hub が提供する機能のいくつかを確認します。

#### <a name="task-1-explore-the-iot-hub-overview-information"></a>タスク 1:IoT Hub の概要情報を確認する

1. Azure portal ウィンドウを閉じた場合は、Microsoft Edge ブラウザー ウィンドウを開いて、Azure portal に移動します。

    +++http://portal.azure.com+++

    複数の Azure アカウントをお持ちの場合は、このコースで使用するサブスクリプションに関連付けられているアカウントを使用してログインしていることを確認してください。

1. AZ-220 ダッシュボードが表示されていることを確認します。

1. **rg-az220** リソース グループのタイルで、**iot-az220-training-{your-id}** をクリックします

    IoT Hub ブレードを最初に開くと、**概要**情報が表示されます。 ご覧のとおり、このブレードの上部にあるエリアには、データセンターの場所やサブスクリプションなど、IoT Hub サービスに関する重要な情報が表示されます。 ただし、このブレードには、ハブの使用方法と最近のアクティビティに関する情報を提供するタイルも含まれています。 さらに探索する前に、これらのタイルを見てみましょう。

1. IoT Hub ブレードの左下にある、**IoT Hub の使用状況** タイルに注目してください。

    > **注**:タイルの位置はブラウザー ウィンドウの幅に基づいているため、レイアウトは説明とは少し異なる場合があります。

    このタイルは、ハブとメッセージ数に接続されている内容の概要を簡単に提供します。 デバイスを追加してメッセージの送信を開始すると、このタイルは "一目でわかる" 情報を提供します。

1. **[IoT Hub の使用状況]** タイルの右側に、 **[使用されたメッセージの数]** タイルと **[デバイスからクラウド メッセージ]** タイルが表示されます。

    **[クラウド メッセージにデバイス]** タイルを使用すると、デバイスからの受信メッセージが時間の経過に応じ、すばやく表示されます。 次のモジュールのモジュールでデバイスを登録し、ハブにメッセージを送信するので、これらのタイルに関する情報がすぐに表示されるようになります。

    **[使用されたメッセージの数]** タイルは、使用されたメッセージの総数を追跡するのに役立ちます。

#### <a name="task-2-view-features-of-iot-hub-using-the-left-side-menu"></a>タスク 2:左側のメニューを使用して IoT Hub の機能を表示する

1. [IoT Hub ブレード] で、左側のメニュー オプションをスキャンします。

    ご期待どおり、ここれらのメニューオプションは、IoT Hub のプロパティと機能へのアクセスを提供するブレードを開きます。 たとえば、一部のペインは、ハブに接続されているデバイスへのアクセスを提供します。

1. 左側のナビゲーション メニューの **[デバイス管理]** の下にある **[デバイス]** をクリックします

    このペインを使用して、ハブに登録されているデバイスを追加、変更、および削除できます。 このコースの終わりまでに、このペインにかなり慣れます。

1. 左側のメニューの上部近くにある **[アクティビティ ログ]** をクリックします。

    名前が示すように、このペインを使用すると、アクティビティの確認や問題の診断に使用できるログにアクセスできます。 ルーチン タスクに役立つクエリを定義することもできます。 非常に便利です。

1. 左側のメニューの **[Hub の設定]** の下にある **[組み込みエンドポイント]** をクリックします。

    IoT Hub は、外部接続を有効にする [エンドポイント] を公開します。 基本的に、エンドポイントは IoT Hub に接続または通信するものです。 ハブには、2 つのエンドポイントが既に定義されていることがわかります。

    * _イベント_
    * _cloud-to-device メッセージング_

1. 左側のメニューの **[Hub の設定]** の下にある **[メッセージ ルーティング]** をクリックします。

    IoT Hub のメッセージ ルーティング機能を使用すると、受信 device-to-cloud へのメッセージを、Azure Storage Containers、Event Hubs、Service Bus キューなどのサービス エンドポイントにルーティングできます。 また、クエリ ベースのルートを実行するルーティング ルールを作成することもできます。

1. **[メッセージ ルーティング]** ペインの上部にある **[カスタム エンドポイント]** をクリックします。

    カスタム エンドポイント (Service Bus キューおよび Storage) は、IoT 実装内で頻繁に使用されます。

1. **[設定]** の下のメニュー オプションで、スキャンしてください。

    > **注**:このラボの演習は、IoT Hub サービスの紹介であり、UI の使いやすさを高める目的だけなので、この時点で少し圧倒された場合でも心配しないでください。 このコースを進めるにつれて、IoT Hub、デバイス、およびそれらの通信を構成および管理することになります。

### <a name="exercise-4-create-a-device-provisioning-service-using-the-azure-portal"></a>演習 4:Azure portal を使用して デバイス プロビジョニング サービスを作成する

Azure IoT Hub Device Provisioning Service は、人間の介入を必要とせずに、適切な IoT Hub に対してゼロタッチの Just-In-Time プロビジョニングを可能にする IoT Hub のヘルパー サービスです。 デバイス プロビジョニング サービスでは、次の機能が提供されます。

* 工場 (初期設定) で IoT Hub 接続情報をハードコーディングすることなく、ゼロタッチで単一の IoT ソリューションにプロビジョニングできます
* 複数のハブ間でデバイスの負荷を分散します
* 販売トランザクション データに基づいて、デバイスをデバイス所有者の IoT ソリューションに接続します (マルチテナント)
* ユースケースに応じて特定の IoT ソリューションにデバイスを接続します (ソリューションの分離)
* 最低限の待機時間でデバイスを IoT ハブに接続します (geo シャーディング)
* デバイスの変化に基づいて再プロビジョニングします
* デバイスが IoT Hub に接続するときに使用するキーをローリングします (接続に X.509 証明書を使用しない場合)

IoT Hub Device Provisioning Service のインスタンスを作成するために使用できる方法がいくつかあります。 たとえば、このタスクで使用する、Azure portal を使用できます。 ただし、Azure CLI または Azure Resource Manager テンプレートを使用して DPS インスタンスを作成することもできます。

#### <a name="task-1-use-the-azure-portal-to-create-a-resource-device-provisioning-service"></a>タスク 1:Azure portal を使用してリソースを作成する (デバイス プロビジョニング サービス)

1. Azure portal ウィンドウを閉じた場合は、Microsoft Edge ブラウザー ウィンドウを開いて、Azure portal に移動します。

    +++http://portal.azure.com+++

    複数の Azure アカウントをお持ちの場合は、このコースで使用するサブスクリプションに関連付けられているアカウントを使用してログインしていることを確認してください。

1. Azure portal で、 **[+ リソースの作成]** をクリックします。

    前に見たように、**新しい**ブレードは、Azure Marketplace でサービスを検索する機能を提供します。

1. [検索] テキスト ボックスに、「**デバイス プロビジョニング サービス**」と入力し、Enter キーを押します。

1. **[Marketplace]** のブレードで、 **[IoT Hub デバイス プロビジョニング サービス]** の検索結果をクリックします。

    > **注**:**作成**アクションは、**IoT Hub デバイス プロビジョニングサービス**の検索結果の下部に表示されます。このアクションは、IoT Hub デバイス プロビジョニング サービスの作成ビューに直接移動します。 通常の使用では、これをクリックすることを選択できます。チュートリアルの目的で、**IoT Hub デバイス プロビジョニング サービス**の検索結果の本体の任意の場所をクリックします。

1. **[IoT Hub デバイス プロビジョニング サービス]** ブレードで、 **[使用情報 + サポート]** をクリックします

    **[便利なリンク]** の下にあるリソース リンクのリストに注目してください。

    このドキュメントを今参照する必要はありませんが、このドキュメントが利用可能であることを知ることは良いことです。 IoT Hub デバイス プロビジョニング サービス ドキュメント ページは、DPS のルート ページです。 このページを使用して、現在のドキュメントを詳しく調査すると、このコースの範囲外のアクティビティを探索するのに役立つチュートリアルやその他のリソースを見つけることができます。 特定のトピックに関する追加の資料については、このコース全体で docs.microsoft.com サイトを参照してください。

    リンクの 1 つを開いた場合は、今すぐ閉じて、ブラウザーを使用して [Azure portal] タブに戻ります。

#### <a name="task-2-create-a-device-provisioning-service-with-required-property-settings"></a>タスク 2:必要なプロパティ設定を使用してデバイス プロビジョニング サービスを作成する

1. 新しい DPS インスタンスの作成プロセスを開始するには、 **[作成]** をクリックします。

    次に、ハブとサブスクリプションに関する情報を指定する必要があります。 次の手順では、各フィールドの入力を説明しながら、設定をウォークスルーします。

1. **[サブスクリプション]** で、このコースに使用しているサブスクリプションが選択されていることを確認します。

1. **[リソース グループ]** で、ドロップダウンを開き、 **[rg-az220]** をクリックします

    このコースで作成したリソースを、同じリソース グループにまとめます。 この方法で関連リソースをグループ化することをお勧めします。リソースが不要になったときに、リソースをクリーンアップするのに役立ちます。

1. **[名前]** で、IoT Hub デバイス プロビジョニング サービスのグローバルに一意の名前を次のように入力します。

    グローバルに一意の名前を指定するには、**dps-az220-training-{your-id}** と入力します( **{your-id}** を演習 1 で作成した一意の ID に置き換えることを忘れないでください)。

    例: **dps-az220-training-cah191216**


1. **[リージョン]** で、ドロップダウン リストを開き、リソースグループに選択したのと同じリージョンを選択します。

    > **注**:リソースをホストするデータセンターを選択する場合、エンド ユーザーに近いデータセンターを選択すると、読み込み/応答時間が短縮されます。 エンド ユーザーから世界の反対側にいる場合は、最も近いデータセンターを選択しないでください。

1. ブレードの最下部で、 **[Review + create]** をクリックします。 検証に成功したら、 **[作成]** をクリックします。

    デプロイが完了するまでに 1 分以上かかることがあります。 Azure portal の通知ウィンドウを開いて、進行状況を監視できます。

1. 数分後に、IoT Hub デバイス プロビジョニング サービス インスタンスが **rg-az220** リソース グループに正常にデプロイされたことを示す通知が表示されます。

1. Azure portal メニューで **[ダッシュボード]** をクリックしてから、 **[更新]** をクリックします。

    リソース グループのタイルに、新しい IoT Hub デバイス プロビジョニング サービスが表示されるはずです。

#### <a name="task-3-link-your-iot-hub-and-device-provisioning-service"></a>タスク 3:IoT Hub とデバイス プロビジョニング サービスをリンクします。

1. AZ-220 ダッシュボードには、IoT Hub リソースと DPS リソースの両方が表示されます。

    IoT Hub と DPS の両方のリソースが表示されます (リソースが最近作成された場合は、 **[更新]** をクリックする必要があります)

1. **rg-az220** リソース グループのタイルで、**dps-az220-training-{your-id}** をクリックします。

1. **[デバイス プロビジョニング サービス]** ブレードの **[設定]** で、 **[リンクされた IoT Hub]** をクリックします。

1. ブレードの上部にある **[+ 追加]** をクリックします。

    **[IoT Hub へのリンクの追加]** ブレードを使用して、デバイス プロビジョニング サービス インスタンスを IoT Hub にリンクするために必要な情報を提供します。

1. **[IoT Hub へのリンクの追加]** ブレードで、 **[サブスクリプション]** ドロップダウンにこのコースで使用しているサブスクリプションが表示されていることを確認します。

    サブスクリプションは、使用可能な IoT Hub の一覧を提供するために使用されます。

1. IoT Hub のドロップダウンを開き、 **[iot-az220-training-{your-id}]** をクリックします。

    これは、前の演習で作成した IoT Hub です。

1. アクセス ポリシー ドロップダウンで **iothubowner** をクリックします。

    _iothubowner_ 認証情報は、指定された IoT Hub とのリンクを確立するために必要なアクセス許可を提供します。

1. 構成を完了するには、 **[保存]** をクリックします。

    選択したハブが [リンクされた IoT Hub] ブレードに表示されます。 リンクされた IoT Hub を表示するには、 **[最新の情報に更新]** をクリックする必要がある場合があります。

1. Azure portal メニューで、 **[ダッシュボード]** をクリックします。

### <a name="exercise-5-examine-the-device-provisioning-service"></a>エクササイズ 5:デバイス プロビジョニング サービスの確認

IoT Hub Device Provisioning Service は、適切な IoT ハブへのゼロタッチでジャストインタイムなプロビジョニングを可能にする、IoT Hub のヘルパー サービスです。人間の介入を必要とせず、安全かつスケーラブルな方法で何百万というデバイスをプロビジョニングできます。

#### <a name="task-1-explore-the-device-provisioning-service-overview-information"></a>タスク 1:デバイス プロビジョニング サービスの概要情報を詳しく見る

1. Azure portal ウィンドウを閉じた場合は、Microsoft Edge ブラウザー ウィンドウを開いて、Azure portal に移動します。

    +++http://portal.azure.com+++

    複数の Azure アカウントをお持ちの場合は、このコースで使用するサブスクリプションに関連付けられているアカウントを使用してログインしていることを確認してください。

1. AZ-220 ダッシュボードが表示されていることを確認します。

1. **rg-az220** リソース グループのタイルで、**dps-az220-training-{your-id}** をクリックします

    デバイス プロビジョニング サービス インスタンスを初めて開くと、[概要] 情報が表示されます。 ご覧のとおり、ブレードの上部にあるエリアには、状態、データセンターの場所、サブスクリプションなど、DPS インスタンスに関する重要な情報が表示されます。 このブレードには、次の項目にアクセスできる_クイック リンク_セクションもあります。

    * [Azure IoT Hub Device Provisioning Service のドキュメント](https://docs.microsoft.com/en-us/azure/iot-dps/)
    * [IoT Hub Device Provisioning Service の詳細](https://docs.microsoft.com/en-us/azure/iot-dps/about-iot-dps)
    * [デバイス プロビジョニングの概念](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-service)
    * [価格とスケーリングの詳細](https://azure.microsoft.com/en-us/pricing/details/iot-hub/)

    時間が許せば、戻ってこれらのリンクを探索できます。

#### <a name="task-2-view-features-of-device-provisioning-service-using-the-navigation-menu"></a>タスク 2:ナビゲーション メニューを使用してデバイス プロビジョニング サービスの機能を表示する

1. 左側のメニュー オプションをスキャンしてください。

    ご存知のように、これらのオプションは、DPS インスタンスのアクティビティ ログ、プロパティ、および機能へのアクセスを提供するペインを開きます。

1. 左側のメニューの上部近くにある **[アクティビティ ログ]** をクリックします。

    名前が示すように、このペインを使用すると、アクティビティの確認や問題の診断に使用できるログにアクセスできます。 ルーチン タスクに役立つクエリを定義することもできます。 非常に便利です。

1. 左側のメニューの **[設定]** で、 **[クイック スタート]** をクリックします。

    このペインには、Iot Hub Device Provisioning Service の使用を開始する手順、ドキュメントへのリンク、および DPS を構成するための他のブレードへのショートカットが表示されます。

1. 左側のメニューの **[設定]** で、 **[共有アクセス ポリシー]** をクリックします。

    このペインでは、アクセス ポリシーの管理、既存のポリシー、および関連するアクセス許可の一覧が表示されます。

1. 左側のメニューの、 **[設定]** で、 **[Linked IoT hubs]** をクリックします。

    ここには、以前からリンクされた IoT Hub を表示できます。 デバイス プロビジョニング サービスでは、このサービスにリンクされた IoT Hub にのみデバイスをプロビジョニングできます。 デバイス プロビジョニング サービスのインスタンスに IoT Hub をリンクすると、サービスの読み取り/書き込みのアクセス許可が IoT Hub のデバイス レジストリに付与されます。デバイス プロビジョニング サービスでは、リンクを使用して、デバイス ツインにデバイス ID を登録して初期構成を設定できます。 リンクされた IoT Hub は、任意の Azure リージョン内に置くことができます。 お使いのプロビジョニング サービスに他のサブスクリプションのハブをリンクすることもできます。

1. 左側のメニューの **[設定]** で、 **[証明書]** をクリックします。

    ここでは、X.509 証明書認証を使用して Azure IoT Hub をセキュリティで保護するための、 X.509 証明書を管理することができます。 後程ラボで、X.509 証明書について説明します。

1. 左側のメニューの **[設定]** で、 **[登録の管理]** をクリックします。

    ここでは、登録グループと個々の登録を管理することができます。

    登録グループは、必要な初期構成を共有する多数のデバイスに対して使用することも、すべて同じテナントに移動するデバイスに使用することもできます。 加入グループは、特定の構成証明メカニズムを共有するデバイスのグループです。 登録グループでは、x.509 と対称の両方がサポートされています。 X.509 登録グループ内のすべてのデバイスでは、同じルートまたは中間の証明機関 (CA) によって署名された X.509 証明書を提示します。 対称キー登録グループ内の各デバイスでは、グループ対称キーから派生した SAS トークンを提示します。 加入グループ名と証明書の名前は、英数字の小文字でなければならず、ハイフンを含めることができます。

    個別加入は、登録する単一のデバイスのエントリです。 個々の登録では、構成証明メカニズムとして X.509 リーフ証明書または (実際の TPM または仮想 TPM の) SAS トークンを使用できます。 個別加入の登録 ID には、英数字、小文字と、必要に応じてハイフンを含めます。 個別登録では、必要な IoT ハブ デバイス ID が指定されている場合があります。

1. **[設定]** の下の他のメニュー オプションをいくつか確認します。

   > **注**:このラボの演習は、IoT Hub デバイス プロビジョニング サービスの紹介であり、UI に慣れることだけが目的ですので、この時点で少し圧倒されたと感じても心配しないでください。 コースが進むにつれて、DPS について詳しく説明します。