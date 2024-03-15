はい、下記の内容を自然な日本語に翻訳しました。

# AppAgent

<div align="center">

<a href='https://arxiv.org/abs/2312.13771'><img src='https://img.shields.io/badge/arXiv-2312.13771-b31b1b.svg'></a> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
 <a href='https://appagent-official.github.io'><img src='https://img.shields.io/badge/Project-Page-Green'></a> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
 <a href='https://github.com/buaacyw/GaussianEditor/blob/master/LICENSE.txt'><img src='https://img.shields.io/badge/License-MIT-blue'></a> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
 <a href="https://twitter.com/dr_chizhang"><img src="https://img.shields.io/twitter/follow/dr_chizhang?style=social" alt="Twitter Follow"></a> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

[**Chi Zhang***†](https://icoz69.github.io/), [**Zhao Yang***](https://github.com/yz93), [**Jiaxuan Liu***](https://www.linkedin.com/in/jiaxuan-liu-9051b7105/), [Yucheng Han](http://tingxueronghua.github.io), [Xin Chen](https://chenxin.tech/), [Zebiao Huang](),
<br>
[Bin Fu](https://openreview.net/profile?id=~BIN_FU2), [Gang Yu✦](https://www.skicyyu.org/)
<br>
(* 同等貢献, † プロジェクトリーダー, ✦ 責任著者)
</div>


![](./assets/teaser.png)

ℹ️ 本プロジェクトご利用の際に何かお困りの点がございましたら、[GitHub Issues](https://github.com/mnotgod96/AppAgent/issues)からご報告いただくか、[Dr. Chi Zhang](https://icoz69.github.io/)までメール (dr.zhang.chi@outlook.com) にてご連絡ください。

## 📝 変更履歴
- __[2024.2.8]__: マルチモーダルモデルの代替として`qwen-vl-max` (通義千問-VL)を追加。現在は無料で利用できるが、GPT-4Vと比較するとパフォーマンスは劣る。
- __[2024.1.31]__: AppAgentのテスト時に使用した[評価ベンチマーク](https://github.com/mnotgod96/AppAgent/blob/main/assets/testset.md)をリリース。
- __[2024.1.2]__: エージェントが画面上にグリッドオーバーレイを表示し、画面上の**任意の場所をタップ/スワイプ**できるようにするオプションの方法を追加。🔥
- __[2023.12.26]__: 使いやすくするための[Tips](#tips)セクションを追加。Androidデバイスを持っていないユーザーのために、**Android Studioエミュレーター**の使用方法を追記。
- __[2023.12.21]__: AppAgentを実装するための詳細な設定手順を含むgitリポジトリを公開。 🔥🔥


## 🔆 はじめに

私たちはスマートフォンアプリを操作するための、新しいLLMベースのマルチモーダルエージェントフレームワークを紹介します。

このフレームワークにより、エージェントはタップやスワイプなどの人間に似た操作を模倣した簡略化されたアクション空間を通じて、スマートフォンアプリを操作できるようになります。この革新的なアプローチにより、システムのバックエンドアクセスが不要になり、様々なアプリに対して広く適用できるようになります。 

エージェントの機能の中核となるのは、革新的な学習方法です。エージェントは自律的な探索や人間によるデモンストレーションの観察を通じて、新しいアプリのナビゲーションや使用方法を学習します。このプロセスにより、エージェントが異なるアプリケーション間で複雑なタスクを実行するための知識ベースが生成されます。


## ✨ デモ

デモ動画は、配備段階でAppAgentを使ってX（Twitter）上でユーザーをフォローする過程を示しています。

https://github.com/mnotgod96/AppAgent/assets/40715314/db99d650-dec1-4531-b4b2-e085bfcadfb7

AppAgentがCAPTCHAをパスできる能力を示す興味深い実験。

https://github.com/mnotgod96/AppAgent/assets/27103154/5cc7ba50-dbab-42a0-a411-a9a862482548

数値タグでラベル付けされていないUI要素を見つけるためにグリッドオーバーレイを使用する例。

https://github.com/mnotgod96/AppAgent/assets/27103154/71603333-274c-46ed-8381-2f9a34cdfc53

## 🚀 クイックスタート

このセクションでは、`gpt-4-vision-preview`（または`qwen-vl-max`）をエージェントとして使用し、Androidアプリ上で特定のタスクを完了する方法を説明します。

### ⚙️ ステップ1. 前提条件

1. PCに[Android Debug Bridge](https://developer.android.com/tools/adb)（adb）をダウンロードしてインストールします。これはPCからAndroidデバイスと通信するためのコマンドラインツールです。

2. Androidデバイスを用意し、設定の開発者オプションでUSBデバッグを有効にします。 

3. USBケーブルでデバイスをPCに接続します。

4. （オプション）Androidデバイスをお持ちでないが、それでもAppAgentを試してみたい場合は、[Android Studio](https://developer.android.com/studio/run/emulator)をダウンロードし、付属のエミュレーターを使用することをお勧めします。エミュレーターはAndroid Studioのデバイスマネージャーにあります。インターネットからAPKファイルをダウンロードし、エミュレーターにドラッグ＆ドロップすることで、エミュレーターにアプリをインストールできます。
   AppAgentはエミュレートされたデバイスを検出し、実際のデバイスを操作するのと同じように、そのデバイス上のアプリを操作できます。

   <img width="570" alt="Screenshot 2023-12-26 at 22 25 42" src="https://github.com/mnotgod96/AppAgent/assets/27103154/5d76b810-1f42-44c8-b024-d63ec7776789">

5. このリポジトリをクローンし、依存関係をインストールします。このプロジェクトのすべてのスクリプトはPython 3で書かれているので、インストールされていることを確認してください。

```bash
cd AppAgent
pip install -r requirements.txt
```

### 🤖 ステップ2. エージェントの設定

AppAgentはテキストと視覚的入力の両方を受け取ることができるマルチモーダルモデルを必要とします。私たちの実験では、`gpt-4-vision-preview`をモデルとして使用し、スマートフォン上のタスクを完了するためにどのようなアクションを取るべきかを判断しました。

GPT-4Vへのリクエストを設定するには、ルートディレクトリの`config.yaml`を変更する必要があります。
AppAgentを試すには、2つの重要なパラメータを設定する必要があります。
1. OpenAI APIキー：OpenAIから適格なAPIキーを購入して、GPT-4Vにアクセスできるようにする必要があります。
2. リクエスト間隔：これはGPT-4Vへの連続したリクエスト間の時間間隔（秒）で、GPT-4Vへのリクエストの頻度を制御します。アカウントのステータスに応じてこの値を調整してください。

`config.yaml`の他のパラメータにはコメントが付けられています。必要に応じて変更してください。

> GPT-4Vは無料ではないことに注意してください。このプロジェクトに含まれる各リクエスト/レスポンスペアのコストは約$0.03です。賢明に使用してください。

また、`qwen-vl-max`（通義千問-VL）をAppAgentを動かすための代替マルチモーダルモデルとして使用することもできます。このモデルは現在無料で使用できますが、AppAgentの文脈におけるパフォーマンスはGPT-4Vと比べると劣ります。

使用するには、Alibaba Cloudアカウントを作成し、[Dashscope APIキーを作成](https://help.aliyun.com/zh/dashscope/developer-reference/activate-dashscope-and-create-an-api-key?spm=a2c4g.11186623.0.i1)して`config.yaml`ファイルの`DASHSCOPE_API_KEY`フィールドに記入する必要があります。また、`MODEL`フィールドを`OpenAI`から`Qwen`に変更してください。

独自のモデルを使用してAppAgentをテストする場合は、`scripts/model.py`に新しいモデルクラスを適切に記述する必要があります。

### 🔍 ステップ3. 探索フェーズ

私たちの論文では、GPT-4Vをタスクが与えられたときにAndroidフォンの操作をサポートできる有能なエージェントに変えるため、探索と配備の2つのフェーズを含む新しいソリューションを提案しました。探索フェーズはあなたが与えたタスクから始まり、エージェントにアプリを自律的に探索させるか、あなたのデモンストレーションから学習させることができます。どちらの場合も、エージェントは探索/デモンストレーション中に相互作用した要素のドキュメントを生成し、配備フェーズで使用するために保存します。

#### オプション1: 自律探索

このソリューションは完全に自律的な探索を特徴としており、人間の介入なしに与えられたタスクを試行することでエージェントがアプリの使用法を探索できるようにします。

開始するには、ルートディレクトリで`learn.py`を実行します。プロンプトの指示に従って`autonomous exploration`を操作モードとして選択し、アプリ名とタスクの説明を入力します。その後、エージェントがあなたの代わりに仕事をします。このモードでは、AppAgentは前のアクションを反省し、そのアクションが与えられたタスクに沿っていることを確認し、探索した要素のドキュメントを生成します。

```bash
python learn.py
```

#### オプション2: 人間のデモンストレーションからの学習

このソリューションでは、ユーザーが最初に類似のタスクをデモンストレーションする必要があります。AppAgentはデモから学習し、デモ中に見られたUI要素のドキュメントを生成します。

人間のデモンストレーションを開始するには、ルートディレクトリで`learn.py`を実行する必要があります。プロンプトの指示に従って`human demonstration`を操作モードとして選択し、アプリ名とタスクの説明を入力します。あなたのスマートフォンのスクリーンショットが撮影され、画面に表示されるすべてのインタラクティブな要素に数値タグが付けられます。次のアクションとそのアクションのターゲットを決定するプロンプトに従う必要があります。デモが終了したと思ったら、`stop`と入力してデモを終了します。

```bash
python learn.py
```

![](./assets/demo.png)

### 📱 ステップ4. 配備フェーズ

探索フェーズが終了したら、ルートディレクトリで`run.py`を実行します。プロンプトの指示に従って、アプリの名前を入力し、エージェントに使用してほしい適切なドキュメンテーションベースを選択し、タスクの説明を入力します。その後、エージェントがあなたの代わりに仕事をします。エージェントは、アプリに以前生成されたドキュメンテーションベースがあるかどうかを自動的に検出します。ドキュメンテーションが見つからない場合は、ドキュメンテーションなしでエージェントを実行することもできます（成功率は保証されません）。

```bash
python run.py
```

## 💡 ヒント<a name="tips"></a>
- より良い体験のために、AppAgentに自律探索を通じてより広範なタスクを実行させたり、アプリの機能をより直接的にデモンストレーションしてアプリのドキュメンテーションを強化することができます。一般に、エージェントに提供されるドキュメンテーションが広範であるほど、タスクの完了率が高くなる可能性があります。
- エージェントによって生成されたドキュメンテーションを検査することは、常に良い習慣です。要素の機能を正確に記述していないドキュメンテーションがある場合は、ドキュメンテーションを手動で修正することもオプションの1つです。


## 📊 評価
[評価ベンチマーク](https://github.com/mnotgod96/AppAgent/blob/main/assets/testset.md)をご参照ください。


## 📖 ToDo リスト
- [ ] より多くのLLM APIをプロジェクトに組み込む。
- [x] ベンチマークをオープンソース化する。
- [x] 設定をオープンソース化する。

## 😉 引用
```bib
@misc{yang2023appagent,
      title={AppAgent: Multimodal Agents as Smartphone Users}, 
      author={Chi Zhang and Zhao Yang and Jiaxuan Liu and Yucheng Han and Xin Chen and Zebiao Huang and Bin Fu and Gang Yu},
      year={2023},
      eprint={2312.13771},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```

## スター履歴

[![Star History Chart](https://api.star-history.com/svg?repos=mnotgod96/AppAgent&type=Date)](https://star-history.com/#mnotgod96/AppAgent&Date)


## ライセンス
[MITライセンス](./assets/license.txt)。